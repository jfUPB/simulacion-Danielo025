#### Obra generativa con partículas!!

##### Bueno, ahora sí, vamos con la obra de arte generativa...

- Mi idea es que se implementen aparte de la herencia y el polimorfismo, la aleatoriedad, la aceleración, la gravedad (fuerzas) y las ondas oscilatiorias.

- Lo que tengo pensado como idea inicial es un espacio, en donde hayan varios attractores que aparezcan aleatoriamente en este espacio, la idea es organizar el código para que no se superpongan
  entre ellos, que tengan diferentes tamaños y ubicaciones a lo largo del canvas de 800x800, estos atractores tendrán obviamente una función de absorber las partículas, aquí implementamos el concepto
  de fuerza y claramente la aleatoriedad se implementa en casi que todo el código, pero bueno, sigamos...

  Otra interacción clave será con el micrófono, aquí lo que haremos será que al detectar un sonido, aparecerá aleatoriamente en algún lugar de la pantalla un emisor de partículas, claramente este emisor no se
  quedará para siempre, sólo estará ahí por 1 seg o 2 segs aproximadamente, y lo mismo con sus partículas, tendrán una vida útil limitada, la idea es que si se detecta un sonido aparezcan estos
  emisores, y entre más alto sea el sonido detectado, más rápido irán las partículas hacia los attractores, es decir, tendrán o ''más aceleración'' o en su defecto ''menos peso'', cuando estas lleguen a los attractores y
  los toquen, deben desaparecer inmediatamente, claramente la idea de esto es que los emisores también tengan cierta aleatoriedad, algunos serán de bolitas normales, otros de confetti, otros de triangulitos,
  obvio con variedad de colores y eso, también me gustaría que los attractores tengan un efecto oscilatorio, meramente para que el concepto no quede vacío

- Con todo esto considero que se está haciendo uso de todoo lo requerido, se están haciendo uso de todos los conceptos, de la gestión de vida de las partículas y de la memoria para que no haya una saturación,
  es interactiva con el micrófono y a continuación mostraré el boceto inicial, y el único cambio que llegué a hacer (esto lo estoy escribiendo después de terminado el código)

Boceto de concepto inicial:

![image](https://github.com/user-attachments/assets/d43c622d-32ab-4db8-9b45-cf099af22e61)

- En este caso se nota todo lo que quiero lograr, la creación de partículas, la interacción con los attractores, la aleatoriedad, fuerzas, oscilación de los attractores, etc

  Me encontré al estar haciendo el código con una propuesta que me resultó interesante, y era de hacer que el fondo cambiara de colores, pero pensé que en es caso entonces también le deberíamos sumar a los attractores un
  fill para que no se perdieran y a las partículas aleatoriedad en tamaños y colores para que siempre haya una vista diferente del código, por lo cual pasamos de esto:

  ![image](https://github.com/user-attachments/assets/4cc21d8b-5b46-4020-a6fe-f3e631c75875)

  A esto:

  ![image](https://github.com/user-attachments/assets/790f505b-12f5-4c02-ae01-d482424fe9fd)
  ![image](https://github.com/user-attachments/assets/81b177f3-1c7e-49ed-9518-e0565fa2427d)

  Ese sería el resultado final, realmente me siento bastante satisfecho, se pueden lograr imágenes muy interesantes con diferentes tonos y entre más duro suene más locas  serás las partículas

  Aquí el link de mi [Obra de Arte generativa con Partículas](https://editor.p5js.org/Danielo025/full/mrTv24auv)

  Y el código utilizado:

```js
  class Emitter {
  constructor(pos, volume) {
    this.pos = pos.copy();
    this.particles = [];
    this.lifespan = 10000; // ms
    this.birth = millis();

    let count = int(random(10, 20));
    for (let i = 0; i < count; i++) {
      let type = int(random(3));
      let p;
      if (type === 0) p = new BallParticle(this.pos, volume);
      else if (type === 1) p = new Confetti(this.pos, volume);
      else p = new TriangleParticle(this.pos, volume);
      this.particles.push(p);
    }
  }

  applyAttractors(attractors) {
    for (let p of this.particles) {
      for (let a of attractors) {
        let dir = p5.Vector.sub(a.pos, p.pos);
        let d = dir.mag();
        if (d < a.r) {
          p.dead = true;
        } else {
          dir.normalize();
          let force = dir.mult(0.2);
          p.applyForce(force);
        }
      }
    }
  }

  update() {
    for (let i = this.particles.length - 1; i >= 0; i--) {
      this.particles[i].update();
      if (this.particles[i].isDead()) this.particles.splice(i, 1);
    }
  }

  display() {
    for (let p of this.particles) {
      p.display();
    }
  }

  isDead() {
    return millis() - this.birth > this.lifespan && this.particles.length === 0;
  }
}

class Particle {
  constructor(pos, volume) {
    this.size = random(5, 20);
    this.col = color(random(100, 255), random(100, 255), random(100, 255));
    this.pos = pos.copy();
    this.vel = p5.Vector.random2D().mult(2);
    this.acc = createVector();
    this.lifespan = 555;
    this.volume = volume;
    this.dead = false;
  }

  applyForce(f) {
    let scaled = f.copy().mult(map(this.volume, 0.05, 0.5, 1.5, 5));
    this.acc.add(scaled);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifespan -= 5;
    if (this.lifespan <= 0) this.dead = true;
  }

  isDead() {
    return this.dead;
  }

  display() {
    // abstract
  }
}

class BallParticle extends Particle {
display() {
  noStroke();
  fill(this.col.levels[0], this.col.levels[1], this.col.levels[2], this.lifespan);
  ellipse(this.pos.x, this.pos.y, this.size);
  }
}

class Confetti extends Particle {
display() {
  push();
  translate(this.pos.x, this.pos.y);
  rotate(frameCount * 0.1);
  noStroke();
  fill(this.col.levels[0], this.col.levels[1], this.col.levels[2], this.lifespan);
  rectMode(CENTER);
  rect(0, 0, this.size, this.size * 1.2);
  pop();
  }
}

class TriangleParticle extends Particle {
display() {
  push();
  translate(this.pos.x, this.pos.y);
  rotate(frameCount * 0.05);
  noStroke();
  fill(this.col.levels[0], this.col.levels[1], this.col.levels[2], this.lifespan);
  beginShape();
  vertex(-this.size / 2, this.size / 2);
  vertex(this.size / 2, this.size / 2);
  vertex(0, -this.size / 1.5);
  endShape(CLOSE);
  pop();
  }
}


class Attractor {
  constructor(pos, r) {
    this.basePos = pos.copy();
    this.pos = pos.copy();
    this.r = r;
    this.oscOffset = random(1000);
  }

  display() {
    let osc = sin(frameCount * 0.05 + this.oscOffset) * 10;
    let newR = this.r + osc;
    fill(255,0,0);
    stroke(255, 100, 200);
    strokeWeight(2);
    ellipse(this.basePos.x, this.basePos.y, newR * 2);
  }
}

let attractors = [];
let emitters = [];
let mic;
let lastEmissionTime = 0;
let emissionCooldown = 10; // ms

function setup() {
  createCanvas(800, 800);
  mic = new p5.AudioIn();
  mic.start();

  // Crear attractors aleatorios que no se superpongan
  let intentos = 0;
  while (attractors.length < 5 && intentos < 1000) {
    let r = random(30, 60);
    let pos = createVector(random(r, width - r), random(r, height - r));
    let valido = true;
    for (let a of attractors) {
      if (p5.Vector.dist(pos, a.pos) < a.r + r + 20) {
        valido = false;
        break;
      }
    }
    if (valido) attractors.push(new Attractor(pos, r));
    intentos++;
  }
}

function draw() {
  let r = map(sin(frameCount * 0.1), -1, 1, 50, 200);
let g = map(sin(frameCount * 0.1 + PI/3), -1, 1, 50, 200);
let b = map(sin(frameCount * 0.1 + PI/2), -1, 1, 50, 200);
background(r, g, b);


  // Dibujar attractors
  for (let a of attractors) {
    a.display();
  }

  // Emitir partículas si se detecta sonido fuerte
  let vol = mic.getLevel();
  if (vol > 0.01 && millis() - lastEmissionTime > emissionCooldown) {
    emitters.push(new Emitter(createVector(random(width), random(height)), vol));
    lastEmissionTime = millis();
  }

  // Actualizar y mostrar todos los emisores y sus partículas
  for (let i = emitters.length - 1; i >= 0; i--) {
    emitters[i].applyAttractors(attractors);
    emitters[i].update();
    emitters[i].display();
    if (emitters[i].isDead()) emitters.splice(i, 1);
  }
}
```

