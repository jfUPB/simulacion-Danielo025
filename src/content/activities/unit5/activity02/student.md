#### Sistemas de partículas - Conceptos aplicados


- 1 - *Nombre: Array of Particles.* - Concepto: Vectores (unidad 2) - 
Aplicación: La idea es que se mueva este sistema de partículas a lo largo de la pantalla con las flechas de dirección.

    *Explicación:* Los vectores son herramientas fundamentales en programación visual para representar posiciones, velocidades y direcciones. En este sistema, los usamos para mover las partículas por la pantalla, pero ahora
    también se integran con los controles del teclado, usando vectores para desplazar todo el sistema de partículas según la flecha presionada. Esto muestra cómo los vectores no solo afectan partículas individuales,
    sino también el movimiento de todo un conjunto de forma precisa y controlada. Usé este concepto ya que es el de los primeros, y este sistema de partículas al ser también básico, me parece que se complementan bien.

    *Link:* [Array Particles con Vectores](https://editor.p5js.org/Danielo025/full/cqhOZ7X0s)

![image](https://github.com/user-attachments/assets/7955e0a1-3868-4c35-8118-b336f1e05d7c)
![image](https://github.com/user-attachments/assets/f2369d3a-2056-41a8-9de5-b7665c114a98)

  *Código:*
    
```js
  class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}

let particles = [];
let emitterPosition;

function setup() {
  createCanvas(640, 240);
  emitterPosition = createVector(width / 2, 20); // posición inicial del sistema
}

function draw() {
  background(255);
  particles.push(new Particle(emitterPosition.x, emitterPosition.y));

  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];
    p.run();
    if (p.isDead()) {
      particles.splice(i, 1);
    }
  }
}

function keyPressed() {
  let move = 10;
  if (keyCode === LEFT_ARROW) {
    emitterPosition.x -= move;
  } else if (keyCode === RIGHT_ARROW) {
    emitterPosition.x += move;
  } else if (keyCode === UP_ARROW) {
    emitterPosition.y -= move;
  } else if (keyCode === DOWN_ARROW) {
    emitterPosition.y += move;
  }
}
```

***

- 2 - *Nombre: System of Systems.* - Concepto: Atracción gravitacional (Unidad 3) - 
Aplicación: La idea es que a pesar de que voy dando click y poniendo emisores de partículas a lo largo del canvas, que estos emisores con el tiempo vayan hacia un atractor movible con el mouse.

    *Explicación:* Este sistema de partículas aplica el concepto de atracción gravitacional simulada, donde cada partícula de cada emisor es afectada por una fuerza que la atrae hacia la posición del mouse.
    Para lograrlo, se calcula un vector desde la partícula hasta el mouse, se normaliza y se le da una magnitud baja para que el efecto no sea abrupto, creando así un campo gravitacional suave.
    Esto transforma el comportamiento natural de caída en una trayectoria curva, haciendo que las partículas tiendan a moverse en dirección al cursor, como si este fuera una especie de planeta atrayéndolas.
    Decidí hacer esto para tener una interacción completa con el mouse, ya que lo utilizo para posicionar los sistemas de partículas, me pareció interseante que estas también interactúen con él de otra forma,
    en este caso, siendo atraídas hacia él.
    La gestión de memoria se maneja limpiamente: cada Emitter contiene su propio arreglo de partículas, y estas se eliminan cuando su vida (lifespan) se agota,
    recorriendo el arreglo en reversa para evitar errores al borrar elementos durante el bucle. Así, se evita la acumulación infinita de objetos en memoria.

    *Link:* [Emisores Múltiples con Atracción gravitacional](https://editor.p5js.org/Danielo025/full/em9b_F9JE)

![image](https://github.com/user-attachments/assets/95e96c42-a3b5-4a49-9997-efb0b5d5d270)
![image](https://github.com/user-attachments/assets/72767d79-add2-4e53-90c4-50239bb89bde)


  *Código:*
```js
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      this.particles[i].run();
      if (this.particles[i].isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
  }

  run() {
    let gravity = createVector(0, 0.05);
    let attraction = p5.Vector.sub(createVector(mouseX, mouseY), this.position);
    attraction.setMag(0.05); // fuerza suave de atracción

    this.applyForce(gravity);
    this.applyForce(attraction);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}

let emitters = [];

function setup() {
  createCanvas(640, 240);
  createP("Click para agregar sistemas de partículas.");
}

function draw() {
  background(255);
  for (let emitter of emitters) {
    emitter.run();
    emitter.addParticle();
  }
}

function mousePressed() {
  emitters.push(new Emitter(mouseX, mouseY));
}
```

***

- 3 - *Nombre: Particle System with Inheritance and Polymorphism.* - Concepto: Aleatoreidad (Unidad 1) - 
Aplicación: Siento que aquí hasta cierto punto ya se usa este concepto, pero la idea es añadir otra figura (triángulos) y colores a las partículas.

    *Explicación:* Este sistema demuestra cómo aplicar la aleatoriedad mediante herencia y polimorfismo. Heredando de la clase Particle, se crean subclases como Confetti (cuadrado rotado) y
    TriangleParticle (triángulo), y se sobrescribe el método show() en cada una para representarlas gráficamente de manera distinta.
    Además, se introduce aleatoriedad en la selección de tipos de partícula y en sus colores. Cada vez que se añade una partícula al sistema, se escoge aleatoriamente entre tres tipos.
    Esto genera un sistema visualmente más dinámico y demuestra cómo la programación orientada a objetos facilita la extensión del comportamiento sin modificar la lógica principal del sistema, la cual de por sí ya tenía
    cierta aleatoreidad implementada.
    También, las partículas se eliminan de memoria cuando su vida (lifespan) llega a cero, lo cual es importante para mantener el rendimiento, especialmente con sistemas que generan muchas instancias a lo largo del tiempo.

    *Link:* [Herencia y polimorfismo con aleatoreidad](https://editor.p5js.org/Danielo025/full/sdy6nZ7TM)

![image](https://github.com/user-attachments/assets/40315b04-a344-4cb0-aee0-93030c6877e7)

  *Código:*
```js
  class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-1, 0));
    this.lifespan = 255.0;
    this.color = color(random(255), random(255), random(255), this.lifespan);
  }

  run() {
    let gravity = createVector(0, 0.05);
    this.applyForce(gravity);
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(this.color);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}

class Confetti extends Particle {
  show() {
    let angle = map(this.position.x, 0, width, 0, TWO_PI * 2);
    rectMode(CENTER);
    fill(this.color);
    stroke(0, this.lifespan);
    strokeWeight(2);
    push();
    translate(this.position.x, this.position.y);
    rotate(angle);
    square(0, 0, 12);
    pop();
  }
}

class TriangleParticle extends Particle {
  show() {
    fill(this.color);
    stroke(0, this.lifespan);
    strokeWeight(2);
    push();
    translate(this.position.x, this.position.y);
    beginShape();
    vertex(-6, 6);
    vertex(6, 6);
    vertex(0, -6);
    endShape(CLOSE);
    pop();
  }
}

class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    let r = random(1);
    if (r < 0.33) {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
    } else if (r < 0.66) {
      this.particles.push(new Confetti(this.origin.x, this.origin.y));
    } else {
      this.particles.push(new TriangleParticle(this.origin.x, this.origin.y));
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      p.run();
      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

let emitter;

function setup() {
  createCanvas(640, 240);
  emitter = new Emitter(width / 2, 20);
}

function draw() {
  background(255);
  emitter.addParticle();
  emitter.run();
}
```

***

- 4- *Nombre: Particle System with Forces.* - Concepto: Movimiento oscilatorio (Unidad 4) - 
Aplicación: La idea es reemplazar la estela que dejan las partículas al caer, por unas ondas, es decir, que estas partículas en lugar de dejar esa especie de trail, generen ondas a medida que van cayendo, como si fueran pulsaciones.

    *Explicación:* La forma más sencilla y visual de representar el rastro oscilante es con un movimiento senoidal horizontal, o sea que,
    mientras las partículas caen verticalmente por la gravedad, su posición X oscila con el tiempo usando una función sin(), lo cual da una especie de simulación visual de trail de ondas. Me parecía ideal utilizar aquí
    este concepto ya que aparte de ser el escenario perfecto para que se note entre todos los sistemas de partículas propuestos, le da además un dinamismo diferente al sistema.

    *Link:* [Sistemas de partículas con fuerzas yyy oscilación](https://editor.p5js.org/Danielo025/full/KETYjaBlR)

![image](https://github.com/user-attachments/assets/196afcd8-be62-427f-a1e6-f29cb7974f00)

   *Código:*
```js
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  applyForce(force) {
    for (let particle of this.particles) {
      particle.applyForce(force);
    }
  }

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      const particle = this.particles[i];
      particle.run();
      if (particle.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

class Particle {
  constructor(x, y) {
    this.basePosition = createVector(x, y); // Referencia original
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = createVector(random(-1, 1), random(-2, 0));
    this.lifespan = 255.0;
    this.mass = 1;
    this.theta = random(TWO_PI); // fase inicial
    this.amplitude = random(10, 30); // amplitud de oscilación
    this.oscillationSpeed = random(0.05, 0.1); // velocidad de oscilación
  }

  run() {
    this.update();
    this.show();
  }

  applyForce(force) {
    let f = force.copy();
    f.div(this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.basePosition.add(this.velocity); // solo mover base Y
    this.theta += this.oscillationSpeed; // aumenta el ángulo
    this.position.x = this.basePosition.x + sin(this.theta) * this.amplitude;
    this.position.y = this.basePosition.y; // mantener en Y
    this.acceleration.mult(0);
    this.lifespan -= 2.0;
  }

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
}

let emitter;

function setup() {
  createCanvas(1280, 480);
  emitter = new Emitter(width / 2, 50);
}

function draw() {
  background(255, 30);

  let gravity = createVector(0, 0.1);
  emitter.applyForce(gravity);

  emitter.addParticle();
  emitter.run();
}
```

***

- 5 - *Nombre: Particle System with a Repeller.* - Concepto: Atracción gravitacional (Unidad 3) - 
Aplicación: Ya se usaron las 4 unidades así que repetiré este, la idea es que el repelente se convierta en un atractor, que este absorba las partículas y se las "trague".

    *Explicación:* Para convertir el repeller en un atractor, simplemente invertimos el signo del cálculo para que sea positivo, o sea que ahora las partículas son atraídas, no repelidas.
    Me parece que este era el lugar perfecto para hacer un atractor jajaja ya que de esta manera le estamos dando completamente la vuelta a la gracia de este sistema de partículas.

    *Link:* [Sistema de partículas con repeller convertido en attractor](https://editor.p5js.org/Danielo025/full/N3iaRVnLt)

![image](https://github.com/user-attachments/assets/d0dcad20-b1d4-4738-b56a-66f5ddb35360)

   *Código:*
```js
class Emitter {

  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
  }

  addParticle() {
    this.particles.push(new Particle(this.origin.x, this.origin.y));
  }

  applyForce(force) {
    //{!3} Applying a force as a p5.Vector
    for (let particle of this.particles) {
      particle.applyForce(force);
    }
  }
  
applyAttractor(attractor) {
  for (let particle of this.particles) {
    let force = attractor.attract(particle);
    particle.applyForce(force);
  }
}

  run() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      const particle = this.particles[i];
      particle.run();
      if (particle.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }
}

class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(random(-0.5, 0.5), random(-0.5, 0));
    this.acceleration = createVector(0, 0);
    this.lifespan = 255.0;
  }

  run() {
    this.update();
    this.show();
  }

  applyForce(f) {
    this.acceleration.add(f);
  }

  // Method to update position
  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.lifespan -= 2;
    this.acceleration.mult(0);
  }

  // Method to display
  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  // Is the particle still useful?
  isDead() {
    return this.lifespan < 0.0;
  }
}

class Attractor {
  constructor(x, y) {
    this.position = createVector(100, 100);
    this.power = 100; // Fuerza de atracción
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(200, 50, 50); // Color rojo para resaltar que "absorbe"
    circle(this.position.x, this.position.y, 40);
  }

  attract(particle) {
    let force = p5.Vector.sub(this.position, particle.position);
    let distance = force.mag();
    distance = constrain(distance, 5, 50);
    let strength = (this.power) / (distance * distance); // signo positivo ahora
    force.setMag(strength);
    return force;
  }
}

let emitter;

//{!1} One repeller
let attractor;

function setup() {
  createCanvas(640 , 640);
  emitter = new Emitter(width / 2, 60);
  attractor = new Attractor(width / 2, 250);
}
function draw() {
  background(255);
  emitter.addParticle();

  let gravity = createVector(0, 0.05);
  emitter.applyForce(gravity);

  emitter.applyAttractor(attractor); // ahora es atractor
  emitter.run();

  attractor.show();
}
```
