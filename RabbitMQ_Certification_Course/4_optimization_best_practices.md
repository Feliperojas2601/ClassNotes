## RabbitMQ UI

### Fecha: 25/07/2024

<img src="images/rabbit.png" alt="Gráfico de Introducción" width="150">

- **Notas:**
  - Entender cómo optimizar el prefetch count maximiza la velocidad del sistema. Este valor determina cuántos msgs se pueden enviar al mismo tiempo hacia los consumidores. Por defecto, se pueden enviar la mayor cantidad posible a un consumido listo para aceptar. Las librerías cachean estos msgs en el consumidor. Hay dos opciones de prefecth, channel y consumer. Channel define el valor máx de unack permitidas, limitando la recepción de msgs en el broker antes de tener acks. Como un canaal puede tener varias colas lo mejor es usar el consumer prefetch. Se configura en la variable global de la lib, false es consumer, true es channel. Prefecth pequeños son ideales para distribuir en sistemas grandes y con 1 tenemos distribución igualiataria, pero esto puede afectar el performace pues se espera un round trip (deliver, process, ack) para seguir enviando. Y uno muy grande puede enviar todo a un mismo consumer. 
  - Si hay pocos consumidores y procesan rápido, es útil prefetchar muchos mensajes para mantener el cliente ocupado. Con muchos consumidores y procesamiento rápido, un valor de prefetch más bajo es mejor para evitar que algunos consumidores queden inactivos. En casos con muchos consumidores o tiempos de procesamiento largos, se sugiere establecer el prefetch a 1 para distribuir equitativamente los mensajes. Evita configuraciones de prefetch ilimitadas, ya que pueden causar problemas de memoria y fallos del cliente. Para calcular un valor óptimo de prefetch en RabbitMQ, divide el tiempo total de ida y vuelta de los mensajes por el tiempo de procesamiento de cada mensaje. Esto ayuda a mantener a los consumidores ocupados de manera eficiente. Si tienes pocos consumidores con tiempos de procesamiento constantes, esta fórmula puede dar un buen valor de prefetch. Sin embargo, ajusta el prefetch según el número de consumidores y sus tiempos de procesamiento para evitar que algunos consumidores queden inactivos o que un solo consumidor maneje demasiados mensajes, lo que podría causar problemas de memoria.
  - Dead lettering, cuando un msg es inmanejable por ejemplo cuando el tiempo en cola excede el TTL o cuando se excede capacidad o cuando es negativamente ack (nack) por el consumer. Para esto está el dead letter exhange y el dead letter queue que recolectan msgs eliminados por alguna razón por la cola principal. Esta estrategia de dos colas sirve para otras cosas también, como reprogramar el consumo de msgs usando el ttl.
  - Alternate exchange, para cuando se usan routing key que no existen y no perder información, el alternate exhange se añade a uno o más exchanges. Esto depende del mandatory header que lleve el msg en true o se borraran.
  - Rabbitmq tiene muchos plugins que dan más funcionalidades, algunos de estos son: 
    - La UI de rabbitmq es el más importante y usado. 
    - Shovel para balanceo de carga de colas o para sacar msgs de un broker a otro. 
    - Federation para transferir msgs de un broker a otro. 
    - Message timestamp que añade un timestamp cuando entra un msg. 
    - Consistent hash exchange type para distribución de msgs según el hash computado.
    - Sharding que permite crecer el número de msgs mediante distribución de colas entre múltiples nodos. 
    - Priority queues, msgs manejados según importancia. 
    - Delayed message exhange para retrasar el manejo de mensajes durante x tiempo.
    - Messaging protocols para usar diferentes protocolos.
      - mqtt 
      - stomp
      - http 
      - webstomp 
      - websockets
  - Buenas prácticas: 
    - Quorum queues 
    - Mantener las colas cortas 
    - Lazy queues para long queues 
    - Limit queue size con size o time
    - Considere el número de colas, no demasiadas
    - Separe las colas en diferentes núcleos o nodos 
    - Nombre las colas 
    - Auto elimine colas que no usan 
    - Ojo con las colas de prioridad porque consumen mucho recurso
    - Considere el payload y su tamaño por el performance 
    - No use muchas conexiones o canales 
    - No comparta canales entre hilos 
    - No abra y cierre conexiones o canales repetidamente 
    - Separe conexiones del pub y el consumer 
    - Use TLS 
    - Haga el ack según el caso de uso, manual si es realmente necesario
    - Limite unack msgs 
    - Use msgs persistentes y colas durables para mantener la info y auto delete para performance 
    - Considere el prefetch y el exchange type 
    - Solo instale plugins que va a usar 
- **Código:**
  ```javascript
  channel.exchange_declare("dlx_exchange", "direct")
  channel.queue_declare("test_queue", arguments={ "x-dead-letter-exchange": "dlx_exchange", "x-dead-letter-routing-key": "dlx_key"})

  conn_str = os.environ["AMQP_URI"]
  params = pika.URLParameters(conn_str)
  connection = pika.BlockingConnection(params)
  channel = connection.channel()

  channel.exchange_declare("alt_exchange", "fanout")
  channel.queue_declare("alt_queue")
  channel.queue_bind("alt_queue", "alt_exchange")
  ```

## Recursos Adicionales
- [RabbitMQ certification course](https://training.cloudamqp.com/course)
