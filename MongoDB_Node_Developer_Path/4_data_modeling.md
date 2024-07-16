## Data Modeling

### Fecha: 16/07/2024

<img src="images/mongo.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - Data modeling es el proceso de definir como la data se almacena y las relaciones entre entidades. Esta organización es el schema. 
  - Debe pensarse en la aplicación, qué hace? Qué almaceno? Cómo acceden los usuarios? Qué es importante? Esto optimiza CPU, memoria, queries, manejo, costos. 
  - MongoDB tiene un principio, data que es accedido de manera junta debe ser almacenada junta.
  - Documento model embebido permite construir relaciones complejas de datos. Mejor normalizar usando referencias la data que no es accedida junta, pensar en cómo el usuario accede a los datos, no es el mismo modelamiento relacional.
  - Evitar las consultas entre colecciones cruzadas para responder por una query.

  - Tipos de relación, 1-1 1-m, m-m.
    - 1-1: relación en un mismo documento. 
    - 1-m: Propiedad es un arreglo en el documento. 
    - m-m. 
  - Podemos o embeed poniendo el documento completo dentro del documento padre o pasar por referencia los ids.
  - Embeed para 1-m o m-m, simplificar queries y mejorar rendimiento. 
  - Nested documents. Muy accurate al principio de MongoDB. 
  - Ojo con crear documents muy largos, data duplicada, slow performance, añadir sin límites (unbound), 16MB máx.
  - Reference, dos colecciones diferentes pero con relación, guardar el _id de un documento en otro. Es normalización. Evita duplicados y lleva a smaller documents pero ojo con los joins para los datos. 

  - Modelos escalables, analizar la frecuencia de añadido al hacer embeed, para evitar unbound, cuando yo añado un nuevo documento nesteado, se reescribe todo el padre y puede ser demorado. 
  - Schema antippaterns son estos unbouds, bloated documents, indices no necesarios, falta  de indices, muchas colecciones, arrays muy grandes, Atlas ayuda a identificarlos. Performance advisor para obtener sugerencias.

## Recursos Adicionales
- [MongoDB Node Developer Path](https://learn.mongodb.com/learn/learning-path/mongodb-nodejs-developer-path)