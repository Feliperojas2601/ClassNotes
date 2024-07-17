## CRUD - Aggregation

### Fecha: 16/07/2024

<img src="images/mongo.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - Framework para consultas de múltiples pasos. La agregación es el análisis y resumen de data. Un stage es una operación/etapa de agregación realizada sobre la data. 
  - Un pipeline de agregación es una serie de stages que se van completando en orden una a la vez. 
  - db.collection.aggregate([{$stage_name: {}}, {...}])
  - Algunos stages comúnes son: 
    - $match: Filtra data que cumple un criterio 
    - $group: Agrupa según criterio 
    - $sort: Ordena 
  - La sintaxis depende de cada etapa.
  - El $ en los nombres de campo hace referencia a valores en el field. $username = 'Felipe'
  - $match: toma un argumento, una consulta como en find, debemos colocarlo lo más pronto posible en el pipeline para aprovechar los indices y reducir costos. 
  - $group: toma como argumento la group key en _id: y un field al cuál hacerle un acumulador. 
  - $sort: ordena resultados por un campo, 1 para ASC y -1 para DESC. Espera un field y un 1/-1  
  - $limit: limita el número de resultados, espera un número. 
  - $project: determina el tamaño de salida pues podemos especificar campos que necesitamos nuevos o existentes. Debría ser la última etapa. No hay necesidad usarlo antes pues MongoDB solo utiliza los campos necesarios. Pude ser de inclusión o exclusión con 1 o 0, si son campos nuevo podemos especificar nuevos valores, también para los existentes. 
  - $set: añade o modifica fields en el pipeline. Cambiar campos existentes o agregar nuevos sin tener que especificar todos de nuevo. 
  - $count: cuenta el número de elementos y los devuelve en un solo documento con el field del nombre especificado como parámetro.
  - $out: guarda los documentos de retorno en una nueva colección, si ya existe reemplaza. Es un stage que debe ir al final. Recibe db: y coll: o puede ser solo $out: "new-coll" y usa la misma db.
- **Código:**
  ```javascript
  db.collection.aggregate([
      {
          $stage1: {
              { expression1 },
              { expression2 }...
          },
          $stage2: {
              { expression1 }...
          }
      }
  ])
  db.zips.aggregate([
    {   
      $match: { 
          state: "CA"
        }
    },
    {
      $group: {
          _id: "$city",
          totalZips: { $count : { } }
      }
    }
  ])
  db.zips.aggregate([
    {
      $sort: {
        pop: -1
      }
    },
    {
      $limit:  5
    }
  ])
  {
    $project: {
        state:1, 
        zip:1,
        population:"$pop",
        _id:0
    }
  }
  { 
    $set: {
        place: {
            $concat:["$city",",","$state"]
        },
        pop:10000
     }
  }
  {
    $count: "total_zips"
  }
  db.sightings.aggregate([
    {
      $match: {
        date: {
          $gte: ISODate('2022-01-01T00:00:00.0Z'),
          $lt: ISODate('2023-01-01T00:00:00.0Z')
        }
      }
    },
    {
      $out: 'sightings_2022'
    }
  ])
  ```

## Recursos Adicionales
- [MongoDB Node Developer Path](https://learn.mongodb.com/learn/learning-path/mongodb-nodejs-developer-path)