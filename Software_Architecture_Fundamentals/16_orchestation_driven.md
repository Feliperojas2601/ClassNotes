## Architecture Styles Orchestation driven 

### Fecha: 17/09/2024

<img src="images/sa.jpg" alt="Gráfico de Introducción" width="150">

- **Notas:**
  - Esta arquitectura debe ser entendida bajo el contexto de su era. Las decisiones combinadas con una cultura organizacional desastrosa llevaron esta arch a la irrelevancia. Las companías en los 90's tardios necesitaban IT más sofisticada, se necesitaba escalabilidad y crear distribuidas pero con limitaciones. OS era por máquina y con licencia individual. Reusar era la nueva filosofía. 
  - La topología establece una taxonomía de servicios.
  <img src="images/63.png" width="1050">
  - Muchas companias se molestaban de tener que reescribir software, cada capa de esa topología busca reducir esto y reusar. 
    - Negocio: dan el punto de entrada, no-code solo input/output y schemas, definidos por usuarios de negocio. 
    - Empresa: servicios de grano fino con implementaciones compartidas. Comportamiento átomico, building-block de servicios de grano grueso utilizando el orquestador. La naturaleza dinamica del software desafia esta colección de reusables.
    - Aplicación: No son tan granulares pero son servicios de un solo uso como geolocation, que no merece ser reusable.
    - Infra: Temas operacionales como log, monitor, auth.
    - Orchestation: Es el corazón en esta distribución, se liga a una o unas pocas dbs, maneja el comportamiento transaccional, por ley de Conway el team de integraciones que maneja esto se vuelve un cuello de botella de burocracia. 
  - En la práctica es un desastre, muy complejo construir servicios tan granulares para su reuso, arch se vuelve cada vez más compleja, extramanejos de campos y lógica que no es necesaria por ser compartida y los límites son dificiles de definir, también la naturaleza va en contra de esto. Impractico una arch tan orientada a separación técnica.
  - Demasiado coupling, el reuso implica acoplamiento que hace el cambio más riesgoso, deploys coordinados, test más complejo y menos eficiencia. 
  - Ratings: 
  <img src="images/64.png" width="1050">
  Partición muy técnica, single quantum por el uso de una sola db y mucho coupling entre layers y el engine orch. <br>
  Técnicas de ingenieria moderna como deploys, tests y evolucionaria no están presentes. <br>
  Elasticidad y escalabilidad están presentes. <br>
  Performance no presente una request está muy distribuida. <br>
  Simplicidad y costo muy malas. <br>
- **Preguntas:**
  - **1. Where does space-based architecture get its name from?**  
  <details>
    <summary>Ver respuesta</summary>
   De tuple space, técnica para comunicar procesos en paralelo usando la memoria compartida.
  </details>

  - **2. What is a primary aspect of space-based architecture that differentiates it from other architecture styles?**  
  <details>
    <summary>Ver respuesta</summary>
    Desaparace la base de datos centralizada y las consultas sync a esta. 
  </details>

  - **3. Name the four components that make up the virtualized middleware within a space-based architecture.**  
  <details>
    <summary>Ver respuesta</summary>
    Messaging grid, data grid, processing grid, deployment manager.
  </details>

  - **4. What is the role of the messaging grid?**  
  <details>
    <summary>Ver respuesta</summary>
    Maneja las inputs y las distribuye a las unidades disponibles, puede ser más complejo y conocer el estado de request/unidad.
  </details>

  - **5. What is the role of a data writer in space-based architecture?**  
  <details>
    <summary>Ver respuesta</summary>
    Escucha del data pump las actualizaciones de data que deben realizarse mediante un contrato de acción definida y realiza estos cambios de manera async.
  </details>

  - **6. Under what conditions would a service need to access data through the data reader?**  
  <details>
    <summary>Ver respuesta</summary>
    Caida de todas las unidades asociadas a una cache, redeploy y data archivada que no está en cache.
  </details>

  - **7. Does a small cache size increase or decrease the chances for a data collision?**  
  <details>
    <summary>Ver respuesta</summary>
    Incrementa, entre más pequeña más probable.
  </details>

  - **8. What is the difference between a replicated cache and a distributed cache? Which one is typically used in space-based architecture?**  
  <details>
    <summary>Ver respuesta</summary>
    tipicamente replicada, in-memory grids en cada unidad replicadas por el data grid, en distributed un server central accedido por las unidades, la decisión se toma según el tamaño y update rate, es performance y tolerancia vs consistencia.
  </details>

  - **9.List three of the most strongly supported architecture characteristics in space based architecture**  
  <details>
    <summary>Ver respuesta</summary>
    Escalabilidad, elasticidad y performance.
  </details>

  - **10. Why does testability rate so low for space-based architecture?**  
  <details>
    <summary>Ver respuesta</summary>
    Es dificil testear los niveles de concurrencia tan extremos, pruebas en caliente en prod. 
  </details>

## Recursos Adicionales
- [Course](https://fundamentalsofsoftwarearchitecture.com/)