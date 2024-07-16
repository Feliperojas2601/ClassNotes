## CRUD - Modify Query Results

### Fecha: 16/07/2024

<img src="images/mongo.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - Ordenamiento cursor.sort() y limite cursor.limit()
  - Un cursor es un apuntamiento al conjunto de resultados de una consulta
  - Métodos sobre los resultados cursor.method()
  - En .sort({field: 1/-1}) ASC/DESC. En limit(número)
  - Para retornar solamente los campos que quiero retornar usaremos un projection. Luego de la query en el find, en las opciones mandamos {field: 1} para proyectar ese campo y usando 0 eliminamos el _id, no podemos usar 1 y 0 si no es del _id.
  - Conteo de documentos con la función countDocuments(query, options).
- **Código:**
  ```javascript
  // Return data on all music companies, sorted alphabetically from A to Z. Ensure consistent sort order
  db.companies.find({ category_code: "music" }).sort({ name: 1,_id: 1 });
  db.companies.find({ category_code: "music" })
    .sort({ number_of_employees: -1, _id: 1 })
    .limit(3)
  db.inspections.find(
    { sector: "Restaurant - 818" },
    { business_name: 1, result: 1, _id: 0 }
  )
  db.trips.countDocuments({ tripduration: { $gt: 120 }, usertype: "Subscriber" })
  ```

## Recursos Adicionales
- [MongoDB Node Developer Path](https://learn.mongodb.com/learn/learning-path/mongodb-nodejs-developer-path)