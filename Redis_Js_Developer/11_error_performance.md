## Redis Error Handling y Performance

### Fecha: 21/07/2024

<img src="images/redis.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - Errores por invocar mal funciones, conexión, etc.
  - Error object de invocación tiene propiedad name, message, command, args, code.
  - En pipelines, en el array de responses tendremos el error, no vamos a tener una rejection del método async (promesa), por tanto no vale esperar en el catch el error.
  - Podemos implementar en el createClient en la prop de opciones retry_strategy una función que utilice options.attempt para una lógica que retorne un error para dejar de intentar o sino el número de segundos que debe esperar antes de una reconexión.
  - El performance es muy importante, redis está diseñada pra sub-milisecond data access. Muy atento a la latencia, el coste de los comandos de Redis o complejidad y el bloqueo y la atomicidad.
  - Latencia: Use pipelining para correr más de un comando sin necesitar respuestas en el medio, reduciendo el número de viajes y el cambio de contexto.
  - Complexity: Puede ver la complejidad de cada comando en la documentación de redis. Ojo con O(n)
- **Código:**
  ```javascript
  const redis = require('redis');
  const bluebird = require('bluebird');

  // Promisify all the functions exported by node_redis.
  bluebird.promisifyAll(redis);

  const replyErrorExamples = async () => {
    const client = redis.createClient({
      port: 6379,
      host: '127.0.0.1',
      // password: 'password',
    });

    const key = 'replyErrorTest';

    // No error, this command will work.
    await client.setAsync(key, 'test');

    // Wrong number of arguments for command.
    // Show this without try/catch for unhandled promise rejection...
    try {
      console.log('Calling set with missing parameter.');
      await client.setAsync(key);
    } catch (setErr) {
      console.log(setErr);
    }

    console.log('----------');

    // No error, this command will work.
    await client.setAsync(key, 'test');

    try {
      console.log('Incrementing a key that holds a string value.');
      await client.incrAsync(key);
    } catch (incrErr) {
      console.log(incrErr);
    }

    console.log('----------');

    // Error in pipeline... (transaction produces same behavior).
    try {
      console.log('Bad command as part of a pipeline.');
      const pipeline = client.batch();
      // This command will succeed.
      pipeline.set(key, 'test');

      // This command will fail.
      pipeline.incr(key);

      // This command will succeed.
      pipeline.get(key);

      const results = await pipeline.execAsync();

      console.log('Results:');
      console.log(results);
    } catch (pipelineErr) {
      // The error will not appear here.
      console.log('Caught pipeline error:');
      console.log(pipelineErr);
    }

    console.log('----------');

    client.quit();
  };

  try {
    replyErrorExamples();
  } catch (e) {
    console.log('Caught exception:');
    console.log(e);
  }

  const redis = require('redis');
  const bluebird = require('bluebird');

  // Promisify all the functions exported by node_redis.
  bluebird.promisifyAll(redis);

  const connectionErrorExample = async () => {
    try {
      const client = redis.createClient({
        port: 6379,
        host: '127.0.0.1',
        // password: 'password',
        retry_strategy: (options) => {
          if (options.attempt > 5) {
            return new Error('Retry attempts exhausted.');
          }

          // Try again after a period of time...
          return (options.attempt * 1000);
        },
      });

      client.on('connect', () => {
        console.log('Connected to Redis.');
      });

      client.on('reconnecting', (o) => {
        console.log('Attempting to reconnect to Redis:');
        console.log(o);
      });

      client.on('error', (e) => {
        console.log('Caught error in handler:');
        console.log(e);
      });

      const key = 'connectionTest';

      const response = await client.setAsync(key, 'hello');
      console.log(`SET response: ${response}`);
    } catch (err) {
      console.log('Caught an error:');
      console.log(err);
    }
  };

  try {
    connectionErrorExample();
  } catch (err) {
    console.log('Caught exception:');
    console.log(err);
  }
  ```

## Recursos Adicionales
- [Redis Js Developers course](https://university.redis.com/courses/ru102js/)