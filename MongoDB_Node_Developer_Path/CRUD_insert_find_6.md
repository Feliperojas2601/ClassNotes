## Connection

### Fecha: 16/07/2024

<img src="images/mongo.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - insertOne e insertMany funciones para insertar en una base de datos. Db.collection.insert...({} or [{}, {}]). 
  - Mongo crea la colección si no existe al llamar la función y crea un _id si no se provee.
  - find para buscar documentos, db.collection.find(). Sin nada retorna todos los documentos, usar it para ir por más. 
  - Dentro de las opciones podemos usar el operador $eq como { field: { $eq: valor } } o simplemente { field: valor } para igualaciones. 
  - Para un múltiple eq usamos $in { field : { $in : [valor, valor] } }
  - Tenemos operadores de comparación
    - gt, mayor
    - lt, menor
    - lte, menor igual
    - gte, mayor igual
  - Se usan con { item.field : { $gt : 50 } } 
  - Para obtener el documento que contenga en un array el valor de búsqueda usamos $elemMatch, { products: { $elemMatch: { $eq: valor } } } o { products: { $elemMatch: { field: valor, field1: valor1, ... } } }
  - Tenemos operadores lógicos
    - and
    - or
- Se usan como find({ $and: [{}, {}] }) o find({{}, {}}) para un and simplificiado y si quiero usar un mismo operador más de una vez en una query debo usar explicitamente un and para no sobreescribir. 

## Recursos Adicionales
- [MongoDB Node Developer Path](https://learn.mongodb.com/learn/learning-path/mongodb-nodejs-developer-path)