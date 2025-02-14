#### Convirtiendo el walker a vectores

En este caso, para poder convertir uno de los walkers que había en mi unidad 1, lo que tocaría hacer sería lo siguiente:

Primero tendríamos que reemplazar las variables 'x' y 'y' por 'this.position', el cual es un vector creado con createVector(width / 2, height / 2), que almacena la posición del Walker en la pantalla.

Luego, en 'step()', en lugar de modificar 'x' y 'y' directamente, creamos un vector 'stepVector' con (1,0), (-1,0), (0,1) o (0,-1), según el valor aleatorio, esto es equivalente a this.x++, this.x--, this.y++ y this.y--,
y lo sumamos a 'this.position' usando 'this.position.add(stepVector)', quedando así el mismo walker pero modificado a un uso de vectores.

Ahora pondré el código actualizado con vectores:

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
    this.position = createVector(width / 2, height / 2);
  }

  show() {
    stroke(0);
    point(this.position.x, this.position.y);
  }

  step() {
    let stepVector = createVector(0, 0);
    const choice = floor(random(4));

    if (choice == 0) {
      stepVector.x = 1;
    } else if (choice == 1) {
      stepVector.x = -1;
    } else if (choice == 2) {
      stepVector.y = 1;
    } else {
      stepVector.y = -1;
    }

    this.position.add(stepVector);
  }
}
```

Y aquí pondré la referencia del walker para que se note el cambio directo 

```js
let walker;

function setup() {
  createCanvas(640, 640);      
  walker = new Walker();
  background(95,128,128);      
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {      
      this.y--;
    }
  }
}
```
