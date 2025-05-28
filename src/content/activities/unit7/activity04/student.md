#### Consolidación y Metacognición

- 1. Primero que todo se configura el motor de Matter.js dentro del setup(), donde también creé los cuerpos físicos de cada letra usando Bodies.circle para que
     tuvieran un comportamiento realista. En el draw() actualizo el motor con Engine.update(engine) y luego dibujo las letras con funciones de p5.js en
     función de la posición de sus cuerpos, y ahí mismo aplico las fuerzas y comportamientos especiales, como la flotación libre y la órbita al activar la letra "O".

- 2. La simulación física la controlé usando cuerpos invisibles y propiedades como frictionAir y restitution. Sin embargo, visualmente decidí no usar el
     texto directamente, sino dibujar las letras encima de cuerpos circulares, lo que me permitió tener más control visual. Un reto fue hacer que los
     dibujos se alinearan bien con los cuerpos físicos sin perder legibilidad, especialmente cuando orbitaban o rotaban, es por eso que tomé la decisión de dejarlas "rectas" o mejor dicho, alineadas
     y que estas no giraran también, como la O, sobre su eje, que era algo que tenía pensado hacer.

- 3. En lugar de formas complejas, opté por cuerpos simples (círculos) para representar cada letra como una entidad independiente. Lo complejo fue
     simular la forma del sol en la letra "O" cuando se hace clic. Para eso usé una figura de un circulito amarillo, reemplazando el
     texto tenía la intención de hacerle también los rayitos, sin embargo aún después de varios intentos, esto no funcionó y realmente la descarté para no seguir perdiendo el tiempo con algo que no sabía cómo hacer que funcionara.
     La limitación principal fue que Matter.js no permite cuerpos con formas de texto directamente, así que tuve que simularlo visualmente encima de cuerpos básicos.

- 4. La simulación física fue clave para representar el significado de la palabra “espacio”. Usar un entorno negro, gravedad mínima y movimiento
     flotante reforzó la sensación de vacío espacial. La órbita al hacer clic en la "O" transformándola en un sol refuerza el sentido de sistema solar. Este
     tipo de simulaciones físicas funciona muy bien para conceptos relacionados con movimiento, energía o entornos naturales. Sería más difícil aplicarlo a
     palabras abstractas como “soledad” o “memoria”, sin embargo, con los ejemplos de la primera actividad,, de Ji Lee es un hecho claro que todo lo que se necesita es poner a trabajar la creatividad.

- 5. Más allá de este reto, la combinación de p5.js y Matter.js abre muchas posibilidades creativas. Se podrían crear instalaciones interactivas,
     visualizaciones de palabras con comportamientos vivos o incluso poemas cinéticos. También es un gran espacio para explorar cómo el significado
     puede cambiar dependiendo del comportamiento físico de las letras o elementos visuales, llevando el lenguaje a un plano dinámico y sensible, incluso llegué a pensar en las palabras que tienen varios significados,
     y esta diferencia se puede mostrar con esto, aunque sea la misma palabra, el entorno podría reaccionar diferente dependiendo del signiicado al que se esté haciendo referencia.


