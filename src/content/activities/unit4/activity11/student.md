#### Obra de arte generativa algorítmica interactiva

- *Idea inicial:*
  
  Mi idea sería básicamente una pieza de arte en la que se interactúe con el micrófono del dispositivo y sólo se reconozcan los bajos, para que así aunque el micrófono esté abierto, no capture todos los sonidos, sólo los graves.

  Tengo pensado que al detectar algún bajo, lo que ocurra sea que desde los bordes de la pantalla aparezcan unas ondas de sonido en una ubicación y con un color aleatorio, la idea es que estas vayan hacia un atractor el cual
  se pueda arrastrar en la pantalla para focalizar estas ondas en determinado lugar o dejar que simplemente vayan hacia el centro, si estas tocan el atractor se desvanecerán, pero si chocan entre ellas o con los bordes, rebotarán.

- *Boceto ideal:*
  
![Boceto1](https://github.com/user-attachments/assets/a1456ce6-9879-4d86-8587-3c01268b7d89) 


- *Proceso creativo:*
   
  Por ahora sólo he logrado que sean bolitas de colores que aparecen en los bordes y que al tocar el atractor el cual ya se mueve, desaparecen, pero aparte de que lo que deseo son ondas y no bolitas, la interacción entre ellas
  aún es un poco extraña y poco fluida, por lo cual toca corregir algunas cosas o cambiar la forma en que estas interactúan para que tengan un movimiento más visualmente atractivo

![Proceso1](https://github.com/user-attachments/assets/66c3c38b-490e-4a71-b68c-75ee0527748e)

 Ahora el código si está corregido, se cambiaron las partículas de bolitas por ondas reales que se desvanecen al llegar al attractor, si mueve este, estas cambian su destino tal y como se puede notar en la foto, él inicialmente estaba en la mitad, pero claramente se nota como al moverse abajo a la derecha las ondas van hacia allá, no rebotan entre ellas, sin embargo me parece que se ve bastante bien, en este caso utilicé el concepto de ondas, aleatoriedad y fuerza gravitacional parar lograr resultados como este:

 ![Proceso2](https://github.com/user-attachments/assets/e8ed5237-4da3-4f27-96eb-f9e1f0c6af39)

 Me parece que este resultado satisfeca lo que quería, es cierto que bajé un poco las expectativas, sin embargo, me parecen mejor de lo que tenía pensado, realmente se ve bastante bien cuando se logran interpolar varias ondas

 - [Aplicacion algorítmica de Ondas](https://editor.p5js.org/Danielo025/full/X70hyP2KD) (Recuerda dar acceso al micrófono, ya que detecta sonidos, es posible que tengas que recargar la página más de una vez si el micrófono está siendo utilizado por otro programa o pestaña del navegador)
   
El código final:

```js
let mic, fft;
let ondas = [];
let attractor;

function setup() {
  createCanvas(800, 800);
  mic = new p5.AudioIn();
  mic.start();
  fft = new p5.FFT();
  fft.setInput(mic);
  attractor = createVector(width / 2, height / 2);
  angleMode(DEGREES);
  noStroke();
}

function draw() {
  background(0, 10);

  // Visual attractor
  fill(255, 100, 100);
  ellipse(attractor.x, attractor.y, 30);

  // Obtener energía del mic
  let spectrum = fft.analyze();
  let bass = fft.getEnergy("bass");

  // Umbral para disparar una onda
  if (bass > 100) {
    let pos = generarPosDesdeBorde();
    ondas.push(new OndaRadial(pos.x, pos.y));
  }

  // Dibujar y actualizar ondas
  for (let i = ondas.length - 1; i >= 0; i--) {
    let o = ondas[i];
    o.update();
    o.atraer(attractor);
    o.display();

    if (o.alpha <= 0 || o.pos.dist(attractor) < 20) {
      ondas.splice(i, 1);
    }
  }
}

function mousePressed() {
  attractor.set(mouseX, mouseY);
}

function generarPosDesdeBorde() {
  let lado = floor(random(4));
  if (lado === 0) return createVector(random(width), 0); // arriba
  if (lado === 1) return createVector(width, random(height)); // derecha
  if (lado === 2) return createVector(random(width), height); // abajo
  return createVector(0, random(height)); // izquierda
}

class OndaRadial {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(2);
    this.radius = 0;
    this.alpha = 255;
    this.color = color(random(150,255), random(150,255), random(150,255), this.alpha);
  }

  update() {
    this.pos.add(this.vel);
    this.radius += 1.5;
    this.alpha -= 2;
    this.color.setAlpha(this.alpha);
  }

  atraer(target) {
    let fuerza = p5.Vector.sub(target, this.pos);
    fuerza.setMag(0.5);
    this.vel.add(fuerza);
    this.vel.limit(4);
  }

  display() {
    push();
    translate(this.pos.x, this.pos.y);
    stroke(this.color);
    noFill();
    strokeWeight(1.5);
    beginShape();
    for (let a = 0; a < 360; a += 10) {
      let r = this.radius + sin(a * 3 + frameCount * 2) * 5;
      let x = cos(a) * r;
      let y = sin(a) * r;
      vertex(x, y);
    }
    endShape(CLOSE);
    pop();
  }
}
```
 
