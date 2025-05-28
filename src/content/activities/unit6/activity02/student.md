#### Análisis de campos de flujo

- *Estructura del campo de flujo*
  
  El campo de flujo se construye como una matriz bidimensional de vectores (p5.Vector) que representan direcciones en cada casilla del canvas. Esta matriz se define según el ancho y alto de la pantalla dividido por
  la resolución, lo que determina cuántas columnas y filas tendrá el campo. Para rellenarla, se utiliza ruido de Perlin, que permite generar direcciones suaves y coherentes entre sí. En cada celda, se genera un ángulo a
  partir del ruido y se convierte en un vector de dirección, logrando así una red de vectores que simula un flujo natural, como el de viento o corrientes.

- *Comportamiento del agente*
  Cada agente (o vehículo) sigue el campo de flujo buscando el vector correspondiente a su posición actual dentro de la matriz. Este vector indica la dirección deseada en la que debería moverse. El agente lo escala a
  su velocidad máxima (maxspeed), y luego calcula una fuerza de giro (steering) restando su velocidad actual a la dirección deseada. Esta fuerza se limita por su capacidad máxima de giro (maxforce) y se aplica como
  aceleración, haciendo que el agente poco a poco se alinee con el flujo, en lugar de cambiar bruscamente de dirección. Esto da lugar a movimientos suaves y naturales.

- *Parámetros que controlan el comportamiento*
  Los tres parámetros principales que afectan el comportamiento de los vehículos son la resolución del campo, la velocidad máxima (maxspeed) y la fuerza máxima de giro (maxforce). La resolución determina el tamaño
  de las celdas del campo: una resolución baja crea más detalle y más cambios en las direcciones del flujo, mientras que una alta simplifica el movimiento. La velocidad máxima define qué tan rápido puede moverse
  un agente, y la fuerza máxima de giro limita qué tan bruscamente puede cambiar de dirección. Estos parámetros trabajan en conjunto para balancear entre un movimiento fluido, controlado y natural o uno más brusco y reactivo.


- *Modificación de código*

  Se modificaron los parámetros del noise() para hacerlo dinámico en el tiempo usando frameCount, lo que genera un campo de flujo en constante cambio, como si el entorno fluyera o respirara. Además, se hizo la
  resolución del campo más fina (de 20 a 10), duplicando la cantidad de vectores y haciendo que los vehículos reaccionen a cambios más detallados en su entorno. Finalmente, se aumentaron los valores de maxspeed y
  maxforce, lo que hace que los agentes se muevan más rápido y hagan giros más bruscos, resultando en trayectorias más agitadas, sensibles y caóticas.

[Modificación de campos de flujo](https://editor.p5js.org/Danielo025/full/2T8ZQXM5K)
  
![image](https://github.com/user-attachments/assets/5cdddb4f-4e5e-4081-9129-e9b37a2c8507)
![image](https://github.com/user-attachments/assets/1c59c9ef-2bcb-4f60-8280-84552de0802c)

```js
class FlowField {
  constructor(cols, rows) {
    this.cols = cols;
    this.rows = rows;
    this.field = Array.from({ length: cols }, () =>
      Array.from({ length: rows }, () => createVector(0, 0))
    );
    this.init();
  }

  // Generación de vectores usando noise animado
  init() {
    let scale = 0.2; // más frecuencia = más detalle
    for (let i = 0; i < this.cols; i++) {
      for (let j = 0; j < this.rows; j++) {
        let angle = map(noise(i * scale, j * scale, frameCount * 0.01), 0, 1, 0, TWO_PI);
        this.field[i][j] = p5.Vector.fromAngle(angle);
      }
    }
  }

  show() {
    for (let i = 0; i < this.cols; i++) {
      for (let j = 0; j < this.rows; j++) {
        let w = width / this.cols;
        let h = height / this.rows;
        let v = this.field[i][j].copy();
        v.setMag(w * 0.5);
        let x = i * w + w / 2;
        let y = j * h + h / 2;
        strokeWeight(1);
        stroke(180, 100, 200); // Color HSB
        line(x, y, x + v.x, y + v.y);
      }
    }
  }

  lookup(position) {
    let column = constrain(floor(position.x / (width / this.cols)), 0, this.cols - 1);
    let row = constrain(floor(position.y / (height / this.rows)), 0, this.rows - 1);
    return this.field[column][row].copy();
  }
}

class Vehicle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = p5.Vector.random2D();
    this.acceleration = createVector(0, 0);
    this.r = 4;
    this.maxspeed = 7;   // ← Aumentado
    this.maxforce = 0.4; // ← Aumentado
  }

  run() {
    this.update();
    this.borders();
    this.show();
  }

  follow(flow) {
    let desired = flow.lookup(this.position);
    desired.mult(this.maxspeed);
    let steer = p5.Vector.sub(desired, this.velocity);
    steer.limit(this.maxforce);
    this.applyForce(steer);
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxspeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  borders() {
    if (this.position.x < -this.r) this.position.x = width + this.r;
    if (this.position.y < -this.r) this.position.y = height + this.r;
    if (this.position.x > width + this.r) this.position.x = -this.r;
    if (this.position.y > height + this.r) this.position.y = -this.r;
  }

  show() {
    let theta = this.velocity.heading();
    fill(127);
    stroke(0);
    strokeWeight(2);
    push();
    translate(this.position.x, this.position.y);
    rotate(theta);
    beginShape();
    vertex(this.r * 2, 0);
    vertex(-this.r * 2, -this.r);
    vertex(-this.r * 2, this.r);
    endShape(CLOSE);
    pop();
  }
}

// Variables globales
let debug = true;
let flowfield;
let vehicles = [];

function setup() {
  createCanvas(640, 640);
  colorMode(HSB, 255); // ← Para ver mejor el campo si quieres usar color

  // Campo más fino: más columnas y filas
  let cols = 80;
  let rows = 60;
  flowfield = new FlowField(cols, rows);

  for (let i = 0; i < 120; i++) {
    vehicles.push(new Vehicle(random(width), random(height)));
  }
}

function draw() {
  background(255);

  flowfield.init(); // ← Campo animado frame a frame

  if (debug) flowfield.show();

  for (let i = 0; i < vehicles.length; i++) {
    vehicles[i].follow(flowfield);
    vehicles[i].run();
  }
}

function keyPressed() {
  if (key === ' ') {
    debug = !debug;
  }
}

function mousePressed() {
  flowfield.init();
}
```







