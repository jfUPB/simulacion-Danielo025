#### Hablemos de la masa

- Aquí la cuestión está en que al utilizar la línea de código

```js
applyForce(force) {
    // Asumiendo que la masa es 10
    force.div(10);
    this.acceleration.add(force);
}
```

Lo que estamos haciendo es modificar directamente el vector de fuerza original, el cual pasamos como argumento, esto tiene mucho que ver con los conceptos de función por valor y por referencia, tal y como dice el enunciado,
en este caso lo ideal sería dividir la fuerza sin modificar el vector original, utiliando algo como esto:

```js
applyForce(force){
let f = p5.Vector.div(force, this.mass); // En este lugar se cambia 10 por this.mass
this.acceleration.add(force);
}
```

Esto lo que nos da es la libertad de cambiar la masa del objeto sin tener que modificar la función applyForce(), sino sólo la "this.mass".

Básicamente el problema aquí radica en que p5.Vector es un objeto pasado por referencia, y si dividimos la "force" entre 10, modificamos el vector original de fuerza, algo que no es bueno en caso tal de necesitar usar la 
fuerza en otros lugares (Lo cual es el caso), para evitar esto lo que debemos hacer es crear una copia o utilizar un nuevo vector, en este caso, lo que hicimos con la línea propuesta fue crear una nueva instancia para que
la fuerza fuera de la función no cambie, es decir, la fuerza original, siga siendo la fuerza original siempre que necesitemos usarla.
