## CRUD - Insert and Find

### Fecha: 16/07/2024

<img src="images/mongo.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - insertOne e insertMany funciones para insertar en una base de datos. Db.collection.insert...({} or [{}, {}]). 
  - Mongo crea la colección si no existe al llamar la función y crea un _id si no se provee.
  - find y findOne para buscar documentos, db.collection.find(). Sin nada retorna todos los documentos, usar it para ir por más. 
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
- **Código:**
  ```javascript
  db.grades.insertOne({
    student_id: 654321,
    products: [
        {
        type: "exam",
        score: 90,
        },
        {
        type: "homework",
        score: 59,
        },
        {
        type: "quiz",
        score: 75,
        },
        {
        type: "homework",
        score: 88,
        },
    ],
    class_id: 550,
  })
  db.grades.insertMany([
    {
        student_id: 546789,
        products: [
        {
            type: "quiz",
            score: 50,
        },
        {
            type: "homework",
            score: 70,
        },
        {
            type: "quiz",
            score: 66,
        },
        {
            type: "exam",
            score: 70,
        },
        ],
        class_id: 551,
    },
    {
        student_id: 777777,
        products: [
        {
            type: "exam",
            score: 83,
        },
        {
            type: "quiz",
            score: 59,
        },
        {
            type: "quiz",
            score: 72,
        },
        {
            type: "quiz",
            score: 67,
        },
        ],
        class_id: 550,
    },
    {
        student_id: 223344,
        products: [
        {
            type: "exam",
            score: 45,
        },
        {
            type: "homework",
            score: 39,
        },
        {
            type: "quiz",
            score: 40,
        },
        {
            type: "homework",
            score: 88,
        },
        ],
        class_id: 551,
    },
  ])
  db.zips.find({ _id: ObjectId("5c8eccc1caa187d17ca6ed16")})
  db.zips.find({ city: { $in: ["PHOENIX", "CHICAGO"] }})
  db.sales.find({ "items.price": { $gt: 50}})
  db.sales.find({
    items: {
        $elemMatch: { name: "laptop", price: { $gt: 800 }, quantity: { $gte: 1 } },
    },
  })
  db.routes.find({
    $and: [
        { $or: [{ dst_airport: "SEA" }, { src_airport: "SEA" }] },
        { $or: [{ "airline.name": "American Airlines" }, { airplane: 320 }] },
    ]
  })
  ```

## Recursos Adicionales
- [MongoDB Node Developer Path](https://learn.mongodb.com/learn/learning-path/mongodb-nodejs-developer-path)