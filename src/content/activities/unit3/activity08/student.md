#### Diferencia entre valor y referencia

- Aquí tenemos un caso claro de la diferencia entre estos conceptos y cómo puede ser uno mucho mejor que otro para los fines que tenemos en mente, en este caso, vamos a hablar individualmente de cada línea, empecemos por la:

```js
let friction = this.velocity.copy();
```

En este caso tenemos un objeto de **paso por valor**, aquí tenemos una ventaja grande y es que la función copy() crea un nuevo objeto con los mismos valores de this.velocity, pero en otro lugar de la memoria, de esta manera podemos 
modificar friction sin afectar el valor original de velocity, dándonos la libertad de poder usar esta función más adelante con su valor original sin tener ningún tipo de problema, algo que no podemos hacer en:

```js
let friction = this.velocity;
```

En donde tenemos un claro ejemplo de **paso por referencia**, en este caso friction en lugar de ser una copia, es una referencia de velocity, esto hace que cualquier cambio que hagamos en esta función afectará directamente a la 
función original de velocity, dándonos como resultado que se modifique su valor original, y al querer por ejemplo modificar friction solamente, no podremos dejarlo ahí ya que esto afectará a su vez a velocity, no tendremos
libertad de utilizar fricción en el objeto ya que si ponemos que la friction se multiplica por -1 por ejemplo, la velocidad también se multiplicará por -1, rompiendo totalmente la naturalidad del objeto en movimiento, en lugar
de simular un fenómeno natural como lo es la fricción
