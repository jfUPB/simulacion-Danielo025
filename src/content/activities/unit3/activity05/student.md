#### Sumatoria de fuerzas

- Esto es algo que ya se había planteado en la unidad 2 del libro, en la parte de acumulación de fuerzas, y es que el problema con este planteamiento es que el método
applyForce(force) está sobrescribiendo el valor de this.acceleration en lugar de acumular las fuerzas que actúan sobre el objeto, esto se hace por referencia, es decir, que el método se reemplaza, al llamar primero a wind,
y luego a gravity, sin importar cual sea el valor, en otras palabras, al llamar mover.applyForce(wind) y luego mover.applyForce(gravity), la primera fuerza (wind) se almacena en this.acceleration, 
pero en la siguiente línea esta es reemplazada por gravity, en lugar de sumarse, que es lo que estamos buscando.

- En este caso sería algo fácil de solucionar, y es que en lugar de reemplazar estas fuerzas, debemos sumarlas en el código.

Es decir, en lugar de escribir:

```js
applyForce(force) {
  this.acceleration = force;
}
```

Debemos escribir:

```js
applyForce(force) {
  this.acceleration.add(force);
}
```

Con esta corrección, cada nueva fuerza que se aplique se sumará a la aceleración existente, en lugar de reemplazarla.

- Ahora, en lugar de implementarlo yo, voy a tomar como ejemplo en el cual esto ya está corregido del libro e indicaré donde se puede notar

```js
class Mover {
  constructor() {
    this.mass = 1;
    this.position = createVector(width / 2, 30);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);                    // A esto me refiero, en lugar de reemplazarse la fuerza, se suma, 
  }                                              // de esta manera no sólo queda el viento, sino también la gravedad

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  display() {
    stroke(0);
    strokeWeight(2);
    fill(127, 127);
    ellipse(this.position.x, this.position.y, 48, 48);
  }

  checkEdges() {
    if (this.position.x > width) {
      this.position.x = width;
      this.velocity.x *= -1;
    } else if (this.position.x < 0) {
      this.velocity.x *= -1;
      this.position.x = 0;
    }
    if (this.position.y > height) {
      this.velocity.y *= -1;
      this.position.y = height;
    }
  }

}


let mover;

function setup() {
  createCanvas(640, 240);
  mover = new Mover();
  createP("Click mouse to apply wind force.");
}

function draw() {
  background(255);

  let gravity = createVector(0, 0.1);
  mover.applyForce(gravity);

  if (mouseIsPressed) {
    let wind = createVector(0.1, 0);
    mover.applyForce(wind);
  }

  mover.update();
  mover.display();
  mover.checkEdges();
}
```
