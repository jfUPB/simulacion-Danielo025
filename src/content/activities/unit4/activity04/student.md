#### Relación con el marco 101

- En este caso, el marco 101, se presenta claramente al final de update, es un hecho que ya está implementado en el código, y esto se puede notar en cada actualización de pantalla, todo se mueve como debería y se 
reinician las fuerzas para que estas no se acumulen indefinidamente, así que no hay que modificar nada en cuanto al marco 101.

- El attractor se presenta como un elipse en toda la mitad de la pantalla, cambiarle el color no es complicado, pues solo se trata de poner valores diferentes en la línea de "fill".

- Por último queda hacer que el attractor se mueva y reaccione visualmente con el mouse utilizando los atributos que ya están en el código.

Con estos puntos en cuenta el código se terminaría viendo así:

[Atractor arrastrado](https://editor.p5js.org/Danielo025/full/bcYtzhfGb)

![Attractor rojo](https://github.com/user-attachments/assets/19a8c819-04c8-47a8-b98a-ce8040664f34)
![Attractor negro](https://github.com/user-attachments/assets/ad284c6a-e89b-41d8-8d8b-64b59049ac89)




```js
class Attractor {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.mass = 20;
    this.G = 1;
    this.dragging = false;
    this.rollover = false;
    this.dragOffset = createVector(0, 0);
  }

  attract(mover) {
    let force = p5.Vector.sub(this.position, mover.position);
    let distance = force.mag();
    distance = constrain(distance, 5, 25);
    let strength = (this.G * this.mass * mover.mass) / (distance * distance);
    force.setMag(strength);
    return force;
  }

  hover(mx, my) {
    let d = dist(mx, my, this.position.x, this.position.y);
    this.rollover = d < this.mass;
  }

  pressed(mx, my) {
    let d = dist(mx, my, this.position.x, this.position.y);
    if (d < this.mass) {
      this.dragging = true;
      this.dragOffset.x = this.position.x - mx;
      this.dragOffset.y = this.position.y - my;
    }
  }

  released() {
    this.dragging = false;
  }

  drag(mx, my) {
    if (this.dragging) {
      this.position.x = mx + this.dragOffset.x;
      this.position.y = my + this.dragOffset.y;
    }
  }

  display() {
    ellipseMode(CENTER);
    stroke(0);
    if (this.dragging) {
      fill(50); // Más oscuro
    } else if (this.rollover) {
      fill(100); // Intermedio
    } else {
      fill(255, 100, 100); // Cambiado a un tono rojizo para destacarlo
    }
    ellipse(this.position.x, this.position.y, this.mass * 2);
  }
}

class Mover {
  constructor(x, y, mass) {
    this.mass = mass;
    this.radius = this.mass * 8;
    this.position = createVector(x, y);
    this.angle = 0;
    this.angleVelocity = 0;
    this.angleAcceleration = 0;
    this.velocity = createVector(random(-1, 1), random(-1, 1));
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.angleAcceleration = this.acceleration.x / 10.0;
    this.angleVelocity += this.angleAcceleration;
    this.angleVelocity = constrain(this.angleVelocity, -0.1, 0.1);
    this.angle += this.angleVelocity;
    this.acceleration.mult(0); // acumulación reseteada
  }

  show() {
    strokeWeight(2);
    stroke(0);
    fill(127, 127);
    rectMode(CENTER);
    push();
    translate(this.position.x, this.position.y);
    rotate(this.angle);
    circle(0, 0, this.radius * 2);
    line(0, 0, this.radius, 0);
    pop();
  }
}

let movers = [];
let attractor;

function setup() {
  createCanvas(640, 240);

  for (let i = 0; i < 20; i++) {
    movers.push(new Mover(random(width), random(height), random(0.1, 2)));
  }
  attractor = new Attractor();
}

function draw() {
  background(255);

  attractor.hover(mouseX, mouseY);
  attractor.drag(mouseX, mouseY);
  attractor.display();

  for (let mover of movers) {
    let force = attractor.attract(mover);
    mover.applyForce(force);
    mover.update();
    mover.show();
  }
}

function mousePressed() {
  attractor.pressed(mouseX, mouseY);
}

function mouseReleased() {
  attractor.released();
}
```
