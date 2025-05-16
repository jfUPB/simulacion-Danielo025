#### Comportamiento de Enjambre

- *Objetivo y lógica de las reglas de Flocking*
  
  El comportamiento de enjambre o Flocking simula cómo se mueven animales como aves o peces en grupo. Se basa en tres reglas principales que cada boid sigue de forma autónoma.
  
  1) La separación evita que los boids choquen entre sí. Cada uno se aleja de los que estén demasiado cerca. Se calcula con una fuerza que empuja en dirección contraria a los vecinos cercanos.
  2) La alineación permite que un boid copie la dirección promedio de los que lo rodean. Esto hace que todos se muevan en la misma dirección y forma.
  3) La cohesión empuja a cada boid hacia el centro del grupo. Se calcula usando la posición promedio de los vecinos cercanos, ayudando a que se mantengan juntos.
  
  Estas reglas combinadas crean un movimiento grupal fluido y realista, sin necesidad de un líder.

  ----

- *Parámetros clave*
  
  Uno de los parámetros clave es el radio de percepción. En el código original, su valor es de aproximadamente 50 píxeles. Esto significa que cada boid solo tiene en cuenta a los demás que se
  encontraban dentro de ese radio. Este parámetro es crucial, ya que define con cuántos vecinos interactúa cada boid para aplicar las reglas de flocking.

  Los pesos de las reglas también juegan un papel fundamental. En el comportamiento original, los tres comportamientos (separación, alineación y cohesión) tienen un peso equilibrado.
  Esto permite que los boids se mantengan juntos sin aglomerarse demasiado, alineándose suavemente con los demás y manteniendo una distancia apropiada.

  El parámetro maxSpeed controla la velocidad máxima de cada boid. En su configuración original, esta velocidad es moderada, permitiendo un movimiento natural sin que los boids se dispersen demasiado
  rápido. A su vez, maxForce limita la fuerza máxima de cambio de dirección. Con un valor bajo, los giros y ajustes de trayectoria son suaves, evitando movimientos bruscos.

  ----

- *Cambios en el código y efecto observado*

  Se aumentó el tamaño del canvas a 800x800 píxeles y se cambió el fondo a un azul claro para que el enjambre se vea mejor.

  Los boids ahora persiguen el cursor del mouse como si fuera un objetivo. Esto hace que todo el grupo reaccione hacia donde el usuario mueve el puntero.

  La alineación y la cohesión se aumentaron considerablemente. La separación se redujo a 10, para que los boids se acerquen mucho entre sí, lo que favorece la formación de grupos apretados.
  Los pesos de las reglas se ajustaron así:
  - Separación: 0.3 (poco efecto)
  - Alineación: 1.0 (efecto medio)
  - Cohesión: 2.0 (muy influyente)
  
  Como resultado, el enjambre forma grupos compactos, muy unidos, que se mueven con fluidez. Parecen una sola entidad, que sigue el mouse en conjunto.

  [Ejambre Modificado](https://editor.p5js.org/Danielo025/full/VkvE4geYs)

  ![image](https://github.com/user-attachments/assets/fe239678-a837-458e-9eca-ab39bbebd0d7)
  ![image](https://github.com/user-attachments/assets/90b1d1ca-f93d-4476-b0a0-58e966e240b0)

  ----  

- El fragmento del código que contiene las modificaciones clave es el siguiente:

```js
createCanvas(800, 800); // Canvas más grande
background('#cceeff'); // Fondo azul claro

sep.mult(0.3);  // Separación baja
ali.mult(1.0);  // Alineación media
coh.mult(2.0);  // Cohesión alta

let desiredSeparation = 10;
let neighborDist = 120; // Alineación
let neighborDist = 150; // Cohesión

// Movimiento hacia el mouse
seekMouse() {
  let mouse = createVector(mouseX, mouseY);
  let steer = this.seek(mouse);
  steer.mult(0.5);
  this.applyForce(steer);
}

// Círculo en el cursor
ellipse(mouseX, mouseY, 20, 20);
```  

