## Modularity

### Fecha: 02/08/2024

<img src="images/sa.jpg" alt="Gráfico de Introducción" width="150">

- **Notas:**
  - La modularidad es un principio de organización. Preservar buena modularidad es una caracteristica arch implicita. 
  - Modularidad es la agrupación lógica de código relacionado. 
  - Los arquitectos deben estar pendintes porque si los desarrolladores modularizan mal, la reutilización se complica. 
  - Para medir la modularidad tenemos la cohesion, acoplamiento y connascence. 
  - La cohesion refiere a que tan relacionadas están las partes en un módulo. Funcional, sequencial, comunicacional, procedural, temporal, lóguca, coincidencial.  
  - El LCOM mide la cohesion de un módulo, es la suma del conjunto de métodos no compartidos. Un LCOM alto indica carencia de cohesion. La deficiencia de esta métrica es que no determina lógicamente si las partes deben estar unidas. 
  - El acoplamiento afferent y efferent mide con grafos las conexiones incoming y outgoing de un artefacto de código a otras. Abstractness es la tasa de artefactos abstractos sobre implementaciones. Inestabilidad es la tasa de efferent sobre efferent y afferent, un código se rompe más con este valor alto por la delegación a otros artefactos.
  - La distancia de la secuencia main se define como la suma de las tasas anteriores - 1, se modela una linea de 1 a 1 X = Inestabilidad y Y = Abstractness, entre más cerca esté a la linea es más balanceada la clase.  Hacia arriba es la zona de lo muy abstracto y dificil de usar por tanto inutil y hacia abajo la zona del dolor, código dificil de mantener. 
  - Connascence es afferent y efferent coupling pero pasado por programación orientada a objetos. La static es la code-level (Nombre, tipo, meaning, position, algorithm) y la dinamica es en ejecución (ejecución, timing, values, identity). Las propiedades son la fuerza, que es el nivel de dificultad para refactorizar, la localidad de qué tan próximos son los modulos en el code. El grado, o el tamañao de su impacto en número de clases.  
## Recursos Adicionales
- [Course](https://fundamentalsofsoftwarearchitecture.com/)