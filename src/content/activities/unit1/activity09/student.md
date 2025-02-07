De momento este es mi código, está en desarrollo, hasta ahora tiene el random walk y la tendencia hacia donde indiquen las 'WASD'


```js
let walker;
let noiseOffsetX = 0;
let noiseOffsetY = 1000;
let mode = 'randomWalk'; // Modo inicial
let moveDirection = { x: 0, y: 0 }; // Dirección de movimiento
let isMoving = false; // Indicador de si el caminante está en movimiento

function setup() {
  createCanvas(640, 640);
  walker = new Walker();
  background(93, 155, 155);
}

function draw() {
  // Verificar si alguna tecla de movimiento está siendo presionada
  if (keyIsDown(87)) { // 'W'
    moveDirection = { x: 0, y: -1 }; // Arriba
    isMoving = true;
  } else if (keyIsDown(83)) { // 'S'
    moveDirection = { x: 0, y: 1 }; // Abajo
    isMoving = true;
  } else if (keyIsDown(65)) { // 'A'
    moveDirection = { x: -1, y: 0 }; // Izquierda
    isMoving = true;
  } else if (keyIsDown(68)) { // 'D'
    moveDirection = { x: 1, y: 0 }; // Derecha
    isMoving = true;
  } else {
    moveDirection = { x: 0, y: 0 }; // No se presiona ninguna tecla
    isMoving = false; // El caminante está quieto
  }

  // Ejecutar el paso del caminante en el modo actual
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    strokeWeight(2);
    point(this.x, this.y);
    fill(255, 0, 0);
    noStroke();
    ellipse(this.x, this.y, 10, 10);
  }

  step() {
    if (mode === 'randomWalk') {
      this.randomWalk();
    } else if (mode === 'perlinNoise') {
      this.perlinMove();
    } else if (mode === 'levyFlight') {
      this.levyMove();
    }
  }

  randomWalk() {
    // Movimiento aleatorio con tendencia hacia la dirección
    this.x += moveDirection.x * 5 + floor(random(3)) - 1;
    this.y += moveDirection.y * 5 + floor(random(3)) - 1;
  }

  perlinMove() {
    // Movimiento con Perlin noise y tendencia hacia la dirección
    this.x += (noise(noiseOffsetX) - 0.5) * 10 + moveDirection.x * 5;
    this.y += (noise(noiseOffsetY) - 0.5) * 10 + moveDirection.y * 5;
    noiseOffsetX += 0.02;
    noiseOffsetY += 0.02;
  }

  levyMove() {
    // Movimiento de Levy Flight con tendencia hacia la dirección
    let stepSize = pow(random(1), -1.5) * 10;
    let angle = random(TWO_PI);
    this.x += cos(angle) * stepSize + moveDirection.x * 5;
    this.y += sin(angle) * stepSize + moveDirection.y * 5;
  }
}

function keyPressed() {
  if (key === 'F' && !isMoving) { // Activar perlinNoise solo cuando está quieto
    mode = 'perlinNoise';
  } else if (key === 'G' && !isMoving) { // Activar levyFlight solo cuando está quieto
    mode = 'levyFlight';
  } else if (key === 'H') {
    mode = 'randomWalk'; // Volver al modo aleatorio
  }
}
```
