#### Función sinusoidal

Esta simulación nos permite explorar de forma dinámica cómo se comporta una función sinusoide modificando sus principales parámetros: amplitud, período, fase y velocidad angular simplemente tocando las teclas correspondientes a cada concepto.

[Función Sinusoidall](https://editor.p5js.org/Danielo025/full/9DmpDaFP-)

![Conceptos1](https://github.com/user-attachments/assets/633e625a-a0ec-4cd2-bb05-c552aed6c671)
![Conceptos2](https://github.com/user-attachments/assets/78d0470b-716d-4914-b4ff-02d02ec419a7)

Código final:

```js
let amplitude = 100;
let period = 120; // afecta la frecuencia y la velocidad angular
let phase = 0;
let speed = 1;

function setup() {
  createCanvas(800, 700);
  textAlign(LEFT, TOP);
  textSize(16);

}

function draw() {
  background(255);
  translate(width / 2, height / 2);

  let omega = TWO_PI / period; // velocidad angular
  let t = frameCount * speed;

  let x1 = amplitude * sin(omega * t);
  let x2 = amplitude * sin(omega * t + phase);

  // Onda 1
  stroke(0);
  fill(127);
  line(0, -30, x1, -30);
  circle(x1, -30, 32);

  // Onda 2 con fase
  stroke(50, 100, 200);
  fill(50, 100, 200, 180);
  line(0, 30, x2, 30);
  circle(x2, 30, 32);

  // Info en pantalla
  noStroke();
  fill(0);
  text(`Controles del teclado: (Mayus = Shift + "Letra")
- A / a → Aumentar / Disminuir amplitud
- P / p → Aumentar / Disminuir período
- F / f → Aumentar / Disminuir fase
- S / s → Aumentar / Disminuir velocidad del tiempo
Amplitud (A): ${amplitude}
Período (T): ${period}
Frecuencia (f): ${(1 / period).toFixed(3)}
Velocidad angular (ω): ${omega.toFixed(3)}
Fase (ϕ): ${(degrees(phase)).toFixed(1)}°
Velocidad de tiempo: ${speed}`, -width / 2 + 10, -height / 2 + 10);
}

function keyPressed() {
  if (key === 'A') amplitude += 10;
  if (key === 'a') amplitude = max(10, amplitude - 10);

  if (key === 'P') period += 10;
  if (key === 'p') period = max(10, period - 10);

  if (key === 'F') phase += PI / 16;
  if (key === 'f') phase -= PI / 16;

  if (key === 'S') speed += 0.1;
  if (key === 's') speed = max(0.1, speed - 0.1);
}
```
