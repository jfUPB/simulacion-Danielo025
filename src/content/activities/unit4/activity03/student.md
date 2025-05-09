#### Practicando los conceptos

Aquí se hizo justo lo que se pidió, un vehículo que acelera de acuerdo a qué flecha presione, al presionar la derecha apunta y va hacia la derecha, y viceversa

[Movimiento de Vehículo](https://editor.p5js.org/Danielo025/full/mTpmAEYZc)

![Derecha](https://github.com/user-attachments/assets/e3cc63e6-9f51-489a-9355-7b0f3807c335)
![Izquierda](https://github.com/user-attachments/assets/a2f8fa6c-d1f8-429d-81e3-a41633137afc)


Código completo:

```js
class Vehicle {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.topspeed = 5;
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topspeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0); // Reset acceleration each frame
  }

  display() {
    let angle = this.velocity.heading(); // Direction of movement

    push();
    translate(this.position.x, this.position.y);
    rotate(angle);

    // Dibujo del vehículo como triángulo
    fill(255,127,127);
    stroke(0);
    strokeWeight(2);
    beginShape();
    vertex(20, 0);
    vertex(-10, 10);
    vertex(-10, -10);
    endShape(CLOSE);

    pop();
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

let vehicle;

function setup() {
  createCanvas(640, 240);
  vehicle = new Vehicle();
}

function draw() {
  background(155,155,155);

  // Control con flechas
  if (keyIsDown(LEFT_ARROW)) {
    let leftForce = createVector(-0.2, 0);
    vehicle.applyForce(leftForce);
  }

  if (keyIsDown(RIGHT_ARROW)) {
    let rightForce = createVector(0.2, 0);
    vehicle.applyForce(rightForce);
  }

  vehicle.update();
  vehicle.checkEdges();
  vehicle.display();
}

```
