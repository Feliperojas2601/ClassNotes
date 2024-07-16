## CRUD - Update and Delete

### Fecha: 16/07/2024

<img src="images/mongo.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - Reemplaza documentos, usamos replaceOne(filter, replacement, options), replacement es un documento.
  - Actualizar un documento con updateOne(filter, update, create) update es una query de actualización a realizar. 
  - Los operadores $set y $push, set añade fields o reemplaza con un valor mientras que push appends el valor a un array o añade un field array con el valor como elemento. 
  - La opción upsert crea un documento si el filter no hizo match.
  - findAndModify({query: {}, update: {}, new:false/true}) se usa para actualizar y entregar el documento actualizado updateOne + findOne. Con el new true se devuelve el documento con las modificaciones.
  - updateMany(filter, update, options) para actualizar varios. No es átomico, si falla ya tendremos algunos actualizados.
  - deleteOne(filter, options) y deleteMany(filter, options) para eliminar documentos, entregan un deletedCount y un ack.
- **Código:**
  ```javascript
  db.books.replaceOne(
    {
      _id: ObjectId("6282afeb441a74a98dbbec4e"),
    },
    {
      title: "Data Science Fundamentals for Python and MongoDB",
      isbn: "1484235967",
      publishedDate: new Date("2018-5-10"),
      thumbnailUrl:
        "https://m.media-amazon.com/images/I/71opmUBc2wL._AC_UY218_.jpg",
      authors: ["David Paper"],
      categories: ["Data Science"],
    }
  )
  db.podcasts.updateOne(
    {
      _id: ObjectId("5e8f8f8f8f8f8f8f8f8f8f8"),
    },
    {
      $set: {
        subscribers: 98562,
      },
    }
  )
  db.podcasts.updateOne(
    { _id: ObjectId("5e8f8f8f8f8f8f8f8f8f8f8") },
    { $push: { hosts: "Nic Raboy" } }
  )
  db.podcasts.updateOne(
    { title: "The Developer Hub" },
    { $set: { topics: ["databases", "MongoDB"] } },
    { upsert: true }
  )
  db.podcasts.findAndModify({
    query: { _id: ObjectId("6261a92dfee1ff300dc80bf1") },
    update: { $inc: { subscribers: 1 } },
    new: true,
  })
  db.books.updateMany(
    { publishedDate: { $lt: new Date("2019-01-01") } },
    { $set: { status: "LEGACY" } }
  )
  db.podcasts.deleteOne(
    { _id: Objectid("6282c9862acb966e76bbf20a") }
  )
  db.podcasts.deleteMany({category: “crime”})
  ```

## Recursos Adicionales
- [MongoDB Node Developer Path](https://learn.mongodb.com/learn/learning-path/mongodb-nodejs-developer-path)