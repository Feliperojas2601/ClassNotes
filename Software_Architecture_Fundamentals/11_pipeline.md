## Architecture Styles Pipeline

### Fecha: 23/08/2024

<img src="images/sa.jpg" alt="Gráfico de Introducción" width="150">

- **Notas:**
  - Pipeline o pipes and filters arch. Patrón cuando se parte las funcionalidades, es el principio que siguen los Unix terminal shell languages como Bash. Una vía de comunicación.
  - Pipes forman el canal de comunicación entre filtros, tipicamente unidireccionales y point to point (no broadcast) por performance. 
  - Filters son autocontenidos, independientes y stateless generalmente, cumplen con una única tarea. Tipos:  
    - Producer: El punto inicial del proceso, source. 
    - Transformer: Acepta input, hace una transformación de la data y saca output.
    - Tester: Acepta input, valida una condición y opcionalmente da output.
    - Consumer: Punto final del pipeline, aquí se puede persistir.
  - La unidirección y simplicidad hace que sean temas reusables y muy extensibles. Ejemplos son las ETL, los document type to another, Apache Camel. 
  <img src="images/26.png" width="1050">
  - Ratings: 
  <img src="images/27.png" width="1050">
  Es técnica porque se parte la lógica en filtros (tipo), monolito y por tanto single quanta. <br>
  Poco costo, muy simple. <br>
  Modular por la separación de responsabilidades entre varios tipos de filtros. Esto también influye en que sea testeable y deployable pero sigue siendo un monolito y por tanto muy normal. <br>
  Fiablidad media, elasticidad y escalabilidad muy bajas al ser un monolito. <br>
  No soporta tolerancia a fallos. <br> 
- **Preguntas:**
  - **1. Can pipes be bidirectional in a pipeline architecture?**  
  <details>
    <summary>Ver respuesta</summary>
    No, son unidireccionales por la naturaleza secuancial del procesamiento.
  </details>

  - **2. Name the four types of filters and their purpose.**  
  <details>
    <summary>Ver respuesta</summary>
    Ver arriba el detalle.
  </details>

  - **3. Can a filter send data out through multiple pipes?**  
  <details>
    <summary>Ver respuesta</summary>
    Puede darse broadcast pero no se recomienda, point-to-point da performance.
  </details>

  - **4. Is the pipeline architecture style technically partitioned or domain partitioned?**  
  <details>
    <summary>Ver respuesta</summary>
    Técnica, se divide la lógica en tipos de filtros, un artefacto técnico de la arquitectura.
  </details>

  - **5. In what way does the pipeline architecture support modularity?**  
  <details>
    <summary>Ver respuesta</summary>
    En la separación de responsabilidades que se da con el uso de varios tipos  filtros para repartir la lógica.
  </details>

  - **6. Provide two examples of the pipeline architecture style.**  
  <details>
    <summary>Ver respuesta</summary>
    Herramientas de ETLs, herramientas de .ext to .ext2.
  </details>

## Recursos Adicionales
- [Course](https://fundamentalsofsoftwarearchitecture.com/)