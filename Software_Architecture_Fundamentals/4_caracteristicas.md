## Architecture Characteristics 

### Fecha: 03/08/2024

<img src="images/sa.jpg" alt="Gráfico de Introducción" width="150">

- **Notas:**
  - Una solución de software es requerimientos de dominio o funcionales y las caracteristicas de la arquitectura. 
  - Es clave definir, descubrir y analizar las caracteristicas no relacionadas al dominio para el éxito de una arquitectura.
  - Una caracteristica debe: 
    - Especificar una consideración no relacionada al dominio. 
    - Influenciar un aspecto estructural de la arquitectura. 
    - Ser importante para el éxito.
  - Especifican criterios de operación y diseño para el éxito. 
  - Debe preguntarse si una caracteristica requiere estructura especial para darse. 
  - Debe ser importante, debemos escoger la menor cantidad de destas de acuerdo a lo necesario. 
  - Implicitas son las que no están en los requermientos y deben ser procesadas por el arquitecto y su conocimiento del dominio.
  - Explicitas si están en los requerimientos.
  - Cada organización define su lista de caracteristicas, se agrupan en categorías amplias. Es imposible encontrar una lista definitiva o definiciones completamente verdaderas, se van a solapar definiciones según el problema.
  - La categoría de las operacionales tiene el performance, elasticidad, resiliencia, las que se cruzan con operacional o devops.
  - Las estructurales van de la mano del código, mantenibilidad, extensibilidad.
  - Hay un grupo que no entra en categorías como seguridad, disponibilidad y que son cross en muchos aspectos.
  - Las arquitecturas pueden soportar solo algunas, no todas porque las soluciones de unas cosas empeoran otras como la seguridad y el rendimiento. 
  - Debe hacerse un tarde-off según el problema de dominio y hacer una arquitectura iterativa y evolucionaria que pueda ser modificada con una iteración ágil.
## Recursos Adicionales
- [Course](https://fundamentalsofsoftwarearchitecture.com/)