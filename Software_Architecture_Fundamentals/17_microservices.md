## Architecture Styles Microservices

### Fecha: 17/09/2024

<img src="images/sa.jpg" alt="Gráfico de Introducción" width="150">

- **Notas:**
  - Muy popular actualmente.
  - A diferencia de otras archs que no son nombradas ni diseñadas por un consenso de arquitectos esta nace de un blog de Martin Fowler en el 2014. 
  - Muy inspirada en el DDD, donde el concepto de bounded context es muy importante. Bounded context representa un estilo de desacoplamiento, cuando un dev define un dominio este tiene muchos artefactos como entidades y comportamientos representados por clases de código, schemas, etc. Un dominio de Catalog tiene conceptos de items, usuarios y pagos, en algo monolitico el dev comparte muchos artefactos, haciendo clases reusables y dbs linked. Dentro de un bounded context, las clases de código y dbs schemas están acopladas para un trabajo pero no para nada por fuera del bounded context (como otro bounded context que necesite esas definiciones). Cada contexto define lo que necesita en lugar de acomodarse a un reuso. Decoupling vs reuso (trade-off), alto decoupling favorece la duplicación, microservices es high high decoupling, modelar la noción de bounded context. 
  - La topología son servicios, API layer, dbs, colas (async communication), cliente(s).
  <img src="images/65.png" width="1050">
  - Ratings: 
  <img src="images/64.png" width="1050">
  Partición muy técnica, single quantum por el uso de una sola db y mucho coupling entre layers y el engine orch. <br>
  Técnicas de ingenieria moderna como deploys, tests y evolucionaria no están presentes. <br>
  Elasticidad y escalabilidad están presentes. <br>
  Performance no presente una request está muy distribuida. <br>
  Simplicidad y costo muy malas. <br>
- **Preguntas:**
  - **1. Why is the bounded context concept so critical for microservices architecture?**  
  <details>
    <summary>Ver respuesta</summary>
    El concepto de bounded context es critico porque esta arch busca generar la noción física de este al generar servicios que declaren lo que necesitan en lugar de adapartse al reuso, se premia la duplicidad ya que lo más importante es el alto nivel de desacoplamiento entre bounded contexts.
  </details>

  - **2. What are three ways of determining if you have the right level of granularity in a microservice?**  
  <details>
    <summary>Ver respuesta</summary>
    Desaparace la base de datos centralizada y las consultas sync a esta. 
  </details>

  - **3. What functionality might be contained within a sidecar?**  
  <details>
    <summary>Ver respuesta</summary>
    Messaging grid, data grid, processing grid, deployment manager.
  </details>

  - **4.What is the difference between orchestration and choreography? Which does microservices support? Is one communication style easier in microservices?**  
  <details>
    <summary>Ver respuesta</summary>
    Maneja las inputs y las distribuye a las unidades disponibles, puede ser más complejo y conocer el estado de request/unidad.
  </details>

  - **5. What is a saga in microservices?**  
  <details>
    <summary>Ver respuesta</summary>
    Escucha del data pump las actualizaciones de data que deben realizarse mediante un contrato de acción definida y realiza estos cambios de manera async.
  </details>

  - **6. Why are agility, testability, and deployability so well supported in microservices?**  
  <details>
    <summary>Ver respuesta</summary>
    Caida de todas las unidades asociadas a una cache, redeploy y data archivada que no está en cache.
  </details>

  - **7. What are two reasons performance is usually an issue in microservices?**  
  <details>
    <summary>Ver respuesta</summary>
    Incrementa, entre más pequeña más probable.
  </details>

  - **8. Is microservices a domain-partitioned architecture or a technically partitioned one?**  
  <details>
    <summary>Ver respuesta</summary>
    tipicamente replicada, in-memory grids en cada unidad replicadas por el data grid, en distributed un server central accedido por las unidades, la decisión se toma según el tamaño y update rate, es performance y tolerancia vs consistencia.
  </details>

  - **9. Describe a topology where a microservices ecosystem might be only a single quantum.**  
  <details>
    <summary>Ver respuesta</summary>
    Escalabilidad, elasticidad y performance.
  </details>

  - **10. How was domain reuse addressed in microservices? How was operational reuse addressed?**  
  <details>
    <summary>Ver respuesta</summary>
    Es dificil testear los niveles de concurrencia tan extremos, pruebas en caliente en prod. 
  </details>

## Recursos Adicionales
- [Course](https://fundamentalsofsoftwarearchitecture.com/)