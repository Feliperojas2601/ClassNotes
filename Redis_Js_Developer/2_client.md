## Cliente

### Fecha: 18/07/2024

<img src="images/redis.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - Manejar conexiones y su ciclo de vida entre cliente y servidor (incluido el pooling), implementar el protocolo de Redis (RESP) sobre TCP sockets y el API según el lenguaje.
  - El cliente emite eventos de ready, end, reconnecting que pueden ser escuchados con una función listener. 
  - El cliente intenta reconexión de manera auto pero se puede implementar una estrategia propia.
  - El conexión pooling es manetener una cantidad de conexiones disponibles para evitar el costo grande de iniciar una nueva conexión, está pensado para aplicaciones multi hilo y múltiples llamados concurrentes. Para NodeJs y Redis no tenemos esta necesidad porque ambos son single thread.

## Recursos Adicionales
- [MongoDB Node Developer Path](https://learn.mongodb.com/learn/learning-path/mongodb-nodejs-developer-path)
