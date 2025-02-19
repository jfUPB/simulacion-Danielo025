#### Respondiendo a las preguntas

- ¿Método mag(), método magSq()? | El método mag() devuelve la magnitud o longitud del vector, es decir, calcula la distancia desde el origen (0,0) hasta el punto definido por el vector, mientras que
magSq() devuelve el cuadrado de la magnitud sin calcular la raíz cuadrada lo cual es más eficiente porque evita calcular la raíz cuadrada, se usa cuando solo necesitas comparar magnitudes y no la longitud exacta.

- ¿Método normalize()? | El método normalize() convierte un vector en un vector unitario, es decir, mantiene la dirección pero lo reduce a longitud 1.

- ¿Método dot()? | dot() mide la alineación entre dos vectores, si el resultado es positivo apuntan en la misma dirección, si es negativo apuntan en direcciones opuestas.

- ¿Versión estática de dot(), versión de instancia de dot()? | La versión de instancia se usa cuando ya tienes un vector y quieres calcular el producto punto con otro, como por ejemplo:

``` Js
let v1 = createVector(1, 2);
let v2 = createVector(3, 4);
console.log(v1.dot(v2)); // Devuelve 11 (1*3 + 2*4)
```
Mientras que en la versión estática no necesitas crear un objeto p5.Vector, simplemente pasas dos vectores y obtienes su producto punto, como por ejemplo:
``` Js
let v1 = createVector(1, 2);
let v2 = createVector(3, 4);
console.log(p5.Vector.dot(v1, v2)); // Devuelve 11
```

Es una diferencia bastante simple, sigamos:

- ¿Producto cruz de dos vectores? | El producto cruz crea un vector perpendicular a los dos originales, su magnitud indica qué tan "cruzados" están los vectores, lo que vendría siendo
lo mismo a qué tan grande es el área que hay entre ellos, mientras que la orientación es hacia donde apunta este vector perpendicular a ellos.
Esto sigue la "regla de la mano derecha": el nuevo vector apunta en la dirección en que tu pulgar apunta si alineas los dedos con el primer vector y giras hacia el segundo.

- ¿Método dist()? | El método dist() calcula la distancia entre dos vectores en el espacio, y pues podría servir para saber si un objeto está lo suficientemente cerca de otro (colisión) o
en su defecto para medir distancias entre enemigos y jugadores en juegos.

- ¿Métodos normalize() y limit()? | Ya sabemos para qué sirve normalize, y podría servir para controlar fuerzas en simulaciones físicas, o para mantener direcciones sin afectar la velocidad.
Mientras que limit() restringe la magnitud de un vector a un máximo dado, esto podría servir para evitar que algunos objetos se muevan demasiado rápido en juegos o simulaciones.
