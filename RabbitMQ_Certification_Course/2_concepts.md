## RabbitMQ Concepts

### Fecha: 24/07/2024

<img src="images/rabbit.png" alt="Gráfico de Introducción" width="150">

- **Notas:**
  - Rabbitmq implementa una extensión del AMPQ 0.9.1 del 2008, application layer protocol.
  - El protocolo es binario, compacto porque todo lo que pasa es binario y optimizado para máquinas. 
  - El protocolo define el model specification layer y el network layer protocol. Está sobre TCP/IP.
  - Conexión, link entre cliente y broker que hace tareas de networking como auth, ip resolution y networking como tal. Soporta IPv4/6 y TLS. 
  - Una conexión virtual ligera es el canal, pueden tener muchos. El canal reusa la conexión y evita reauth y otros procesos y luego abre otro stream TCP para hacer operaciones de protocolo.  
  - Una cola es el lugar en donde los mensajes son almacenados hasta que son consumidos. Propiedades: 
    - Tienen un nombre declarado o uno por defecto.
    - Durable, si la cola sobrevive luego de un restart del broker.
    - Exclusive, usada solo por una conexión y eliminada cuando se cierra.
    - Auto-delete, sin consumidores se elimina.
    - TTL, cola expira luego de un período de tiempo, 
  - El ciclo de vida de una cola es, el cliente declara la cola, el servidor confirma, el cliente inicia un consumo en la cola, el cliente cierra el consumo, la cola se elimina.
  - El exchange es el primer punto de entrada para un msg en el broker, que funciona como un agente de enrutamiento hacia la cola respectiva usando headers, bindings y routing-keys. También tiene propiedades de durable, temporary, auto-delete.
  - El ciclo de vida de un exchange es, el cliente pregunta al servidor por la existencia de un exchange o lo declara, el cliente publica un msg al exchange, el cliente según la config elimina el exchange. 
  - Los tipos de exchange son direct, topic, fanout y headers. El tipo directo compara igualda en la routing key, el fanout compara key-routing patterns, fanout envía a todas las colas sin importar el routing-key, headers es parecido a topic pero usa headers en lugar de la routing key.
  - Un binding es una asociación entre cola y exchange. Usan la routing_key.
  - Los acknowledgments permiten la retransmisión de msgs al no ser transmitidos, el cliente decide cuando mandar el ack. Podemos considerar algo enviado desde que sale (publisher confirm) o desde que es escrito o desde que explicitamente se recibe el ack. El msg se decarta de la cola, el field de requeue permite reenviar a la misma posición el msg, ojo con los bucles.
  - Los Vhosts segregan aplicaciones dentro de la misma instancia de RabbitMQ. Los recursos anteriores todos están sobre un vhost. Los usuarios son asignables con permisos a los vhost. Entre vhosts no se comparten cosas excepto recurso físico y por tanto pueden afectarse el rendimiento.
- **Código:**
  ```javascript
  ```

## Recursos Adicionales
- [RabbitMQ certification course](https://training.cloudamqp.com/course)
