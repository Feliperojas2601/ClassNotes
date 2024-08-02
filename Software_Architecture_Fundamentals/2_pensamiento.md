## Architectural Thinking

### Fecha: 02/08/2024

<img src="images/sa.jpg" alt="Gráfico de Introducción" width="150">

- **Notas:**
  - Lo primero es entender la diferencia entre arquitectura y diseño, creemos erradamente que las responsabilidades del diseño o desarrollo estpan en las clases, UI, code en general y el arquitecto tiene una línea divisoria pensando en estilo, estructura, caracteristicas pero debemos pensar la arquitectura como algo más global y el diseño mucho más detallado pero el arquitecto es parte de ambos ciclos y da liderazgo al desarrollador en cargo del código. 
  - La amplitud técnica es más que la profundidad técnica, en las cosas que sabes son cosas que debes mantener y en donde se da la profundidad técnica. El arquitecto debe ir más allá, a las cosas que conoce peor no sabe y agrandar su amplitud, para conocer más posibles soluciones a un problema. 
  No se debe quemar intentando ser experto en varias áreas. 
  - El antipatrón frozen caverman sucede cuando un arquitecto por una experiencia pasada tiene siempre la astilla y la incluye en todo lo que desarrolla, debemos ser conscientes del riesgo sí pero también de su probabilidad y realidad. 
  - Todo es un trade-off, en el ejemplo tenemos usar una cola pub sub o varias colas a distintos ms, al parecer la pub sub es más escalable y es la primera opción, pero debemos analizar el trade off, pros extensibilidad y decoupling, cons data access security, no hay contratos de respuesta personalizados, no hay un monitoreo personalizado. Ya luego de esto se analiza y se toma la decisión, pero debe hacerse el pros cons para la decisión que se tome. 
  - Es importante entender el negocio, qué requerimientos son necesarios y cómo transformalos en caracteristicas. 
  - Debe analizar el balance entre arquitecto y desarrollador, evite el cuello botella cuando el arquitecto tiene mucho control sobre el código y detiene al equipo. Reparta las funcionalidades en el equipo y el arch concentrado en una pieza de funcionalidad. Hay 4 maneras de seguir codeando: 
    - Haga POC's. 
    - Ataque la deuda técnica. 
    - Ataque bugs. 
    - Automatice procesos. 

## Recursos Adicionales
- [Course](https://fundamentalsofsoftwarearchitecture.com/)