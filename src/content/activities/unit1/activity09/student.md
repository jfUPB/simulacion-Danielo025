#### Aplicación interactiva 

Hice un cambio en el concepto, ahora el ruido perlin no irá en dirección de las teclas 'WASD' sino que al presionar la tecla 'g', este se moverá libremente durante un par de segundos, el vuelo de levy sigue siendo igual, al presionar la 'f' se mueve aleatoriamente a una parte de la pantalla.

De momento este es mi código, está en desarrollo, hasta ahora tiene el random walk y la tendencia hacia donde indiquen las 'WASD', ya logré implementar el vuelo de lévy, falta implementar el ruido perlink


```js
let x, y; // Posición de la bola
let prevX, prevY; // Posición anterior para la trayectoria
let stepSize = 5; // Tamaño del paso en el random walk
let levyStepSize; // Tamaño del paso en el vuelo de Lévy
let isLevy = false; // Indicador de si está en modo vuelo de Lévy
let levySteps = 0; // Contador de los pasos del vuelo de Lévy
let maxLevySteps = 4; // Número máximo de pasos para el vuelo de Lévy

function setup() {
  createCanvas(640, 640);
  // Inicializamos las posiciones de la bola
  x = width / 2;
  y = height / 2;
  prevX = x;
  prevY = y;
  background(93, 155, 155); // Fondo azul cian
}

function draw() {
  // Dibujamos la trayectoria de la bola
  stroke(0); // Trazo negro para la trayectoria
  line(prevX, prevY, x, y); // Dibuja línea entre la posición anterior y la nueva

  // Dibujamos la bola en rojo oscuro
  fill(139, 0, 0); // Rojo oscuro
  noStroke();
  ellipse(x, y, 10, 10); // La bola tiene un tamaño de 10px

  // Guardamos la posición anterior
  prevX = x;
  prevY = y;

  if (isLevy) {
    levyMove(); // Si está en modo Lévy, mueve con Lévy
  } else {
    // Movimiento de la bola según la tecla presionada
    if (keyIsDown(65)) { // Tecla 'A' para izquierda
      x -= stepSize;
    } else if (keyIsDown(87)) { // Tecla 'W' para arriba
      y -= stepSize;
    } else if (keyIsDown(68)) { // Tecla 'D' para derecha
      x += stepSize;
    } else if (keyIsDown(83)) { // Tecla 'S' para abajo
      y += stepSize;
    } else {
      // Movimiento random walk cuando no se presiona ninguna tecla
      let angle = random(TWO_PI);
      let dx = cos(angle) * stepSize;
      let dy = sin(angle) * stepSize;
      x += dx;
      y += dy;
    }
  }

  // Aseguramos que la bola no salga del canvas
  x = constrain(x, 0, width);
  y = constrain(y, 0, height);
}

// Activar el vuelo de Lévy al presionar la tecla 'F'
function keyPressed() {
  if (key === 'f' || key === 'F') {
    if (!isLevy) {
      isLevy = true; // Activa el vuelo de Lévy
      levyStepSize = levy(); // Establece el tamaño de paso para el vuelo de Lévy
      levySteps = 0; // Reinicia el contador de pasos de Lévy
    }
  }
}

// Función de vuelo de Lévy
function levyMove() {
  if (levySteps < maxLevySteps) {
    let angle = random(TWO_PI);
    let dx = cos(angle) * levyStepSize;
    let dy = sin(angle) * levyStepSize;

    x += dx;
    y += dy;

    // Después de un salto, generamos un nuevo tamaño de paso para el próximo salto
    levyStepSize = levy(); // Calculamos un nuevo tamaño de paso

    // Aseguramos que la bola no salga del canvas
    x = constrain(x, 0, width);
    y = constrain(y, 0, height);

    levySteps++; // Incrementamos el contador de pasos de Lévy
  } else {
    isLevy = false; // Detenemos el vuelo de Lévy después de 4 pasos
  }
}

// Función para calcular un tamaño de paso de Lévy
function levy() {
  let num;
  do {
    num = randomGaussian(0, 1); // Generamos un valor normal (gaussiano)
  } while (abs(num) < 0.1); // Nos aseguramos de que el valor no sea demasiado pequeño

  return pow(abs(num), 2) * 20 * random([1, -1]); // Retornamos un valor de paso de Lévy
}
```
