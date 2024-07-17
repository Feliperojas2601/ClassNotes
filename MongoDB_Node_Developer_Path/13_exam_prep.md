## CRUD - Exam Prep

### Fecha: 17/07/2024

<img src="images/mongo.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - Para obtener todos los documentos de un cursos podemos usar find seguido de toArray(), find() retorna un cursos con elementos en pantalla y el toArray los podría alamacenar en una variable array después.
  - Una colección tiene documentos con un campo name y quiero hacer un indice de autocompletado y debo tokenizar lo que hay en esos valores de nombre para que busque resultados por el inicio de palabras en el campo name. Una tokenización con regex hace expresiones regulares útiles para coincidencias complejas, un edgeGram usa tokens a partir del inicio de la palabra, un nGram genera una subsecuencia de los caracteres de las palabras, útil para coincidencias sobre cualquier palabra y matchGram es más general. En general para buscar coincidencias con la palabra inicial usar el edgeGram.
  - Para encontrar algo por su nombre el indice search de búsqueda atlas se usa search > text > path para dar el nombre del field y el query a buscar. 
  - getSiblingDb(name) es un método que puede ser llamado desde X db. y ejecutar cosas sobre la otra db name. 
  - Al crear un indice multikey sobre un arreglo con documentos, si la query se hace sobre documents.field el indice que cubre mejor es por tanto documents.field, no sobre todo documents y no redudante documents: 1 y documents.field: 1
  - Mongo client tiene métodos db(), close(), no tiene métodos open() ni destroy()
  - Usar connection pooling en el driver reduce latencia y limita el número de conexiones, no remueve auth y remueve la necesidad de abrir o cerrar conexiones.
  - Si tengo una query de ordenación, los indices que me cubren completo el rendimiento son field: 1 y field: -1 con compuestos claro está.
  - En un ecommerce, guarde el inventario embeed en los productos, las reviews ni las ordenes no, no se accede junto.
- **Código:**
  ```javascript
  {
    "mappings": {
      "dynamic": false,
      "fields": {
        "name": [
          {
            "type": "autocomplete",
            "tokenization": "edgeGram"
          }
        ]
      }
    }
  }
  db.restaurants.aggregate([
    {
      "$search": {
        "text": {
          "path": "name",
          "query": "cuban"
        }
      }
    }
  ])
  ```

## Recursos Adicionales
- [MongoDB Node Developer Path](https://learn.mongodb.com/learn/learning-path/mongodb-nodejs-developer-path)