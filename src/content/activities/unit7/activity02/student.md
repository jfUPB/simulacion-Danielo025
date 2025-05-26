#### Implementación de matter.js

- *Explicación de conceptos clave:*

  *Engine:* Es tal tal y como su nombre lo dice, el “motor” que pone todo a funcionar en Matter.js. Es el que se encarga de calcular todas las propiedades físicas: gravedad, colisiones, velocidad, etc. 
  Cuando uno crea un Engine, básicamente está encendiendo el simulador del mundo físico, es fundamental.

  *World:* Es básicamente el mundo, el espacio, el lugar, el contexto donde pasan las cosas. Ahí es donde se agregan todos los objetos (como cajas, círculos, el suelo, las cuerdas, todo).
  El mundo es parte del Engine, y sin él, los objetos no tienen dónde estar ni cómo interactuar entre sí.

  *Bodies:* Son los objetos físicos que existen en el mundo y hay diferentes tipos: rectángulos, círculos, polígonos, son los bloques con los que se construyen las escenas físicas.
  Uno puede hacerlos dinámicos (que se muevan y caigan) o estáticos (que no se muevan, como el suelo), simplemente agregando una función.

  *Constraint:* Es como una cuerda o conexión entre dos cuerpos, esta puede servir para que dos objetos se muevan juntos o con cierta restricción. 
  Por ejemplo, una cuerda o una barra que une dos objetos, se puede usar para crear efectos como péndulos, grúas, o articulaciones.

  *MouseConstraint* Es como una función que permite mover objetos con el mouse, básicamente uno puede agarrar un objeto con el clic y moverlo, y este se comporta como si lo estuvieras arrastrando físicamente,.
   con peso y todo, lo puedes tirar, empujar, de todo

- *Problemas encontrados*

  Uno de los problemas principales fue la referenciación en el archivo HTML, no sabía muy bien por qué muchas veces a pesar de haber puesto todos los js me seguía dando error de no encontrar la referencia, sin embargo,
  después de buscar el por qué de esto me di cuenta que el orden importa, cosa que honestamente ignoraba por completo, no sabía que era importante que el sketch fuera el último por ejemplo, y cosas así, ya que realmente
  con los códigos que había hecho hasta el momento, lo que hacía era duplicar el código base que se daba y se trabajaba en él, pero esto, al ser otra implementación tocó crear un archivo limpio y eso fue algo confuso.

  Otro problema fue traducir algunas funciones al lenguaje de p5js, en el video de Patt esto fue explícito, pero aún así habían cosas que al ser a mi parecer redundantes, me confundían un poco.

----

#### - *Experimentación con códigos*

*[Código 1 - Cuadritos en un piso - Ejemplo del video de Patt Vira](https://editor.p5js.org/Danielo025/full/WDcw5xzyw)*

![image](https://github.com/user-attachments/assets/7137a32d-777b-4f72-b7a0-0f918bc765f1)
![image](https://github.com/user-attachments/assets/2a3c83e5-7401-44c2-bea7-1fd56fea550f)


*[Código 2 - Bolitas en rampa - Ejemplo de página web Matter.js](https://editor.p5js.org/Danielo025/full/HE0Qjz_nw)*

![image](https://github.com/user-attachments/assets/4519684a-fd44-49f5-9d12-62b17f61139d)
![image](https://github.com/user-attachments/assets/96cff38a-f676-4b80-8a06-a1d31bbb9efe)


----

- Código 1:

```js
class Ground {
  constructor(x, y, w, h) {
    this.w = w;
    this.h = h;
    this.body = Bodies.rectangle(x, y, w, h, { isStatic: true });
    Composite.add(engine.world, this.body);
  }

  display() {
    let pos = this.body.position;
    push();
    fill(100);
    noStroke();
    rectMode(CENTER);
    rect(pos.x, pos.y, this.w, this.h);
    pop();
  }
}

class Rect {
  constructor(x, y, w, h) {
    this.w = w;
    this.h = h;
    this.body = Bodies.rectangle(x, y, w, h);
    Body.setAngularVelocity(this.body, random(0.05, 0.2)); // leve variación aleatoria
    Composite.add(engine.world, this.body);
  }

  display() {
    let pos = this.body.position;
    let angle = this.body.angle;

    push();
    translate(pos.x, pos.y);
    rotate(angle);
    rectMode(CENTER);
    stroke(50);
    fill(255);
    rect(0, 0, this.w, this.h);
    pop();
  }
}

const { Engine, Bodies, Composite, Body } = Matter;

let engine;
let ground;
let boxes = [];

function setup() {
  createCanvas(700, 700);
  engine = Engine.create();
  ground = new Ground(200, 380, 400, 20); 
}

function draw() {
  background(240);
  Engine.update(engine);

  for (let i = 0; i < boxes.length; i++) {
    boxes[i].display();
  }

  ground.display();
}

function mousePressed() {
  boxes.push(new Rect(mouseX, mouseY, 30, 30)); 
}
```


----


- Código 2:

```js
class Ball {
  constructor(x, y, r) {
    this.r = r;
    this.color = random([
      color(255, 234, 167), // amarillo claro
      color(255, 255, 240), // blanco
      color(255, 107, 107), // coral
      color(255, 177, 66),  // naranja
      color(28, 40, 51)     // azul oscuro
    ]);
    this.body = Matter.Bodies.circle(x, y, r, {
      restitution: 0.6,
      friction: 0.1
    });
    Matter.Composite.add(world, this.body);
  }

  show() {
    let pos = this.body.position;
    push();
    noStroke();
    fill(this.color);
    circle(pos.x, pos.y, this.r * 2);
    pop();
  }
}

class Ramp {
  constructor(x, y, w, h, angle) {
    this.w = w;
    this.h = h;
    this.body = Matter.Bodies.rectangle(x, y, w, h, {
      isStatic: true,
      angle: angle
    });
    Matter.Composite.add(world, this.body);
  }

  show() {
    let pos = this.body.position;
    let angle = this.body.angle;

    push();
    translate(pos.x, pos.y);
    rotate(angle);
    noFill();
    stroke(180);
    strokeWeight(1);
    rectMode(CENTER);
    rect(0, 0, this.w, this.h);
    pop();
  }
}

let Engine = Matter.Engine,
    World = Matter.World,
    Bodies = Matter.Bodies;

let engine, world;
let balls = [];
let ramps = [];

function setup() {
  createCanvas(windowWidth, windowHeight);
  engine = Engine.create();
  world = engine.world;
  let runner = Matter.Runner.create();
  Matter.Runner.run(runner, engine); 


  ramps.push(new Ramp(width / 2, height * 0.3, width * 0.8, 20, -0.2));
  ramps.push(new Ramp(width * 0.6, height * 0.5, width * 0.5, 20, 0.15));
  ramps.push(new Ramp(width * 0.3, height * 0.7, width * 0.6, 20, -0.1));

  for (let i = 0; i < 50; i++) {
    let x = random(width / 4, (3 * width) / 4);
    let y = random(-200, 0);
    let r = random(10, 20);
    balls.push(new Ball(x, y, r));
  }
}

function draw() {
  background(12, 14, 23); // Oscuro

  for (let ball of balls) {
    ball.show();
  }

  for (let ramp of ramps) {
    ramp.show();
  }
}

function mousePressed() {
  let r = random(10, 20);
  balls.push(new Ball(mouseX, mouseY, r));
}
```
  
