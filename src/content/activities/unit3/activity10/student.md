#### Modelando las fuerzas:

- **Fricción:**
  
  La fricción es una fuerza que se modela tomando una copia de la velocidad del objeto, normalizándola para obtener solo la dirección y multiplicándola
  por un coeficiente negativo, lo que genera una fuerza que se opone al movimiento; esta fuerza se aplica a la aceleración del objeto,
  sumándose a otras fuerzas (como la gravedad), y al final de cada frame se reinicia la aceleración para que las fuerzas se
  recalculen desde cero, logrando que el objeto desacelere de forma realista.

  El acceso a la aplicación interactiva estaría en el siguiente link: [Aplicación interactiva de Fricción](https://editor.p5js.org/Danielo025/full/XxgtGaqX7)


- **Resistencia al aire:**

  En esta simulación, la resistencia del aire se modela calculando una fuerza de arrastre (drag) que se opone al movimiento del objeto y es proporcional al cuadrado de su
  velocidad. Para lograrlo, se copia la velocidad del objeto, se invierte su dirección y se normaliza, y luego se multiplica por un coeficiente y por la magnitud de la velocidad
  al cuadrado. Esta fuerza de drag se aplica al objeto junto con otras fuerzas (como la gravedad y, opcionalmente, el viento al
  presionar el mouse), lo que provoca que el objeto desacelere de forma natural al moverse. Al final de cada frame, la aceleración
  se reinicia para que en el siguiente se vuelvan a calcular las fuerzas actuales, logrando así una simulación realista del efecto de la resistencia del aire.

  El acceso a la aplicación interactiva estaría en el siguiente link: [Aplicación interactiva de Resistencia al aire](https://editor.p5js.org/Danielo025/full/lHoA-nuC7)

- **Atracción gravitacional:**

  En esta simulación se modela la atracción gravitacional aplicando la ley de gravitación universal de Newton, que establece que la fuerza de atracción entre dos cuerpos es
  proporcional al producto de sus masas e inversamente proporcional al cuadrado de la distancia entre ellos. Se define un objeto "Attractor" con una masa
  considerable y una posición fija (que se puede reposicionar haciendo click), y se calcula el vector de fuerza desde el "mover" hacia el "attractor". Esta fuerza se normaliza,
  se ajusta con la constante gravitacional y se aplica al mover, modificando su aceleración, velocidad y posición en cada frame. De esta forma, se observa cómo el mover es atraído
  hacia el atractor, creando una experiencia interactiva donde se puede modificar la atracción al reposicionar el atractor.

  El acceso a la aplicación interactiva estaría en el siguiente link: [Aplicación interactiva de la Atracción gravitacional](https://editor.p5js.org/Danielo025/full/w8HC2q3vJ)

  
  Ahora, mostremos los códigos de manera explícita:


- Aquí tendríamos el código de la fricción:
  
```js
class Mover {
  constructor() {
    this.mass = 1;
    this.position = createVector(width / 2, 30);
    this.velocity = createVector(2, 0); // Se inicia con cierta velocidad horizontal
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  // Calcula y aplica la fricción, que se opone a la dirección del movimiento.
  applyFriction() {
    // Copia la velocidad para no modificar el vector original (paso por referencia vs copia)
    let friction = this.velocity.copy();
    friction.normalize();       // Nos quedamos solo con la dirección
    friction.mult(-0.02);       // Coeficiente de fricción: ajustar para mayor o menor efecto
    this.applyForce(friction);  // Se suma a la aceleración, junto con otras fuerzas aplicadas
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  display() {
    stroke(0);
    strokeWeight(2);
    fill(255,127, 127);
    ellipse(this.position.x, this.position.y, 48, 48);
  }

  checkEdges() {
    if (this.position.x > width) {
      this.position.x = width;
      this.velocity.x *= -1;
    } else if (this.position.x < 0) {
      this.velocity.x *= -1;
      this.position.x = 0;
    }
    if (this.position.y > height) {
      this.velocity.y *= -1;
      this.position.y = height;
    }
  }
}

let mover;

function setup() {
  createCanvas(640, 640);
  mover = new Mover();
  createP("Esta simulación modela fricción: la fuerza que se opone al movimiento, sumada a la gravedad.");
}

function draw() {
  background(92,155,155);

  // Aplicamos una gravedad constante
  let gravity = createVector(0, 0.1);
  mover.applyForce(gravity);

  if (mouseIsPressed) {
  let wind = createVector(0.1, 0); // Aumenté la fuerza del viento
  mover.applyForce(wind);
}

  
  // Aplicamos fricción: fuerza opuesta a la dirección del movimiento
  mover.applyFriction();

  mover.update();
  mover.display();
  mover.checkEdges();
}
```

- Aquí tendríamos el código de la resistencia al aire:
  
```js
class Mover {
  constructor() {
    this.mass = 1;
    this.position = createVector(width / 2, 30);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  // Aplica una fuerza dividiéndola por la masa
  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }
  
  // Modela la resistencia del aire (drag)
  // Se calcula como una fuerza opuesta al movimiento y proporcional al cuadrado de la velocidad.
  applyAirResistance(coefficient) {
    // Copiamos la velocidad y obtenemos su magnitud al cuadrado.
    let drag = this.velocity.copy();
    let speedSq = this.velocity.magSq();
    
    // Invertir la dirección para que la fuerza se oponga al movimiento
    drag.mult(-1);
    drag.normalize();
    // La fuerza de drag es proporcional al coeficiente y al cuadrado de la velocidad
    drag.mult(coefficient * speedSq);
    
    this.applyForce(drag);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    // Reinicia la aceleración para que en cada frame se recalculen las fuerzas aplicadas
    this.acceleration.mult(0);
  }

  display() {
    stroke(0);
    strokeWeight(2);
    fill(127,255, 127);
    ellipse(this.position.x, this.position.y, 48, 48);
  }

  checkEdges() {
    if (this.position.x > width) {
      this.position.x = width;
      this.velocity.x *= -1;
    } else if (this.position.x < 0) {
      this.velocity.x *= -1;
      this.position.x = 0;
    }
    if (this.position.y > height) {
      this.position.y = height;
      this.velocity.y *= -1;
    }
  }
}

let mover;

function setup() {
  createCanvas(640, 640);
  mover = new Mover();
  createP("Click mouse to apply wind force.");
}

function draw() {
  background(155,155,155);

  // Aplica gravedad
  let gravity = createVector(0, 0.1);
  mover.applyForce(gravity);

  // Aplica la resistencia del aire con un coeficiente (ajusta este valor para modificar el efecto)
  mover.applyAirResistance(0.01);

  // Si se presiona el mouse, aplica una fuerza de viento
  if (mouseIsPressed) {
    let wind = createVector(0.1, 0);
    mover.applyForce(wind);
  }

  mover.update();
  mover.display();
  mover.checkEdges();
}
```


- Aquí tendríamos el código de la atracción gravitacional:
  
```js
class Mover {
  constructor() {
    this.mass = 1;
    this.position = createVector(random(width), random(height));
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    // Se divide la fuerza por la masa para obtener la aceleración (F = ma)
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0); // Reinicia la aceleración para el siguiente frame
  }

  display() {
    stroke(0);
    strokeWeight(2);
    fill(255,127, 127);
    ellipse(this.position.x, this.position.y, 48, 48);
  }

  checkEdges() {
    if (this.position.x > width) {
      this.position.x = width;
      this.velocity.x *= -1;
    } else if (this.position.x < 0) {
      this.position.x = 0;
      this.velocity.x *= -1;
    }
    if (this.position.y > height) {
      this.position.y = height;
      this.velocity.y *= -1;
    } else if (this.position.y < 0) {
      this.position.y = 0;
      this.velocity.y *= -1;
    }
  }
}

class Attractor {
  constructor() {
    this.mass = 20;
    this.position = createVector(width / 2, height / 2);
  }

  attract(mover) {
    // Vector que va desde el mover hacia el atractor
    let force = p5.Vector.sub(this.position, mover.position);
    // Calcula la distancia y la limita para evitar fuerzas extremas\n
    let distance = force.mag();
    distance = constrain(distance, 5, 25);
    force.normalize();
    // La fuerza de atracción se calcula con la ley gravitacional: F = G * (m1 * m2) / (r^2)
    let strength = (G * this.mass * mover.mass) / (distance * distance);
    force.mult(strength);
    return force;
  }

  display() {
    fill(200, 11, 11);
    noStroke();
    ellipse(this.position.x, this.position.y, this.mass, this.mass);
  }
}

let mover;
let attractor;
let G = 1; // Constante gravitacional para ajustar la fuerza

function setup() {
  createCanvas(640, 640);
  mover = new Mover();
  attractor = new Attractor();
  createP("Haz click para reposicionar el atractor.");
}

function draw() {
  background(155);

  // Calcula la fuerza gravitacional ejercida por el atractor sobre el mover
  let force = attractor.attract(mover);
  mover.applyForce(force);

  mover.update();
  mover.display();
  mover.checkEdges();
  attractor.display();
}

// Permite reposicionar el atractor con un click
function mousePressed() {
  attractor.position.set(mouseX, mouseY);
}
```
