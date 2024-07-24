## Introducción - RabbitMQ

### Fecha: 23/07/2024

<img src="images/rabbit.png" alt="Gráfico de Introducción" width="150">

- **Notas:**
  - RabbitMQ es un message broker escrito en Erlang, componente de software intermediario que permite comunicación entre dos servicios con producción y consumo de mensajes.
  - CloudAMPQ hostea RabbitMQ y lo ofrece como servicio.
  - El encolamiento de mensajes o message queuing permite la comunicación mediante el envío de mensajes y el guardado de mensajes cuando el destino está desconectado. Sus componentes son el producer, el broker, el msg y el consumer. El broker puede tener una o muchas colas. 
  - Una cola es una línea de cosas esperando a ser atendidas, funciona con FIFO. Un mensaje es cualquier tipo de información que se desea transmitir.
  - Cuando usamos un message broker estamos haciendo comunicación entre componetes ASYNC.
  - Algunas ventajas de usar un broker: 
    - Hace sencillo desacoplar el sistema y puede actuar como el orquestador/comunicador de los componentes del mismo.
    - Transferencia confiable de información con un storage temporal en las colas y la posibilidad de reenvío desde la cola.
    - Descarga o offload de la base de datos con reintentos y tolerancia a fallos.
    - El sistema es escalable al añadir más consumidores capaces de procesar más de los producers a tope.
    - Resiliente y tolerante al desacoplar servicios entonces el fallo de uno no tumba todo.
    - Establece async en operaciones.
  - El protocolo que usa RabbitMQ se llama AMQP, advanced message queuing protocol, es el éstandar de comunicación entre dispositivos electronicos, ¿Cómo se mandan los datos? Define reglas para eso. En este protocolo definimos la conexión entre el producer/consumer al broker, el canal que es la representación virtual de la conexión física y que es más ligera para operar, todos los mensajes se mandan a través de un canal, una sola conexión puede tener múltiples canales. Los mensajes no son directamente publicados sobre la cola, el producer los manda a través de un Exchange, que se encarga de ubicarlo en la cola correcta a través de una routing key y un binding entre cola y exchange. El mensaje tiene headers con su durabilidad, prioridad. Finalmente, los comandos de acknowledging permiten determinar la confirmación de entregas. Los vhost permiten separar entornos de exchange-bind-queue dentro del broker.
  - En la UI podemos, añadir/eliminar colas en la sección de queues, añadir/eliminar exchanges y binding, publicar mensajes en la sección de exchanges.
- **Código:**
  ```javascript
  ```

## Recursos Adicionales
- [RabbitMQ certification course](https://training.cloudamqp.com/course)
