#### Codificando la idea:

- El único cambio que hice realmente fue de estética, ahora los vectores de salto de posición no quedaron rojos, sino que van cambiando aleatoriamente de salto en salto, esto para que se vea algo un poco diferente, y no tan monótono y simple

[Aquí se puede probar la aplicación](https://editor.p5js.org/Danielo025/full/L5jCOf5yA)

Se vería algo así:

![image](https://github.com/user-attachments/assets/2a839b2e-3a03-426e-b5d1-73c889ed1567)


Y así quedaría el código:

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
    // Dibujar el rastro normal
    stroke(255, 165, 0, 150);
    strokeWeight(2);
    noFill();
    beginShape();
    for (let p of this.trail) {
      vertex(p.x, p.y);
    }
    endShape();

    fill(95, 200, 200);
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
    let randomColor = color(random(255), random(255), random(255));
    teleportLines.push({start: this.position.copy(), end: createVector(x, y), color: randomColor}); // Guarda la línea de teletransporte con color aleatorio
    this.position.set(x, y);
  }
}

let mover;
let clickPositions = []; // Lista para almacenar los clics
let teleportLines = []; // Lista para almacenar las líneas de teletransporte

function setup() {
  createCanvas(640, 640);
  mover = new Mover();
}

function draw() {
  background(155, 155, 155);

  // Dibujar las líneas de teletransporte con flecha y color aleatorio
  strokeWeight(3);
  for (let lineData of teleportLines) {
    stroke(lineData.color);
    line(lineData.start.x, lineData.start.y, lineData.end.x, lineData.end.y);
    drawArrow(lineData.start, lineData.end, lineData.color);
  }

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

function drawArrow(start, end, color) {
  push();
  let angle = atan2(end.y - start.y, end.x - start.x);
  let arrowSize = 10;
  translate(end.x, end.y);
  rotate(angle);
  fill(color);
  noStroke();
  triangle(-arrowSize, -arrowSize / 2, -arrowSize, arrowSize / 2, 0, 0);
  pop();
}
```
