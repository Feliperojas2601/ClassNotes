## Architecture Characteristics Scope 

### Fecha: 22/08/2024

<img src="images/sa.jpg" alt="Gráfico de Introducción" width="150">

- **Notas:**
  - Una suposición predominante que se ha quedado obsoleta es que las caracteristicas se ponen en el scope del sistema, lo cual era verdad para monolitos pero con la distribución se ha estrechado más este alcance.
  - En las caracteristicas estructurales mediamos una base de código y no evaluamos componentes como las bases de datos que impactan sobre todo las caracteristicas operacionales. Cuando se evaluan este tipo de caracteristicas debemos tener en cuentas estos componentes por fuera de la base de código que van a influir en nuestras caracteristicas. 
  - Connascence, dos componentes lo son si el cambio en uno hace que se requiera el cambio en el otro para mantener la correctitud del sistema. La estatica es cuando por ejemplo dos microservicios comparten la defininicón de una clase. La dinamica puede ser del tipo sync y async, sync con el llamado y espera de la respuesta en la comunicación.  
  - La cohesion funcional se da cuando conceptos de negocio semanticos pegan partes del sistema
  - Architecture quantum, un artefacto con independencia de despliegue con alta cohesion funcional y connasence sync. Que sea independiente deployable es porque el quantum incluye todos los componentes necesarios para funcionar independiente de otras partes de la arquitectura, una base de datos en un microservicio propia hace parte de multiples quantum (multiples ms - dbs). La alta cohesion funcional implica que el quantum haga algo valioso por ejemplo que una clase Customer tenga cohesion alta es porque sus metodos y propiedades estan conectados. La sync connasence implica llamados sync dentro de la aplicación o el quantum.
  - DDD es una tecnica de modelado que permite la descomposicion organizada para problemas de dominio. Define el bounded context donde todo lo relacionado al dominio es visible internamente pero opaco al exterior (otros bounded context). Si cada entidad funciona mejor con un contexto localizado entonces en lugar de unificar y hacer código más complejo podemos tener distintas implementaciones de lo mismo adaptadas en cada contexto.
  - Arquitectos definen caracteristicas más a nivel de quantum que de sistema.
  - Katas Going going gone <br>
  Descripción: Compania de subastas quiere online a escala nacional. <br>
  Users: Escalar a cientos/miles por subasta. <br>
  Requerimientos/Contexto adicional: Ver a detalle en libro. Tarjetas de crédito, track personas con un indice de reputación, expansión agresiva mezclando competidores, video stream de las subastas. <br>
  National/Cientos/Miles usuarios -> Escalabilidad, elasticidad. <br>
  Tarjetas de crédito -> Seguridad? Influencia estructura o solo con buenas prácticas del código? Preguntar por fraude para ver qué. <br>
  Track -> Audability, loggabilit? Aspecto estructural, qué es exactamente el indice? <br>
  Expansión -> Interoperabilidad. <br>
  Stream -> Disponibilidad. <br>
  La disponibilidad hace que nos planteemos una cosa, es tan importante en las distintas partes, es mucho más critico para el operador de subastas que el sitio esté disponible que para los cientos de usuarios, aquí llega la parte de distintas quantums del sistema, distintas caracteristicas. <br>
  Para un scope más granualr tenemos el stream bidder, el bidder platform y el auctioneer platform. 
    - Stream -> Disponiubilidad, escalabilidad, performance. 
    - Bidders -> Confiabilidad, Dispo, Esca, Elasticidad. 
    - Auctioner -> Dispo, confi, esca, seguridad. 
- **Preguntas:**
  - **1. What is an architectural quantum, and why is it important to architecture?**  
  <details>
    <summary>Ver respuesta</summary>
    El quantum es un artefacto deployable indepentiende con alta cohesion funcional y connasence sync. Es importante para identificar partes del sistema que funcionen separadas de otras partes y tener un scope más granualr de las caracteristicas de las distintas partes. 
  </details>

  - **2. Assume a system consisting of a single user interface with four independently deployed services, each containing its own separate database. Would this system have a single quantum or four quanta? Why?**  
  <details>
    <summary>Ver respuesta</summary>
    Es un sistema de múltiple quanta, en especifico de 4, suponiendo que cada servicio tenga su parte en la UI entonces un quantum es UI + MS + DB, 4 partes distintas que operan de manera independiente, con cohesion funcional alta porque son agrupaciones de código y funcionalidades diferentes y con manejo sync de connascence como en llamados de ms - db.
  </details>

  - **3. Assume a system with an administration portion managing static reference data (such as the product catalog, and warehouse information) and a customer-facing portion managing the placement of orders. How many quanta should this system be and why? If you envision multiple quanta, could the admin quantum and customer-facing quantum share a database? If so, in which quantum would the database need to reside?**  
  <details>
    <summary>Ver respuesta</summary>
    Son dos quantas distintas, por un lado el sistema de admin de data estatica es un quanta por si solo y por otro lado del sistema de ordenes. Si los ponemos a compartir una base de datos entonces debe de ser para el de las ordenes ya que es más critico y no debería ser así pues rompe la independencia de los quuanta. 
  </details>

## Recursos Adicionales
- [Course](https://fundamentalsofsoftwarearchitecture.com/)