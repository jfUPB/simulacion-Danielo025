#### Moviendo los vectores

En este caso lo único que tocaría hacer sería que la base de los vectores no empiece en un lugar específico sino que sea igual a la posición del mouse, por lo cual, en la creación de los vectores, en la posición X
puse el mouse en X, y en la posición Y puse el mouse en Y, de esa forma puedo mover todos los vectores sólo moviendo el mouse, con 'mouseX' y 'mouseY'.

Respecto al tamaño lo más fácil es agregar un factor multiplicador para este, de esta manera se puede aumentar o disminuir el tamaño dependiendo de este factor, este factor se modificaría con la rueda del mouse, subiéndola 
y bajándola, esto no es complicado pues es en esencia una función que ya está en js, la cual sería 'mouseWheel', esta modificaría el factor, y a su vez el factor se multiplicaría por la escala, todo esto se regula
para que el tamaño no exceda los límtes del canvas.


Siendo así las cosas, el código sería este:

```js
let t = 0;
let increasing = true;
let scaleFactor = 1; // Factor de escala inicial

function setup() {
    createCanvas(700, 700);
}

function draw() {
    background(200);

    let v0 = createVector(mouseX, mouseY); // Base de las flechas sigue el mouse
    let v1 = createVector(250, 0).mult(scaleFactor);  // Vector rojo escalado
    let v2 = createVector(0, 250).mult(scaleFactor);  // Vector azul escalado

    let vGreen = p5.Vector.sub(v2, v1); // Vector verde: punta de rojo a punta de azul
    let v3 = p5.Vector.lerp(v1, v2, t); // Vector morado interpolado

    // Color del vector morado
    let startColor = color(255, 0, 0);  // Rojo (color del vector rojo)
    let endColor = color(0, 0, 255);    // Azul (color del vector azul)
    let lerpedColor = lerpColor(startColor, endColor, t); //Transición de color

    // Dibujar vectores
    drawArrow(v0, v1, 'red');  // Rojo
    drawArrow(v0, v2, 'blue'); // Azul
    drawArrow(p5.Vector.add(v0, v1), vGreen, 'green'); // Verde
    drawArrow(v0, v3, lerpedColor); // Morado interpolado

    // Animación del vector morado
    if (increasing) {
        t += 0.02;
        if (t >= 1) increasing = false;
    } else {
        t -= 0.02;
        if (t <= 0) increasing = true;
    }
}

// Función para dibujar una flecha
function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}

// Cambiar la escala de los vectores con la rueda del mouse
function mouseWheel(event) {
    scaleFactor += event.delta * -0.001; // Ajustar sensibilidad de la escala
    scaleFactor = constrain(scaleFactor, 0.5, 2.5); // Limitar el tamaño
}
```
