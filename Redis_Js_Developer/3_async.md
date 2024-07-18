## Async Redis

### Fecha: 18/07/2024

<img src="images/redis.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - Tenemos el manejo por callbacks, promesas o async/await.
- **Código:**
  ```javascript
  const redis = require('redis');

  // Create a client and connect to Redis.
  const client = redis.createClient({
    host: 'localhost',
    port: 6379,
    // password: 'password',
  });

  // Run a Redis command, receive response in callback.
  client.set('hello', 'world', (err, reply) => {
    console.log(reply); // OK

    // Run a second Redis command now we know that the
    // first one completed.  Again, response in callback.
    client.get('hello', (getErr, getReply) => {
      console.log(getReply); // world

      // Quit client and free up resources.
      client.quit();
    });
  });

  const redis = require('redis');
  const { promisify } = require('util');

  // Create a client and connect to Redis.
  const client = redis.createClient({
    host: 'localhost',
    port: 6379,
    // password: 'password',
  });

  // Use Node's built in promisify to wrap the Redis
  // command functions we are going to use in promises.
  const setAsync = promisify(client.set).bind(client);
  const getAsync = promisify(client.get).bind(client);

  // Chain promises together to call Redis commands and
  // process the results.
  setAsync('hello', 'world')
    .then(res => console.log(res)) // OK
    .then(() => getAsync('hello'))
    .then(res => console.log(res)) // world
    .then(() => client.quit());

  const redis = require('redis');
  const bluebird = require('bluebird');

  // Make all functions in 'redis' available as promisified
  // versions whose names end in 'Async'.
  bluebird.promisifyAll(redis);

  // When using 'await', code needs to be in a function that
  // is declared 'async', so our code is wrapped in here and
  // called at the bottom of the script.
  const runApplication = async () => {
    // Connect to Redis.
    const client = redis.createClient({
      host: 'localhost',
      port: 6379,
      // password: 'password',
    });

    // Run a Redis command.
    const reply = await client.setAsync('hello', 'world');
    console.log(reply); // OK

    const keyValue = await client.getAsync('hello');
    console.log(keyValue); // world

    // Clean up and allow the script to exit.
    client.quit();
  };

  try {
    runApplication();
  } catch (e) {
    console.log(e);
  }
  ```

## Recursos Adicionales
- [MongoDB Node Developer Path](https://learn.mongodb.com/learn/learning-path/mongodb-nodejs-developer-path)
