#### Modelado de una fuerza

Bueno, según lo leído en el texto guía, tenemos que primero que todo tener clara la fórmula que vamos a usar en nuestra simulación y las fuerzas que están presentes en la misma, de esta manera tendremos claro desde un 
principo qué vectores van a ser factores clave para el modelo de esta en la simulación, es cierto que podemos inventarnos las fuerzas que querramos, pero realmente aquí haré un ejemplo hablando como de fuerzas ya existentes.

En este caso, teniendo una fuerza con cierto grupo de variables, debemos principalmente tener en cuenta lo siguiente: la dirección y la magnitud de la fuerza, y cómo estas dos cosas afectan a las variables involucradas
en la fórmula de la misma, después de tener esto totalmente claro, el siguiente paso es traducir esta fórmula a un código que pueda ser utilizado para simular la fuerza y aplicarla al objeto que tenemos en pantalla.

Ahora, desglosemos esto con ejemplos, si por ejemplo en la fuerza están involucradas la gravedad, la velocidad, y la fuerza normal, y estas se multiplican entre sí, tenemos que tener claro en qué dirección se multiplican 
las mismas, ya que es importante siempre saber hacia dónde va la fuerza, por ejemplo si una de las variables es negativa, esta siempre debe ser multiplicada por -1, si una variable ya es un vector dentro del código, se le 
puede llamar mediante una referencia utilizando una copia, una instancia, a veces, las fuerzas tienen constantes que arbitrariamente podemos cambiar, esto es algo que realmente va en decisión de quien esté haciendo el código.

Ahora, al final, después de tener en cuenta estos valores, si son constantes, variables, referencias dentro del código, etc, lo que debemos hacer es usar funciones de JS para recrear la fórmula en código, multiplicando con mult(),
dividiendo por div(), etc, al tener todo esto se normaliza el vector y se multiplica por la magnitud, la cual normalmente depende de nosotros y suele ser la normal multiplicada por la constante que arbitrariamente decidamos,
hecho esto, ya estaría lista y calculada nuestra fuerza, al mismo tiempo que traducida a código, realmente no es algo complicado, se debe tener un orden y siempre lo recomendable es utilizar copias para no modificar los valores 
de ninguna otra fuerza y poder tener una mayor libertad en el código, saber cómo se usan las funciones y como se aplican en el contexto, y poco más, las cosas principales son la dirección y la magnitud, y pues siempre saber
en qué momento se usa una función u otra en la fórmula para que la codificación sea correcta.
