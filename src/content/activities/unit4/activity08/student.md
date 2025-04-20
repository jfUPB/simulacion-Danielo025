#### Moviendo la onda

Aquí lo ideal es trasladar ese código del setup() al draw(), y hacer que el ángulo inicial cambie con el tiempo, como si la onda estuviera “desplazándose” hacia la derecha o izquierda, esto dará una sensación de movimiento.

[Onda en movimiento](https://editor.p5js.org/Danielo025/full/RPOps5N7q)

![Mov1](https://github.com/user-attachments/assets/b7bb48f4-936b-4bb2-9fd7-bd59c595b87c)
![Mov2](https://github.com/user-attachments/assets/dc100d33-300d-43b8-b296-698ddd6d5a93)

```js
let startAngle = 0; // ángulo base que se va actualizando
let angleVelocity = 0.2;
let amplitude = 100;

function setup() {
  createCanvas(640, 320);
}

function draw() {
  background(255);

  let angle = startAngle; // ángulo inicial que se va incrementando para cada círculo

  stroke(0);
  strokeWeight(2);
  fill(127, 127);

  for (let x = 0; x <= width; x += 24) {
    let y = amplitude * sin(angle);
    circle(x, y + height / 2, 48);
    angle += angleVelocity;
  }

  startAngle += 0.07; // hace que la onda se desplace con el tiempo
}

```

