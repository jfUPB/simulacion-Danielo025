#### Trabajando con vectores

En este caso tenemos algo que para poder entender, debemos entender como funciona add(), y es que en este caso 
'position' y 'velocity' son vectores creados de dos dimensiones (x, y), donde 'position' representa la ubicación del objeto y 'velocity' su movimiento.

Aquí lo que pasa es que en cada fotograma, la posición se actualiza sumándole la velocidad con position.add(velocity), lo que significa que 
'position.x' aumenta en 'velocity.x' y 'position.y' en 'velocity.y', lo que es en esencia la fórmula de la suma de unos vectores pero en código, y de esta manera es como se desplaza el vector en pantalla.

Ya la otra línea de código que consiste en los bordes (width o height), lo que haces es que al tocarlos la velocidad se invierte y por ende la pelota se devuelve, y poco más.


Ahora, por qué no funciona la línea 'position = position + velocity', el tema aquí es cómo funciona la lógica de los vectores en js, y debemos empezar porque los vectores son objetos, y es que 
si 'position' y 'velocity' son objetos que representan vectores, necesitas un método que entienda cómo sumar sus componentes, que en este caso, sería la función add(), ya que
no puedes usar + para sumar objetos en JavaScript.
