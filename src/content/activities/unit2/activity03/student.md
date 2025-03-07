#### Experimentando con el código

En este caso se me piden varias cosas, por lo cual primero daré las explicaciones pertinentes y luego pondré el código con comentarios que hagan referencia a los cambios hechos o a las líneas relevantes del código:

- Primeramente usaré el console.log() y el print() para mostrar mi nombre, el vector que se crea y posteriormente el vector modificado, todo esto se debería mostrar en la consola del p5.js en la versión web.

- Efectivamente al momento de realizar el cambio en el código implementando estas funciones, sí se realizó esto que esperaba, fue un cambio simple, pero saber que funciona nunca sobra.

- Ahora, ¿qué tipo de paso se está realizando en este código?, en este caos tenemos un paso por referencia, en este código 'posicion' es un objeto 'p5.Vector', y los objetos
en JavaScript se pasan por referencia. Cuando se llama a 'playingVector(posicion)', la función recibe una referencia al mismo objeto, no una copia, y es por eso que
cuando modificamos 'v.x' y 'v.y' dentro de 'playingVector(v)', esos cambios afectan directamente a posicion en 'setup()'.

- Realmente no 'recordé' los conceptos de paso por valor y paso por referencia, los aprendí ya que no eran algo de lo que tuviera conocimiento, así que empezando por ahí ya es un aprendizaje, también entendí que la prinicpal
diferencia entre las funciones console.log() y print() es que la console es para un debugging un poco más serio, mientras que print la usé para algo muy simple como simplemente escribir mi nombre, también aprendí un poco
más sobre como funciona los objetos en javascript y es que estos son más complejos que un número primitivo cualquier, lo cual es realmente interesante.

- Ahora sí, el código final modificado con lo que me esperaba encontrar en él y la explicación en comentario del paso por referencia 

```js
let position;

function setup() {
    createCanvas(400, 400);
    
    position = createVector(6, 9);
    console.log("Vector original:", position.toString()); // Muestra el vector antes de modificarlo
    
    playingVector(position);
    
    console.log("Vector después de modificar:", position.toString()); // Muestra el vector después de modificarlo
    
    print("Daniel Muñoz"); // Escribo mi nombre en la pestaña de mensajes de p5.js
    
    noLoop();
}

function playingVector(v) {
    // PASO POR REFERENCIA: Se modifica el mismo objeto, no una copia.
    v.x = 20;
    v.y = 30;
}

function draw() {
    background(220);
    console.log("Only once");
}
```
