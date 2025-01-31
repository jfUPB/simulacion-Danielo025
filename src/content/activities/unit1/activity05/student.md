Estaba experimentando, pondré el código mientras, después lo explico jeje



```javascript
function setup() {
  createCanvas(100, 100);

  background(93,125,125);

  describe('Robé el ejemplo de la actividad #4 y le estuve haciendo modificacionse hasta que funcionó como lo necesitaba.');
}

function draw() {
  // Style the circles.
  noStroke();
  fill(0, 10);

  y = randomGaussian(70,1);
  x = 10;
  circle(x, y, 5);
  
  y = randomGaussian(60,5);
  x = 20;
  circle(x, y, 5);
  
  y = randomGaussian(50,5);
  x = 30;
  circle(x, y, 5);

  y = randomGaussian(40, 5);
  x = 40;
  circle(x, y, 5);
  
  y = randomGaussian(30, 5);
  x = 50;
  circle(x, y, 5);
  
  y = randomGaussian(40, 5);
  x = 60;
  circle(x, y, 5);
  
  y = randomGaussian(50, 5);
  x = 70;
  circle(x, y, 5);
  
  y = randomGaussian(60, 5);
  x = 80;
  circle(x, y, 5);
  
  y = randomGaussian(70, 1);
  x = 90;
  circle(x, y, 5);
}
```
