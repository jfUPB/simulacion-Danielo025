#### Marco motion 101

- En este caso, en la parte del código en donde se ve claramente el marco 101, es en la parte de Update(), aquí se calcula la aceleración, esta se suma a la velocidad, y la velocidad se suma a la posición, en esencia, esta
es la base del marco motion 101, también, para complementar el código, se limita la velocidad y se verifican los bordes, para que el objeto nunca salga de la pantalla.

- Hablando de los algoritmos de la unidad anterior mediante los cuales yo definía la aceleración, eran bastante simples, básicamente la aceleración que personalmente utilizaba en la unidad anterior era un vector el cual yo, 
arbitrariamente decidía qué valores tenía, podían ser aleatorios, o podían ser valores que como antes mencioné, yo simplemente ponía y poco más, a continuación mostraré ejemplo de estas entradas de código, directamente
desde la unidad anterior:

```js
class Mover {
  constructor() {
    this.position = createVector(random(width), random(height));
    this.velocity = createVector(random(-2, 2), random(-2, 2));
    this.aceleracion = createVector(0.01, 0.01);
  }

  update() {
    this.velocity.add(this.aceleracion)
    this.position.add(this.velocity);
  }


.
.
.
.
```

En este caso se aprecia cómo claramente está el marco motion 101, en donde a la velocidad se le suma la aceleración, y a la posición se le suma la velocidad, aquí la aceleración tiene valores concretos, y la velocidad 
está limitada a mi manera, me funciona y eso fue lo importante en su momento 

- Ahora, respecto a qué tienen que ver estas cosas con las leyes de movimiento de Newton, y es que en el caso de la unidad anterior, prácticamente ya empezamos a aplicar las leyes de Newton, sólo que de una forma sumamente básica, 
en la unidad actual, la 3, la idea es tener en cuenta las leyes de movimiento, el cómo se mueven los objetos en el mundo físico y cómo esto se puede traducir a código, y era algo que ya habíamos hecho, sólo que de forma
muy arbitraria, básica, y sin tener en cuenta factores externos, prácticamente estábamos aplicando las leyes en un ambiente sumamente controlado, lo cual realmente es poco realista, ahora, la diferencia es que estas fuerzas
que se aplican sobre el "objeto" serán calculadas, puede que mediante alguna fórmula de alguna fuerza ya existente, o nos podríamos inventar una, y también afectarán otros factores al objeto, como resistencia del aire,
la masa del mismo, etc. Ahora todo es como un marco motion 101, pero mucho más complejo, y esto se traduce a las leyes de newton, pero en código.
