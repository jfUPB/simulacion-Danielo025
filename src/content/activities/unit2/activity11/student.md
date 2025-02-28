#### Codificando la idea:

Así va el código:

```js
class Mover {
  constructor() {
    this.position = createVector(random(width), random(height));
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.trail = []; // Almacena todas las posiciones anteriores
  }

  update() {
    this.acceleration = createVector(random(-0.1, 0.1), random(-0.1, 0.1));
    this.velocity.add(this.acceleration);
    
    // Limitar la velocidad máxima
    this.velocity.limit(2);
    
    this.position.add(this.velocity);
    
    // Agregar la posición actual al rastro (ahora sin límite)
    this.trail.push(createVector(this.position.x, this.position.y));
  }

  show() {
    stroke(255, 165, 0, 150);
    strokeWeight(2);
    noFill();
    beginShape();
    for (let p of this.trail) {
      vertex(p.x, p.y);
    }
    endShape();
    
    fill(95,200,200);
    stroke(0);
    strokeWeight(3);
    circle(this.position.x, this.position.y, 25);
  }

  checkEdges() {
    if (this.position.x > width) this.position.x = 0;
    if (this.position.x < 0) this.position.x = width;
    if (this.position.y > height) this.position.y = 0;
    if (this.position.y < 0) this.position.y = height;
  }

  teleport(x, y) {
    this.position.set(x, y);
  }
}



let mover;
let clickPositions = []; // Lista para almacenar los clics

function setup() {
  createCanvas(640, 640);
  mover = new Mover();
}

function draw() {
  background(155,155,155);
  
  // Dibujar los puntos rojos de los clics anteriores
  fill(255, 0, 0);
  noStroke();
  for (let pos of clickPositions) {
    circle(pos.x, pos.y, 8);
  }

  mover.update();
  mover.checkEdges();
  mover.show();
}

function mousePressed() {
  mover.teleport(mouseX, mouseY);
  clickPositions.push(createVector(mouseX, mouseY)); // Guarda la posición del clic
}
```
