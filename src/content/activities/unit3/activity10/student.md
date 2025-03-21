#### Modelando las fuerzas:

- **Fricción:**
  
  La fricción es una fuerza que se modela tomando una copia de la velocidad del objeto, normalizándola para obtener solo la dirección y multiplicándola
  por un coeficiente negativo, lo que genera una fuerza que se opone al movimiento; esta fuerza se aplica a la aceleración del objeto,
  sumándose a otras fuerzas (como la gravedad), y al final de cada frame se reinicia la aceleración para que las fuerzas se
  recalculen desde cero, logrando que el objeto desacelere de forma realista.

El acceso a la aplicación interactiva estaría en el siguiente link: [Aplicación interactiva de Fricción](https://editor.p5js.org/Danielo025/full/XxgtGaqX7)

Aquí tendríamos el código de la fricción: 
```js
class Mover {
  constructor() {
    this.mass = 1;
    this.position = createVector(width / 2, 30);
    this.velocity = createVector(2, 0); // Se inicia con cierta velocidad horizontal
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  // Calcula y aplica la fricción, que se opone a la dirección del movimiento.
  applyFriction() {
    // Copia la velocidad para no modificar el vector original (paso por referencia vs copia)
    let friction = this.velocity.copy();
    friction.normalize();       // Nos quedamos solo con la dirección
    friction.mult(-0.02);       // Coeficiente de fricción: ajustar para mayor o menor efecto
    this.applyForce(friction);  // Se suma a la aceleración, junto con otras fuerzas aplicadas
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  display() {
    stroke(0);
    strokeWeight(2);
    fill(255,127, 127);
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
  createCanvas(640, 640);
  mover = new Mover();
  createP("Esta simulación modela fricción: la fuerza que se opone al movimiento, sumada a la gravedad.");
}

function draw() {
  background(92,155,155);

  // Aplicamos una gravedad constante
  let gravity = createVector(0, 0.1);
  mover.applyForce(gravity);

  if (mouseIsPressed) {
  let wind = createVector(0.1, 0); // Aumenté la fuerza del viento
  mover.applyForce(wind);
}

  
  // Aplicamos fricción: fuerza opuesta a la dirección del movimiento
  mover.applyFriction();

  mover.update();
  mover.display();
  mover.checkEdges();
}
```
