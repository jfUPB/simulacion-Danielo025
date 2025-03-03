#### Interpolación de vectores

En este caso para que el código propuesto termine dando como resultado la animación que se nos muestra, debemos primero que todo añadir un tercer vector, entre el rojo y el azul, esto no es complicado, pues básicamente
es un vector que sea la resta entre estos vectores, posteriormente tendríamos que hacer que se interpole el vector de la mitad, el morado, este vector lo que tendría que hacer, utilizando el método lerp():

Tenemos que interpolar los dos primeros vectores, el rojo y el azul, básicamente lerp() calcula un número que esté entre dos números en un momento específico, en este caso, lerp hace que el vector morado
sea la interpolación entre el vector rojo y el vector azul a lo largo del tiempo, dando como resultado un vector que va de uno al otro a lo largo del tiempo.

Ahora, hablemos del método lerpColor(), y es que si se logró entender el método lerp, lerpColor es exactamente lo mismo, pero con colores, en este caso, le dimos un color inicial (El rojo, que es el vector inicial,
para que coincida al llegar a él), y un color final (El azul, que es el vector final, para que coincida al llegar a él), y también se interpola a lo largo del tiempo, esto da como resultado que al ir de un vector al otro
el vector "morado" cambie de color entre un vector y otro, a lo largo del tiempo.


Esto se vería así, tal y como pide el enunciado que se vea:


![image](https://github.com/user-attachments/assets/941b69ff-e9e1-40b6-9dd0-1ddee0cbf1a6)
![image](https://github.com/user-attachments/assets/89a87d61-bcf0-4ef6-80ce-9a4817197f15)


Y el código quedaría de la siguiente manera, con unos pequeños ajustes para mejor visualización:

```js
let t = 0;
let increasing = true;

function setup() {
    createCanvas(500, 500);
}

function draw() {
    background(200);

    let v0 = createVector(10, 10); // Cambio de tamaño por visualización
    let v1 = createVector(400, 0);  // Vector rojo, cambio de tamaño por visualización
    let v2 = createVector(0, 400);  // Vector azul, cambio de tamaño por visualización

    let vGreen = p5.Vector.sub(v2, v1); // Vector verde: punta de rojo a punta de azul, se resta
    let v3 = p5.Vector.lerp(v1, v2, t); // Vector morado en movimiento, función lerp()

    // Color del vector morado
    let startColor = color(255, 0, 0);  // Rojo, el color del vector de arriba
    let endColor = color(0, 0, 255);    // Azul, el color del vector de abajo
    let lerpedColor = lerpColor(startColor, endColor, t); //Transición entre los colores a lo largo del tiempo

    // Dibujar vectores
    drawArrow(v0, v1, 'red');  // Rojo
    drawArrow(v0, v2, 'blue'); // Azul
    drawArrow(p5.Vector.add(v0, v1), vGreen, 'green'); // Verde, resta entre rojo y azul
    drawArrow(v0, v3, lerpedColor); // Morado en movimiento y cambiando de color

    // Animación del vector morado
    if (increasing) {
        t += 0.02;
        if (t >= 1) increasing = false;
    } else {
        t -= 0.02;
        if (t <= 0) increasing = true;
    }
}

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
```
