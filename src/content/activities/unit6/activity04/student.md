#### Aplicación de agentes autónomos 

###  *Comportamiento de la sociedad*

  En este caso lo que haré será integrar ambos conceptos, los campos de flujo y el comportamiento de enjambre, para mostrar una representación de cómo funciona la sociedad, de como en un 
  principio a las personas se nos guía por un camino específico, en conjunto, pero que a medida que crecemos, y tomamos nuestras propias decisiones a pesar de ser individuos con metas 
  específicas, nuestra necesidad de pertenecer a algo, y también por el hecho de ser seres sociales, nos vamos formando en grupos, siguiendo a aquellos cuyas ideas se conectan con las 
  nuestras, esas conexiones son más fuertes que cualquier otra influencia externa, pues al final del día nos alineamos con aquello que nos llena más, que nos hace más feliz, aquello para lo que "estamos hechos".

  Esta pequeña reflexión fue resultado directo de ver ambos comportamientos, y es que me parece que es una analogía interesante, el sistema está organizado de tal manera que hasta cierto punto
  de nuestra vida todos tenemos en general experiencias bastante similares, estudiamos, pasamos por cambios en nuestro cuerpo, llegan las críticas sociales y la expectativa de nuestros 
  familiares, y eventualmente tomamos la decisión, si queremos hacer algo bueno, algo malo, estudiar una cosa u otra, trabajar en un lugar u otro, y es entonces cuando todos esos campos de 
  flujo (la educación, la familia y la sociedad) se convierten en comportamientos de enjambres (las carreras universitarias, los puestos de trabajo, los equipos de deporte, etc).

  Así que lo que haré será una obra que integre eso de una forma visual, lo que se me ocurre es un comportamiento de campos de flujo en que seamos un boid, uno específico, tendremos la 
  posibilidad de controlarlo con las flechas, y aunque seremos atraídos por los campos de flujo y eventualmente por el comportamiento de enjambre, seremos nosotros quienes trataremos de encajar
  en uno o en otro, es decir, estando en la fase de campos de flujo, decidiremos a cuál entrar, y aunque en un principio nuestro color sea negro, al entrar en uno u otro adquiriremos el color 
  que estos tengan, adicionalmente habrá un interruptor en la parte superior, el cual al presionarlo, estos voids dejarán de seguir los campos de flujo (desaparecerán) y empezarán a 
  comportarse como enjambre, formándose nuevamente en distintos enjambres de colores (como el código original, pero con 4 colores limitados (Azul,Verde,Rojo y Morado), cada uno representando realidades sociales 
  istintas), y nuevamente tendremos la oportunidad de encajar en uno o en otro, de movernos a nuestra merced entre ellos, pero si nos metemos en alguno y nos dejamos de mover, seremos parte
  de ese sistema, debido a su evidente atracción.

  ---

#### *Documentación de proceso*

- Algoritmo elegido
  
  Para esta pieza decidí combinar los dos algoritmos estudiados: Flow Fields y Flocking. Me interesa cómo ambos pueden representar aspectos distintos pero complementarios del comportamiento social: uno como
  estructura invisible que guía, y otro como la respuesta colectiva que emerge de los individuos.

- Concepto de aplicación inesperada

  Mi propuesta representa el comportamiento de la sociedad humana desde una perspectiva simbólica. En lugar de simular fluidos o criaturas, uso los algoritmos para explorar cómo las personas buscan pertenecer a
  grupos, influenciadas por contextos sociales invisibles. El flujo representa fuerzas externas como educación, familia o cultura. El flocking representa la forma en que nos alineamos con otros, en búsqueda de identidad y conexión.

- Adaptación de la lógica del algoritmo

  Los campos de flujo guían los movimientos de todos los agentes en la escena, simulando esa estructura social que muchas veces no vemos, pero que nos empuja a movernos en ciertas direcciones. Al mismo
  tiempo, los agentes siguen un comportamiento de enjambre (flocking), buscando alinearse con otros cercanos. Cuando un agente entra en una zona de color, su comportamiento cambia: adquiere un nuevo
  color (representando una identidad) y se alinea con agentes del mismo color, reforzando esa identidad grupal.

- Interacción implementada

  El usuario controla un agente individual con las flechas del teclado. Este agente puede desplazarse libremente por los campos de flujo, pero al entrar en las "zonas de identidad" , adopta un nuevo comportamiento e
  identidad visual. A partir de ahí, se comporta como parte del grupo con el que se alineó. La pieza permite explorar activamente cómo tomamos decisiones y cómo estas decisiones nos convierten en parte de una estructura mayor.

----

#### "Código Final"

[Comportamiento Social](https://editor.p5js.org/Danielo025/full/9XfE9nTXG)


![Niñez1](https://github.com/user-attachments/assets/de2d83e3-c605-4db3-9c48-499c5a143f44)
![Niñez2](https://github.com/user-attachments/assets/e69fa23f-fd41-4a37-a351-3e4b092fa95d)
![Adultez1](https://github.com/user-attachments/assets/202bead5-c00a-427b-a530-d8f5ce0d5f30)
![Adultez2](https://github.com/user-attachments/assets/c1010294-b265-4df9-b494-a6be7c1732af)

*Nota:* Se aumentó la velocidad para poder ver la simulación de manera rápida, pero la idea es bajar la velocidad de los agentes para poder tener un comportamiento social más acorde al esperado


```js
class Boid {
  constructor(x, y, isPlayer) {
    this.position = createVector(x, y);
    this.velocity = p5.Vector.random2D();
    this.velocity.setMag(random(1, 2));
    this.acceleration = createVector();
    this.maxForce = 0.08;
    this.maxSpeed = isPlayer ? 3 : 2;
    this.isPlayer = isPlayer;
    this.r = isPlayer ? 10 : 6;

    this.group = floor(random(numGroups));
    this.setBaseColor();
  }

  setBaseColor() {
    if (this.isPlayer) {
      this.baseColor = color(0);
    } else {
      this.baseColor = mode === 'adultez'
        ? groupColors[this.group]
        : color(random(255), random(255), random(255));
    }
    this.currentColor = this.baseColor;
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxSpeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  edges() {
    if (this.position.x < 0) this.position.x = width;
    else if (this.position.x > width) this.position.x = 0;

    if (this.position.y < 0) this.position.y = height;
    else if (this.position.y > height) this.position.y = 0;
  }

  followFlowField(flowfield, cols) {
    let x = floor(this.position.x * flowfieldScale);
    let y = floor(this.position.y * flowfieldScale);
    let index = x + y * cols;
    let force = flowfield[index];
    this.applyForce(force);
  }

  flock(boids) {
    let perceptionRadius = 80;
    let alignment = createVector();
    let cohesion = createVector();
    let separation = createVector();
    let groupCenter = createVector();
    let total = 0;
    let sameGroupCount = Array(numGroups).fill(0);

    for (let other of boids) {
      let d = dist(this.position.x, this.position.y, other.position.x, other.position.y);
      if (other != this && d < perceptionRadius) {
        alignment.add(other.velocity);
        cohesion.add(other.position);
        let diff = p5.Vector.sub(this.position, other.position);
        diff.div(d * d);
        separation.add(diff);
        sameGroupCount[other.group]++;
        if (other.group === this.group) {
          groupCenter.add(other.position);
        }
        total++;
      }
    }

    if (total > 0) {
      alignment.div(total);
      alignment.setMag(this.maxSpeed);
      alignment.sub(this.velocity);
      alignment.limit(this.maxForce);

      cohesion.div(total);
      cohesion.sub(this.position);
      cohesion.setMag(this.maxSpeed);
      cohesion.sub(this.velocity);
      cohesion.limit(this.maxForce);

      separation.div(total);
      separation.setMag(this.maxSpeed);
      separation.sub(this.velocity);
      separation.limit(this.maxForce);

      this.applyForce(alignment);
      this.applyForce(cohesion.mult(1.5));
      this.applyForce(separation.mult(1.0));

      let maxGroup = this.group;
      let maxCount = 0;
      for (let i = 0; i < numGroups; i++) {
        if (sameGroupCount[i] > maxCount) {
          maxCount = sameGroupCount[i];
          maxGroup = i;
        }
      }

      this.group = maxGroup;

      let targetColor = groupColors[this.group];
      if (this.isPlayer) {
        if (maxCount > 0) {
          groupCenter.div(maxCount);
          let dir = p5.Vector.sub(groupCenter, this.position);
          dir.setMag(this.maxForce * 1.8);
          this.applyForce(dir);
        }
        this.currentColor = lerpColor(this.currentColor, lerpColor(this.baseColor, targetColor, 0.5), 0.015);
      } else {
        this.currentColor = lerpColor(this.currentColor, targetColor, 0.07);
      }
    }
  }

  display() {
    noStroke();
    fill(this.currentColor);
    ellipse(this.position.x, this.position.y, this.r);
  }
}

let boids = [];
let player;
let flowfield;
let mode = 'niñez';
let flowfieldScale = 0.05;
let cols, rows;
let zoff = 0;
let switchButton;

let groupColors = [];
let numGroups = 4;
let boidCount = 300;

function setup() {
  createCanvas(800, 800);
  colorMode(RGB);
  background(100);

  cols = floor(width * flowfieldScale);
  rows = floor(height * flowfieldScale);
  flowfield = new Array(cols * rows);

  groupColors = [
    color(255, 0, 0),
    color(0, 255, 0),
    color(0, 0, 255),
    color(255, 0, 255)
  ];

  for (let i = 0; i < boidCount; i++) {
    let b = new Boid(random(width), random(height), false);
    b.r = 6;
    boids.push(b);
  }

  player = new Boid(width / 2, height / 2, true);
  player.r = 10;
  boids.push(player);

  switchButton = createButton('Cambiar a adultez');
  switchButton.position(10, 10);
  switchButton.mousePressed(toggleMode);
}

function toggleMode() {
  mode = (mode === 'niñez') ? 'adultez' : 'niñez';
  switchButton.html(`Cambiar a ${mode === 'niñez' ? 'adultez' : 'niñez'}`);

  for (let b of boids) {
    if (!b.isPlayer) {
      b.setBaseColor();
    }
  }
}

function draw() {
  background(100);

  if (mode === 'niñez') {
    updateFlowField();
  }

  maintainGroupBalance();

  for (let boid of boids) {
    if (mode === 'niñez') {
      boid.followFlowField(flowfield, cols);
    } else {
      boid.flock(boids);
    }

    boid.update();
    boid.edges();
    boid.display();
  }

  handlePlayerInput();
}

function updateFlowField() {
  let xoff = 0;
  for (let x = 0; x < cols; x++) {
    let yoff = 0;
    for (let y = 0; y < rows; y++) {
      let index = x + y * cols;
      let angle = noise(xoff, yoff, zoff) * TWO_PI * 1.5;
      let v = p5.Vector.fromAngle(angle);
      v.setMag(0.4);
      flowfield[index] = v;
      yoff += 0.1;
    }
    xoff += 0.1;
  }
  zoff += 0.003;
}

function handlePlayerInput() {
  let force = createVector(0, 0);
  if (keyIsDown(LEFT_ARROW)) force.x = -0.5;
  if (keyIsDown(RIGHT_ARROW)) force.x = 0.5;
  if (keyIsDown(UP_ARROW)) force.y = -0.5;
  if (keyIsDown(DOWN_ARROW)) force.y = 0.5;
  player.applyForce(force);
}

function maintainGroupBalance() {
  let groupCounts = Array(numGroups).fill(0);
  for (let b of boids) {
    groupCounts[b.group]++;
  }

  for (let i = 0; i < numGroups; i++) {
    if (groupCounts[i] < 5) {
      let candidates = boids.filter(b => !b.isPlayer);
      for (let j = 0; j < 5 - groupCounts[i]; j++) {
        let b = random(candidates);
        if (b) {
          b.group = i;
          b.setBaseColor();
        }
      }
    }
  }
}
```


  
