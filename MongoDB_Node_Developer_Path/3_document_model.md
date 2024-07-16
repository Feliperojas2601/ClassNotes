## Document Model

### Fecha: 16/07/2024

<img src="images/mongo.png" alt="Gráfico de Introducción" width="300">

- **Notas:**
  - Documentos se muestran en JSON pero se guardan por debajo en BSON, binario de JSON. 
  - BSON da más data types de JSON, todos los de JSON, dates, numbers, ObjectId, string, object, array, boolean, null.
  - ObjectId lo usa mongo para crear un _id a cada documento, como una PK. 
  - Data polimórfica, bajo un mismo documento diferentes campos con diferentes tipos de datos.  
  - En mongo solo afectamos el schema con nuevas propiedades y ya, no necesitamos scripts, rellenos ni migraciones, etc. 

## Recursos Adicionales
- [MongoDB Node Developer Path](https://learn.mongodb.com/learn/learning-path/mongodb-nodejs-developer-path)