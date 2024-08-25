## Architecture Styles Microkernel

### Fecha: 25/08/2024

<img src="images/sa.jpg" alt="Gráfico de Introducción" width="150">

- **Notas:**
  - La arquitetcura de microkernel o plug-in es muy usada a día de hoy. Hace match con las aplicaciones de software basadas en productos, empacada y disponible para descragar e instalar como un single monolito deployment instalado en el sitio del producto.
  - Dos componentes principales, el core system y los plug-ins. La lógica se reparte en estos componentes dando mucha extensibilidad, adaptabilidad e isolation de features.
  - El core es la mínima cantidad de features para correr el sistema. Los IDE's son un buen ejemplo, core es abrir archivos, modificar texto y guardar. También se puede entender el core como el happy path del proceso general de la aplicación. Remover la cyclomatic complexity del core hacia los plugins incrementa la mantenibilidad y testeabilidad del código. Una aplicación de device recycling no va a poner todos los dispositivos en el core sino cada uno de ellos como un plug-in y en el core tendremos la localización e invocación del plug-in que corresponda. 
  - Dependiendo del tamaño y complejidad se puede implementar el core como un layered (técnico) o modular (funcionalidades) monolith, en algunos casos puede separarse también en varios deploy unit services con plugins especificos para cada uno. Es común que compartan una sola DB.  
  <img src="images/31.png" width="1050">
  - La UI puede estar embeed o como otra deployment unit e incluso en la UI se puede implemntar plugins. 
  <img src="images/32.png" width="1050">
  - Los plugins son standalone, independientes que contienen procesamiento especializado, features adicionales que extiendan al core. Pueden usarse para isolar codigo altamente volatil.
  - La comunicación core - plug es generalmente 1-1 con el pipe siendo una invocación de un método. Pueden ser compile-based o runtime-based. Pueden ser añadidos o removidos en runtime sin tener que redeplegar el core y se manejan a través de un framework como OSGi. Los compile-based son mucho más sencillos de manjear pero requieren redeploy. Pueden ser implementados como shared librarios (como JAR, DLL, Gem) pero lo más sencillo es otro paquete o namespace dentro de la misma base de código. 
  - La comunicación también puede ser con un remote access protocol con un plug-in como un standalone service pero esto sigue siendo single quanta por el monolito del cor, toda request debe ir al core antes de llegar al plug-in. Con esta alternativa hay mejor decoupling y permite async pero la transformación a lo distribuido por tanto mayor complejidad y costo. Debe analizarse muy bien si irse por esta alternativa de comunicación. 
  - No es común que cada plug-in se conecte a la bd compartida, en lugar de eos el core pasa toda la data necesaria para que el plug-in opere. Pero esto no limite que cada plug-in tenga su propia datastore a la que ellos acceden solamente. 
  - Registro de plugins, para que el core sepa cuales son los plugins y como acceder a ellos. Una manera común es hacer un registry de plugins, que tenga el nombre, data contracts y método de acceso. El registry puede ser tan simple como un struct map que pertenezca al core y tenga key-values de cada plugin o tan complejo como un discovery tool dentro del core o externa (Apche ZooKeeper). 
  - Los contratos entre plugin y core son estandar através de los dominios del plugin e incluyen input output data retornada. Contratos personalizados se dan cuando un tecero desarrolla el plugin y se recomienda realizar un adapter para que el core no tenga código especializado. 
  - Ratings: 
  <img src="images/33.png" width="1050">
  Simplicidad y costo resaltan como layered (Monolito).<br>
  Escalabilidad, elasticidad y tolerancia a fallos sus debilidades (Monolito) <br>
  Tanto técnico como dominio en su partición, es único este estilo. <br>
  Test, deploy y fiabilidad estándar pero por encima gracias al isolate de plugins. <br>
  Modularidad y extensibilidad también resaltan porque el estilo permite agregar, remover y separar muy bien responsabilidades. <br>
  Performance normal porque suelen ser aplicaciones pequeñas que no crecen mucho como la mayoría de layereds. Además no sufren tanto de sinkhole porque se puede desconectar funcionalidad de manera sencilla.<br>
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