## Introducción - Redis

### Fecha: 18/07/2024

<img src="images/redis.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - node_redis es el paquete de npm utilizado oficialmente. Importar redis, e inicializar el cliente con el host el puerto, tls para transporte seguro. 
  - client.set para establecer una llave-valor, client.set(key, value, (err, response) => {});
  - client.get(key, (err, response) => {}); 
  - client.quit() cierra la conexión al terminar los comandos pendientes.
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
  ```

## Recursos Adicionales
- [MongoDB Node Developer Path](https://learn.mongodb.com/learn/learning-path/mongodb-nodejs-developer-path)
