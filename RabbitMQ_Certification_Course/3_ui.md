## RabbitMQ UI

### Fecha: 24/07/2024

<img src="images/rabbit.png" alt="Gráfico de Introducción" width="150">

- **Notas:**
  - Interfaz de usuario para monitoreo y operación desde un navegador. 
  - En el overview encontramos dos gráficas, una para mensajes encolados y otra para el message rate. En la de msgs encolados tenemos el total: la cantidad de msgs para ser entregados y el unacked: la cantidad de msgs para los cuales se espera el ack. El msg rate muestra la tasa de que tan rapido se manejan los msgs. En global count tenemos el número total de conexiones, colas, exchanges, etc.
  - Nodos, un clúster de RabbitMQ puede tener varios nodos y su info como procesos o memoria se ve en node view.
  - En churn tenemos las statas de opening/closure de conexión y canales. También tenemos los puertos. Podemos exportar e importar definiciones de configuración JSON.
  - En conexiones tenemos estsos estados Starting, Tuning, Opening, Running, Flow, Blocking, Blocked, Closing, Closed. Aquí podemos ver número de canales, username, vhost, ssl/tls activado. Misma info para canales.
  - En exchanges podemos ver el typo, vhost, parámetros y añadir bindigs, eliminar exchange o publicar mensajes.
  -  En las colas vemos la col features con los params de la cola. Podemos crear colas y las charts de overview pero especificas para la cola. Podemos ver consumers y canales pegados.
- **Código:**
  ```javascript
  ```

## Recursos Adicionales
- [RabbitMQ certification course](https://training.cloudamqp.com/course)
