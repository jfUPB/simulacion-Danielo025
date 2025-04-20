#### Resortes en serie

Al código se añadió una segunda masa conectada a la primera mediante un segundo resorte, formando un sistema en serie; se creó un nuevo método connectBetween en la clase Spring para
aplicar la fuerza de resorte entre dos masas, y se actualizó el draw() para aplicar gravedad, actualizar, arrastrar, conectar y dibujar ambos bobs y resortes.

[Doble Resorte](https://editor.p5js.org/Danielo025/full/v4i-93v9L)

![Resorte1](https://github.com/user-attachments/assets/8e48dac1-9629-4e3c-866c-916dcfc63b99)
![Resorte2](https://github.com/user-attachments/assets/41dcffd7-bfca-45f6-91bd-7004057d2a2b)

Código final:

```js
class Bob {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector();
    this.acceleration = createVector();
    this.mass = 24;
    this.damping = 0.98;
    this.dragOffset = createVector();
    this.dragging = false;
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.mult(this.damping);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  applyForce(force) {
    let f = force.copy();
    f.div(this.mass);
    this.acceleration.add(f);
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(this.dragging ? 200 : 127);
    circle(this.position.x, this.position.y, this.mass * 2);
  }

  handleClick(mx, my) {
    let d = dist(mx, my, this.position.x, this.position.y);
    if (d < this.mass) {
      this.dragging = true;
      this.dragOffset.x = this.position.x - mx;
      this.dragOffset.y = this.position.y - my;
    }
  }

  stopDragging() {
    this.dragging = false;
  }

  handleDrag(mx, my) {
    if (this.dragging) {
      this.position.x = mx + this.dragOffset.x;
      this.position.y = my + this.dragOffset.y;
    }
  }
}

class Spring {
  constructor(anchorX, anchorY, length) {
    this.anchor = createVector(anchorX, anchorY);
    this.restLength = length;
    this.k = 0.2;
  }

  // If connected to a fixed anchor
  connect(bob) {
    let force = p5.Vector.sub(bob.position, this.anchor);
    let stretch = force.mag() - this.restLength;
    force.setMag(-1 * this.k * stretch);
    bob.applyForce(force);
  }

  // If connected between two bobs
  connectBetween(bobA, bobB) {
    let force = p5.Vector.sub(bobB.position, bobA.position);
    let stretch = force.mag() - this.restLength;
    force.setMag(this.k * stretch);
    bobA.applyForce(force);
    force.mult(-1);
    bobB.applyForce(force);
  }

  constrainLength(bob, minlen, maxlen) {
    let dir = p5.Vector.sub(bob.position, this.anchor);
    let len = dir.mag();

    if (len < minlen) {
      dir.setMag(minlen);
      bob.position = p5.Vector.add(this.anchor, dir);
      bob.velocity.mult(0);
    } else if (len > maxlen) {
      dir.setMag(maxlen);
      bob.position = p5.Vector.add(this.anchor, dir);
      bob.velocity.mult(0);
    }
  }

  show() {
    fill(127);
    circle(this.anchor.x, this.anchor.y, 10);
  }

  showLineTo(bob) {
    stroke(0);
    line(bob.position.x, bob.position.y, this.anchor.x, this.anchor.y);
  }

  showLineBetween(bobA, bobB) {
    stroke(0);
    line(bobA.position.x, bobA.position.y, bobB.position.x, bobB.position.y);
  }
}

let bob1, bob2;
let spring1, spring2;

function setup() {
  createCanvas(640, 400);

  spring1 = new Spring(width / 2, 20, 100); // ancla a bob1
  bob1 = new Bob(width / 2, 120);

  spring2 = new Spring(0, 0, 100); // conectará bob1 a bob2 (no usa ancla real)
  bob2 = new Bob(width / 2, 220);
}

function draw() {
  background(255);

  let gravity = createVector(0, 2);
  bob1.applyForce(gravity);
  bob2.applyForce(gravity);

  bob1.update();
  bob2.update();

  bob1.handleDrag(mouseX, mouseY);
  bob2.handleDrag(mouseX, mouseY);

  spring1.connect(bob1);
  spring2.connectBetween(bob1, bob2);

  spring1.constrainLength(bob1, 30, 200);

  spring1.showLineTo(bob1);
  spring2.showLineBetween(bob1, bob2);

  spring1.show();
  bob1.show();
  bob2.show();
}

function mousePressed() {
  bob1.handleClick(mouseX, mouseY);
  bob2.handleClick(mouseX, mouseY);
}

function mouseReleased() {
  bob1.stopDragging();
  bob2.stopDragging();
}

```
