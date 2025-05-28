#### Animando mi palabra

- *Palabra escogida*

  En mi caso utilizaré la palabra "Espacio", mi idea es complementar lo que ya había descrito en la actividad uno con lo siguiente: Hacer que la O se vuelva un sol al dar click en ella, que esta empiece a girar y las
  demás letras de la palabra a orbitar alrededor de ella, es decir, expicando todo nuevamente, la idea es que la palabra espacio, se encuentre en un entorno negro, un canvas casi totalmente oscuro, que estas letras
  empiecen a flotar libremente por el canvas sin salir de los límites, y que haya interacción del mouse específica y únicamente con la letra O, al dar click en esta, esta adoptará la forma de un sol y
  empezará a girar, y las demás letras asumirán una órbita alrededor de esta, haciendo de esto una especie de sistema solar.

- *Descripción técnica*

  Las letras fueron formadas como cuerpos circulares usando Matter.js, distribuidos inicialmente de manera horizontal y con físicas realistas como rebote (restitution) y
  resistencia al aire (frictionAir). Se añadieron cuerpos invisibles en los bordes para evitar que salieran del canvas. Las letras flotan aleatoriamente por el espacio gracias a pequeñas fuerzas generadas con ruido
  Perlin, simulando un ambiente con gravedad casi nula. Al hacer clic en la letra “O”, esta se transforma en un sol gráfico que rota y emite rayos, reemplazando el texto original. Las demás letras cambian su
  comportamiento para orbitar suavemente alrededor del sol, calculando su radio y ángulo de órbita en función de su posición actual. No se usaron restricciones (Constraint) porque los movimientos orbitantes 
  se controlan manualmente mediante funciones de rotación.

  ----

- *[Palabra ESPACIO](https://editor.p5js.org/Danielo025/full/3uJzdC6ZR)*

- *[GIF en Acción](https://drive.google.com/file/d/1TKnX4B-sgX5H08VkpMI4IremkDF8HPft/view?usp=sharing)*


*Código final:*

```js
class Letra {
  constructor(char, x, y, index) {
    this.char = char;
    this.body = Bodies.circle(x, y, 30, {
      restitution: 1,
      frictionAir: 0.05
    });
    this.radius = 30;
    this.index = index;
    this.angle = 0;
    this.isSol = false;
    this.orbitAngle = random(TWO_PI);
    this.orbitRadius = 100;
    this.noiseOffset = random(100); 
    World.add(world, this.body);
  }

  contains(x, y) {
    let pos = this.body.position;
    return dist(x, y, pos.x, pos.y) < this.radius;
  }

  convertToSol() {
    this.isSol = true;
    this.radius = 50;
  }

  setOrbit(center) {
    let pos = this.body.position;
    let cx = center.body.position.x;
    let cy = center.body.position.y;
    this.orbitRadius = dist(pos.x, pos.y, cx, cy);
    this.orbitAngle = atan2(pos.y - cy, pos.x - cx);
  }

  orbitAround(center) {
    this.orbitAngle += 0.01;
    let r = this.orbitRadius;
    let cx = center.body.position.x;
    let cy = center.body.position.y;
    let x = cx + r * cos(this.orbitAngle);
    let y = cy + r * sin(this.orbitAngle);
    Body.setPosition(this.body, { x, y });
  }

  update() {
    if (!this.isSol && !clicked) {
      let n = noise(this.noiseOffset + frameCount * 0.005);
      let angle = n * TWO_PI;
      let forceMag = 0.0005;
      let fx = cos(angle) * forceMag;
      let fy = sin(angle) * forceMag;
      Body.applyForce(this.body, this.body.position, { x: fx, y: fy });
    }
  }

  display() {
    let pos = this.body.position;
    push();
    translate(pos.x, pos.y);

    if (this.isSol) {
      rotate(this.angle);
      fill(255, 204, 0);
      noStroke();
      for (let i = 0; i < 12; i++) {
        let len = 25 + 5 * sin(frameCount * 0.2 + i);
        let angle = TWO_PI / 5 * i;
        line(0, 0, len * cos(angle), len * sin(angle));
      }
      fill(255, 204, 0);
      ellipse(0, 0, this.radius * 2);
    } else {
      fill(255);
      noStroke();
      textAlign(CENTER, CENTER);
      textSize(32);
      text(this.char, 0, 0);
    }

    pop();
  }
}

const { Engine, World, Bodies, Body } = Matter;

let engine, world;
let letras = [];
let palabra = "ESPACIO";
let letraSol = null;
let clicked = false;
let dragging = false;
let offsetX = 0, offsetY = 0;
let bounds = [];

function setup() {
  createCanvas(1920, 1080);
  engine = Engine.create();
  world = engine.world;
  engine.world.gravity.y = 0; 

  let thickness = 50;
  bounds.push(Bodies.rectangle(width / 2, -thickness / 2, width, thickness, { isStatic: true }));
  bounds.push(Bodies.rectangle(width / 2, height + thickness / 2, width, thickness, { isStatic: true }));
  bounds.push(Bodies.rectangle(-thickness / 2, height / 2, thickness, height, { isStatic: true }));
  bounds.push(Bodies.rectangle(width + thickness / 2, height / 2, thickness, height, { isStatic: true }));
  World.add(world, bounds);

  let startX = width / 2 - palabra.length * 40;
  for (let i = 0; i < palabra.length; i++) {
    let letra = palabra[i];
    let x = startX + i * 80;
    let y = height / 2 + random(-100, 100);
    letras.push(new Letra(letra, x, y, i));
  }
}

function draw() {
  background(10);
  Engine.update(engine);

  for (let letra of letras) {
    letra.update();
    letra.display();
  }

  if (clicked && letraSol) {
    letraSol.angle += 0.05;
    for (let letra of letras) {
      if (letra !== letraSol) {
        letra.orbitAround(letraSol);
      }
    }
  }
}

function mousePressed() {
  for (let letra of letras) {
    if (letra.contains(mouseX, mouseY) && letra.char === 'O' && !clicked) {
      letraSol = letra;
      letraSol.convertToSol();
      clicked = true;

      for (let l of letras) {
        if (l !== letraSol) {
          l.setOrbit(letraSol);
        }
      }
    }

    if (clicked && letra === letraSol && letra.contains(mouseX, mouseY)) {
      dragging = true;
      offsetX = mouseX - letra.body.position.x;
      offsetY = mouseY - letra.body.position.y;
    }
  }
}

function mouseReleased() {
  dragging = false;
}

function mouseDragged() {
  if (dragging && letraSol) {
    Body.setPosition(letraSol.body, { x: mouseX - offsetX, y: mouseY - offsetY });
  }
}
```
