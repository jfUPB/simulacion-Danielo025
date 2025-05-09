#### El problema de los n-cuerpos

En este caso lo que hice fue modificar el sistema de cuerpos para que no se atraigan entre sí todo el tiempo, sino que floten libremente hasta que le de click en la pantalla, siendo así, cuando hago click, 
los cuerpos son atraídos hacia ese lugar con una fuerza gravitacional más fuerte, lo que hace que se intenten agrupar, una vez que llegan, cuando logran hacerlo, la fuerza vuelve a la normalidad y los cuerpos 
se dispersan de nuevo, también puse colisiones con los bordes de la pantalla para que reboten en lugar de atravesarlos e irse al inifinito, limité la velocidad y poco más.
A pesar de estos cambios, sigue siendo un problema de los n-cuerpos porque cada objeto sigue teniendo masa, posición, velocidad y aceleración, y las fuerzas 
siguen afectando su movimiento. En vez de que los cuerpos interactúen gravitacionalmente entre ellos, ahora reaccionan a la interacción con el
punto donde hago clic, pero la esencia del problema sigue siendo la simulación de cuerpos en movimiento bajo fuerzas externas, con más o menos control en este caso.

[Problema de los n-cuerpos modificado](https://editor.p5js.org/Danielo025/full/Iq6H9nzmf)

![Aleatorio](https://github.com/user-attachments/assets/cf93264c-a8e1-4bc4-b8dc-03cceee28225)
![Click1](https://github.com/user-attachments/assets/7e941438-f7ed-4292-868b-f5ccc2eb4722)
![Click2](https://github.com/user-attachments/assets/e7b42b05-b7af-4712-9e21-82813f09dfd1)



```js
class Body {
  constructor(x, y, m) {
    this.mass = m;
    this.position = createVector(x, y);
    this.velocity = createVector(random(-1, 1), random(-1, 1)); // Velocidad inicial aleatoria
    this.acceleration = createVector(0, 0);
    this.target = null; // Punto objetivo
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    if (this.target) {
      let force = p5.Vector.sub(this.target, this.position);
      let distance = force.mag();
      
      if (distance < 5) {
        this.target = null; // Liberar cuando llegan
      } else {
        force.setMag(G * this.mass);
        this.applyForce(force);
      }
    }

    this.velocity.add(this.acceleration);
    this.velocity.limit(2);
    this.position.add(this.velocity);
    this.acceleration.mult(0);

    this.checkEdges(); // Verificar colisión con bordes
  }

  checkEdges() {
    if (this.position.x <= 0 || this.position.x >= width) {
      this.velocity.x *= -1;
      this.position.x = constrain(this.position.x, 0, width);
    }
    if (this.position.y <= 0 || this.position.y >= height) {
      this.velocity.y *= -1;
      this.position.y = constrain(this.position.y, 0, height);
    }
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(255,127, 127);
    circle(this.position.x, this.position.y, this.mass * 25);
  }
}

let bodies = [];
let G = 0.1;

function setup() {
  createCanvas(640, 640);
  for (let i = 0; i < 10; i++) {
    bodies[i] = new Body(random(width), random(height), random(0.5, 2));
  }
}

function draw() {
  background(155,155,255);

  for (let body of bodies) {
    body.update();
    body.show();
  }
}

function mousePressed() {
  for (let body of bodies) {
    body.target = createVector(mouseX, mouseY);
  }
}
```
