## CRUD - Index

### Fecha: 17/07/2024

<img src="images/mongo.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - Son una estructura de datos especial que almacenan una pequeña porción de data, están ordenadaos para buscar de manera eficiente y apuntan a la identidad del documento.
  - Se usan para mejorar consultas, velocidad, reducir tamaño en disco y recursos. 
  - Sin indices tocaría leer todos los documentos, con el indice solo se traen los documentos identificados con el indice que está manejando la consulta. 
  - Solo se crea por defecto el _id. 
  - Toda query debería usar un indice. 
  - Ojo porque al hacer operaciones de escritura debemos actualizar la estructura del indice, vamos a volver la operación más pesada. Debemos asegurarnos que todos los indices están siendo usados. 
  - Los tipos más comunes son single field, compound field y multikey que operan sobre fields arrays. 
  - Si tengo una consulta que busca una cuenta de banco activa debería hacer un indice compuesto con el número de cuenta y el active. 
  - Para crear un indice single usamos db.coll.createIndex({field: 1}) el 1 es ASC pero sirve para clasificar ASC o DESC, en single no tiene relevancia. Si el field es único podemos especificar en createIndex({}, {unique:true}), ayuda e impide repetición. 
  - Usando db.coll.getIndexes() obtengo los indices que están operando. Utilizando el explain de una query puedo ver si se está usando algún indice. db.coll.explain().find(...) y obtendré en la respuesta el winningPlan en donde saldrá el indice. 
  - Para los array, se usan los index multikey que puede ser a su vez single o compound y los elementos puedes ser primitivos, documentos o sub arrays. Solo podemos hacer un array field por index.
  - En los indices compuestos, el primer campo es el prefijo y una query de solo un campo que tenga ese prefijo lo va a utilizar, pero para los siguiientes fields no lo van a utilizar porque no son el prefijo. Esto porque el orden en la estructura importa. Siga el orden igualdad (prueban matches exactos y filtran), orden (determinan el orden según un parámetro y evitan ordenamiento en memoria) y rango al crear un indice con varios fields.
  - En los indices de sort, importa el orden de clasificación 1 o -1.
  - Importante, si mi query projecta campos que no están en el indice podemos agregarlos al final del indice para evitar una proyección extra en el plan. Cubrimiento total.
  - Para eliminar indices no utilizados o redundates, sino ocultarlo con db.coll.hideIndex(name) porque recrearlo toma tiempo y recursos. Si ya es seguro que no es útil entonces, db.coll.dropIndex(name) o dropIndexes([]), eliminar todos sin el [].
- **Código:**
  ```javascript
  db.customers.createIndex({
    email: 1
  },
  {
    unique:true
  })
  db.customers.getIndexes()
  db.customers.explain().find({
  birthdate: {
    $gt:ISODate("1995-08-01")
    }
  }).sort({
    email:1
  })
  db.customers.createIndex({
    active:1, 
    birthdate:-1,
    name:1
  })
  db.customers.find({
    birthdate: {
      $gte:ISODate("1977-01-01")
    },
    active:true
  }).sort({
    birthdate:-1, 
    name:1
  })
  db.customers.dropIndex(
    'active_1_birthdate_-1_name_1'
  )
  ```

## Recursos Adicionales
- [MongoDB Node Developer Path](https://learn.mongodb.com/learn/learning-path/mongodb-nodejs-developer-path)