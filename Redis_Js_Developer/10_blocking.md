## Redis Blocking Commands 

### Fecha: 21/07/2024

<img src="images/redis.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - Comandos que bloquean al cliente esperando al resultado. Son usados en lugar de hacer polling al server, el polling es un consumer que no es notificado sino que está consultado constantemente en por ejemplo una work queue con una lista en redis. Ineficiente pues muchos llamados y entre más clientes consumiendo, complicado.
  - Si al cliente consumidor, al momento de hacer un llamado por un valor lo bloqueamos entonces solo esperará en ese llamado hasta un resultado. El comando BRPOP es bloquante del RPOP que remueve y regresa el elemento de una lista, acepta el tiempo de bloqueo y si mando 0 sera tiempo indefinido.
- **Código:**
  ```javascript
  const redis = require('redis');
  const bluebird = require('bluebird');

  // Promisify all the functions exported by node_redis.
  bluebird.promisifyAll(redis);

  // Redis key that the producer and consumer will use to share data.
  const key = 'blockingdemo';

  const startConsumer = async () => {
    let retryCount = 0;
    let done = false;

    const consumerClient = redis.createClient({
      port: 6379,
      host: '127.0.0.1',
      // password: 'password',
    });

    while (!done) {
      // Block for up to two seconds waiting on new item to
      // be added to the list.

      /* eslint-disable no-await-in-loop */
      const response = await consumerClient.brpopAsync(key, 2);
      /* eslint-enable */

      if (response === null) {
        console.log('consumer: queue was empty.');
        retryCount += 1;

        if (retryCount === 5) {
          done = true;
          console.log('consumer: shutting down.');
          consumerClient.quit();
        }
      } else {
        // Response is an array of keyname, value.  Example:
        // [blockingdemo, 0]
        console.log(`consumer: popped ${response[1]}.`);
        retryCount = 0;
      }
    }
  };

  const startProducer = async () => {
    let n = 1;

    const producerClient = redis.createClient({
      port: 6379,
      host: '127.0.0.1',
      // password: 'password',
    });

    // Run the producer at one second intervals.
    const producerInterval = setInterval(async () => {
      console.log(`producer: pushing ${n}.`);

      /* eslint-disable no-await-in-loop */
      await producerClient.lpushAsync(key, n);
      /* eslint-enable */

      n += 1;

      // Stop after producing 20 numbers.
      if (n > 20) {
        console.log('producer: shutting down.');
        clearInterval(producerInterval);
        producerClient.quit();
      }
    }, 1000);
  };

  // Start up both the consumer and producer.
  startConsumer();

  // Start producer five seconds later.
  setTimeout(() => {
    startProducer();
  }, 5000);
  ```

## Recursos Adicionales
- [Redis Js Developers course](https://university.redis.com/courses/ru102js/)