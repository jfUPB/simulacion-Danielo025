#### Comparación de Algoritmos

- 1. La diferencia principal entre Flow Fields y Flocking está en que la "Inteligencia" reside en un lugar diferente dependiendo del tipo de simulación, en uno es en el entorno, y en otro en los agentes como tal.
     En Flow Fields, las reglas están en el entorno; los agentes siguen vectores predefinidos que guían su movimiento.
     En cambio, en Flocking, la inteligencia está en las interacciones entre los agentes mismos, quienes se coordinan con sus vecinos para mantener separación, alineación y cohesión.

- 2. Con Flow Fields es más fácil lograr patrones visuales uniformes y controlados, como corrientes de partículas que fluyen en una dirección definida, similar a un río o viento. En Flocking, los comportamientos
     colectivos como bandadas de aves o cardúmenes de peces resultan más naturales, donde cada agente se adapta a los movimientos de los otros generando formas fluidas y dinámicas.
     O por ejemplo como en el caso de mi código, en patrones de simulación de multitudes o grupos que se conectan entre sí por uno u otro motivo, en este caso, metafóricamente, ideologías más que nada.

- 3. Flow Fields es ventajoso cuando se quiere un control claro y direccional del movimiento, útil para efectos como humo, agua o flujos de tráfico. Sin embargo, puede parecer menos orgánico. Flocking es ideal para
     simular grupos con comportamiento autónomo y flexible, pero puede ser más complejo y menos predecible, aparte, otra desventaja es que Flocking puede ser más costoso computacionalmente y más difícil de controlar en detalles específicos.

- 4. Estos ejemplos me ayudaron a entender que un agente autónomo es una entidad que toma decisiones basadas en reglas simples y su percepción local, sin un control central. Sus características principales son la
     capacidad de percibir el entorno o a otros agentes, y de actuar en consecuencia para lograr un comportamiento colectivo emergente.

- 5. El comportamiento emergente lo noté cuando, sin programar movimientos específicos para el grupo, los agentes empezaron a formar patrones complejos como formaciones o agrupaciones en Flocking, y flujos
     suaves y ordenados en Flow Fields, mostrando cómo reglas simples pueden generar comportamientos complejos sin control directo, o bueno, un poco indirecto en el caso del "comportamiento social".
