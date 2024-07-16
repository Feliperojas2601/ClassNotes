## Connection

### Fecha: 16/07/2024

<img src="images/mongo.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - Se usa una cadena de conexión, describe el host y las opciones de conexión. 
  - Formato estándar o DNS, estándar para standalone clústers, replica sets o shared clústers. DNS da una lista de servidores, es más flexible para desplegar y para cambiar/rotar servers sin reconfigurar. 
  - En Atlas se encuentra en Database, connect.
  - Comienza con el prefijo mongodb, se le puede añadir +srv para establecer TLS.
  - :// Le siguen username:password.
  - Siguen el @host y el puerto opcional o 27017 por defecto.
  - Por último tendremos /? las opciones de conexión.
  - MongoDB Shell es un entorno REPL de NodeJs (Read-Evaluate-Print-Loop), como un entorno pequeño para operar con la base de datos conectada, tenemos variables, funciones, etc de Js. Nos conectamos con el string de conexión que nos da Atlas y el password de Admin.
  - Errores de conexión y credenciales son los más comúnes, generalmente por la IP no agregada en Atlas o contraseña.
  - mongodb+srv://myDatabaseUser:D1fficultP%40ssw0rd@cluster0.example.mongodb.net/?retryWrites=true&w=majority

## Recursos Adicionales
- [MongoDB Node Developer Path](https://learn.mongodb.com/learn/learning-path/mongodb-nodejs-developer-path)