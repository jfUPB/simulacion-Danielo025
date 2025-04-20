#### Conceptos anteriores (Aleatoriedad y Fuerzas)

En la simulación se combinó la aleatoriedad para generar variedad en los comportamientos y colores, junto con una simulación de fuerza para hacer que las trayectorias de las bolitas sean curvas,
vivas y con un comportamiento físico más natural y atractivo, pero que este se vea afectado por la fuerza gravitacional de un gran atractor el cual se puede arrastrar con el mouse.

[Aleatoriedad y Fuerzas](https://editor.p5js.org/Danielo025/full/4xXieTj_x)


![Inicio](https://github.com/user-attachments/assets/84934133-9e3b-4e99-b4be-01c25e0cfdab)
![Final](https://github.com/user-attachments/assets/41bd927f-530d-4138-90d6-9899baf0df42)

```js
class Attractor {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.dragging = false;
    this.rollover = false;
    this.mass = 30;
    this.G = 1;
  }

  attract(oscillator) {
    let force = p5.Vector.sub(this.position, oscillator.position);
    let distance = force.mag();
    distance = constrain(distance, 5, 25);

    let strength = (this.G * this.mass * oscillator.mass) / (distance * distance);
    force.setMag(strength);
    return force;
  }

  over() {
    let d = dist(mouseX, mouseY, this.position.x, this.position.y);
    this.rollover = d < this.mass;
  }

  pressed() {
    if (this.rollover) {
      this.dragging = true;
    }
  }

  released() {
    this.dragging = false;
  }

  update() {
    if (this.dragging) {
      this.position.x = mouseX;
      this.position.y = mouseY;
    }
  }

  display() {
    stroke(0);
    fill(this.dragging ? color(50) : this.rollover ? color(150) : color(200));
    ellipse(this.position.x, this.position.y, this.mass * 2);
  }
}

class Oscillator {
  constructor() {
    this.position = createVector(random(width), random(height));
    this.velocity = createVector();
    this.acceleration = createVector();
    this.angle = createVector(random(TWO_PI), random(TWO_PI));
    this.angleVelocity = createVector(random(-0.05, 0.05), random(-0.05, 0.05));
    this.amplitude = createVector(random(20, width / 2), random(20, height / 2));
    this.mass = random(1, 3);
    this.hue = random(360); // Valor inicial del color
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.velocity.mult(0.95);
    this.acceleration.mult(0);

    this.angle.add(this.angleVelocity);

    this.hue = (this.hue + 1) % 360; // Actualiza el color constantemente
  }

  show() {
    let x = sin(this.angle.x) * this.amplitude.x;
    let y = sin(this.angle.y) * this.amplitude.y;

    push();
    translate(this.position.x, this.position.y);
    stroke(0);
    strokeWeight(2);
    fill(this.hue, 100, 100); // Color basado en el valor de hue
    line(0, 0, x, y);
    circle(x, y, this.mass * 8);
    pop();
  }
}

let oscillators = [];
let attractor;

function setup() {
  createCanvas(800, 700);
  colorMode(HSB, 360, 100, 100); // Cambia el modo de color
  attractor = new Attractor();

  for (let i = 0; i < 15; i++) {
    oscillators.push(new Oscillator());
  }
}

function draw() {
  background(0, 0, 70); // Blanco en modo HSB
  attractor.over();
  attractor.update();
  attractor.display();

  for (let osc of oscillators) {
    let force = attractor.attract(osc);
    osc.applyForce(force);
    osc.update();
    osc.show();
  }
}

function mousePressed() {
  attractor.pressed();
}

function mouseReleased() {
  attractor.released();
}
```
