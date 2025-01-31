Pondré el código por acá, después lo explico

```js
let walker;

function setup() {
  createCanvas(640, 640);
  walker = new Walker();
  background(95, 128, 128);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
    this.prevX = this.x;
    this.prevY = this.y;
  }

  show() {
    stroke(0);
    line(this.prevX, this.prevY, this.x, this.y); // Dibuja una línea entre el punto anterior y el actual
  }

  step() {
    this.prevX = this.x;
    this.prevY = this.y;

    let stepSize = this.levy();
    let angle = random(TWO_PI);
    let dx = stepSize * cos(angle);
    let dy = stepSize * sin(angle);

    this.x += dx;
    this.y += dy;

    this.x = constrain(this.x, 0, width);
    this.y = constrain(this.y, 0, height);
  }

  levy() {
    let num;
    do {
      num = randomGaussian(0, 1);
    } while (abs(num) < 0.1); 

    return pow(abs(num), 2) * 7 * random([1, -1]); // Aumenta la distancia de los saltos grandes
  }
}
```
