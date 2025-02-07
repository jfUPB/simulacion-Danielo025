#### Concepto de Ruido Perlin

![Fotos de Ruido](https://github.com/user-attachments/assets/e3d1518f-3b5d-46ee-b2bf-8243d30a0619)

La izquierda es algo así como un camino aleatorio suavizado, es claro que esta usa Ruido Perlin, y se parece a una señal natural como una montaña, unas olas o una onda suave, mientras que la figura de la derecha 
es totalmente caótica y aleatoria, sin siquiera continuidad, cada punto es completamente aleatorio e independiente.

Ahora, hagamos una variación en mi código de la actividad #4 para que se note un ruido perlin en este, Ahora el movimiento del walker es más fluido y natural, 
como si siguiera una corriente en lugar de moverse de forma impredecible, y esto se debe a que su dirección no cambia abruptamente 
sino que sigue una secuencia de valores que evolucionan de manera continua, evitando los saltos bruscos del movimiento aleatorio.

![Ruido Perlin Mio](https://github.com/user-attachments/assets/1e0314fa-3399-43b0-89df-b754214a5834)



```js
let walker;

function setup() {
  createCanvas(640, 640);
  walker = new Walker();
  background(93, 155, 155); // Azul
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
    this.tx = 0; // Tiempo para Perlin Noise en X
    this.ty = 10000; // Tiempo para Perlin Noise en Y (desfase para evitar valores iguales)
  }

  show() {
    stroke(0);
    strokeWeight(2);
    point(this.x, this.y);
  }

  step() {
    this.x = map(noise(this.tx), 0, 1, 0, width);
    this.y = map(noise(this.ty), 0, 1, 0, height);

    this.tx += 0.01; // Pequeño incremento para transición suave
    this.ty += 0.01;
  }
}
```
