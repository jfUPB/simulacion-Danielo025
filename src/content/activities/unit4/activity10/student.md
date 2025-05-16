#### Péndulos en serie

Se modificó el código original para crear dos péndulos conectados en serie, el segundo péndulo toma como punto de pivote el extremo del primero. Se añadió un nuevo objeto Pendulum con su propia longitud,
color y dinámica, y se actualiza su posición en cada cuadro usando la posición actualizada del primero.

[Péndulos en serie](https://editor.p5js.org/Danielo025/full/p6TXd6rFd)

![Pend1](https://github.com/user-attachments/assets/b29fba04-52b8-48b3-a05c-5ba67b324753)
![Pend2](https://github.com/user-attachments/assets/32b676af-3f8e-4f03-a77f-3287f5e115a9)

Código final:

```js
class Pendulum {
  constructor(pivot, r, color) {
    this.pivot = pivot.copy(); 
    this.bob = createVector();
    this.r = r;
    this.angle = PI / 4;

    this.angleVelocity = 0.0;
    this.angleAcceleration = 0.0;
    this.damping = 0.995;
    this.ballr = 24.0;
    this.color = color;
    this.dragging = false;
  }

  update() {
    if (!this.dragging) {
      let gravity = 0.4;
      this.angleAcceleration = ((-1 * gravity) / this.r) * sin(this.angle);

      this.angleVelocity += this.angleAcceleration;
      this.angle += this.angleVelocity;

      this.angleVelocity *= this.damping;
    }
  }

  show() {
    this.bob.set(this.r * sin(this.angle), this.r * cos(this.angle));
    this.bob.add(this.pivot);

    stroke(0);
    strokeWeight(2);
    line(this.pivot.x, this.pivot.y, this.bob.x, this.bob.y);

    fill(this.color);
    circle(this.bob.x, this.bob.y, this.ballr * 2);
  }

  clicked(mx, my) {
    let d = dist(mx, my, this.bob.x, this.bob.y);
    if (d < this.ballr) {
      this.dragging = true;
    }
  }

  stopDragging() {
    this.angleVelocity = 0;
    this.dragging = false;
  }

  drag() {
    if (this.dragging) {
      let diff = p5.Vector.sub(this.pivot, createVector(mouseX, mouseY));
      this.angle = atan2(-1 * diff.y, diff.x) - radians(90);
    }
  }

  getBobPosition() {
    return this.bob.copy();
  }
}

let pendulum1, pendulum2;

function setup() {
  createCanvas(800, 500);
  pendulum1 = new Pendulum(createVector(width / 2, 100), 175, color(150, 100, 255));
  pendulum2 = new Pendulum(pendulum1.getBobPosition(), 125, color(255, 100, 100));
}

function draw() {
  background(255);

  pendulum1.update();
  pendulum1.drag();
  pendulum1.show();

  // Actualiza el segundo péndulo con la posición del pivote del primero
  pendulum2.pivot = pendulum1.getBobPosition();
  pendulum2.update();
  pendulum2.drag();
  pendulum2.show();
}

function mousePressed() {
  pendulum1.clicked(mouseX, mouseY);
  pendulum2.clicked(mouseX, mouseY);
}

function mouseReleased() {
  pendulum1.stopDragging();
  pendulum2.stopDragging();
}

```


