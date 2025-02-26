#### Experimentando con el movimiento

El ejemplo se entiende debido a la actividad anterior, en donde precisamente se explica como funciona el movimiento y posteriormente se muestra el código y la vista previa de este, ahora, siguiendo propiedades físicas
básicas, qué pasaría si teniendo en cuenta que la velocidad es la derivada del movimiento, derivamos nuevamente la velocidad para conseguir una aceleración, es decir:

- ¿Qué pasaría si añadimos un vector de aceleración y este se lo sumamos a la velocidad?
  
- Supongo que lo que pasará es que efectivamente habrá una aceleración constante ilimitada, pues será cada que se haga una iteración, en un principio no tengo pensado limitarla o controlarla

- Al principio reemplacé la velocidad por la aceleración, pero esto fue una falla al escribir el código jajaja, puse que se le añadiera la aceleración a la posición en lugar de a la velocidad, sin embargo, al corregir
es un hecho que funcionó como se esperaba, era algo bastante intuitivo, pero de esta forma podemos hacer más cosas, incluso, después de cierto tiempo desacelerar nuevamente, y si ponemos límites podemos tener un
control absoluto de todo

- Pues en este caso el porqué es que se está sumando la aceleración con la velocidad, y la velocidad con el tiempo, creando así lo que en física conocemos como doble derivada, o segunda derivada, que en este caso
al estar hablando del movimiento, la primera derivada es la velocidad, y al derivar esta, tenemos la aceleración, entonces, en esencia, al movimiento se le está sumando el vector de velocidad, mientras que al vector
de velocidad se le está sumando constantemente el vector de aceleración, como anteriormente dije, aquí lo importante no es controlar, sino experimentar, y funcionó

- Llega un punto en que sin un límite de velocidad, la aceleración constante hace que el objeto se mueva cada vez más rápido,
  lo que puede llevar a parecer que el objeto se teletransporta de un lado a otro, lo que lo hace poco realista, esto se puede controlar uun poco, más o menos, y también depende un poco de la velocidad inicial la cual es aleatoria
  en cierto rango, pero sigue sin tener un límite así que eventualmente siempre tendrá este comportamiento

Código modificado de la clase mover:

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

  show() {
    stroke(0);
    strokeWeight(2);
    fill(95,200,200);
    circle(this.position.x, this.position.y, 48);
  }

  checkEdges() {
    if (this.position.x > width) {
      this.position.x = 0;
    } else if (this.position.x < 0) {
      this.position.x = width;
    }

    if (this.position.y > height) {
      this.position.y = 0;
    } else if (this.position.y < 0) {
      this.position.y = height;
    }
  }
}
```


