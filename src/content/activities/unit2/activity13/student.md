#### Aplicaciones en el entretenimiento digital

Me parece que estos conceptos son fundamentales en el diseño de entretenimiento digital, y es que es un hecho que realmente los vectores son una base fundamental en el movimiento, teniendo en cuenta que como tal el vector de posición, de velocidad y aceleración 
pueden formar entre si una gama de movimientos complejos y naturales, tanto en animaciones como videojuegos y por ende llegando al campo de las experiencias interactivas de forma aún más compleja.

- Por ejemplo, se me ocurre que para empezar, en los videojuegos, en un juego de plataformas, un personaje se mueve con aceleración y velocidad, aquí ya se implementan los vectores, y de esta forma al caer el personaje, saltar, etc, este tiene una aceleración mucho más 
realista, simulando diferentes gravedades modificando algo tan simple como el valor del vector de la aceleración.

- Otro ejemplo en los videojuegos, sean en 3D o en 2D, es en los momentos de sigilo, en donde la función dist() podría ser esencial para verificar si el jugador está en el campo de visión de un enemigo, empezando así la alerta, en este caso, en juegos 3D más complejos,
se agregaría la función de dot(), para saber si el enemigo está viendo o no al jugador, alertándolo así de manera inmediata o 'ignorando' su presencia, como resultado el enemigo detecta al jugador solo si está dentro de su ángulo de visión, 
tal y como se puede ver por ejemplo en la serie de videojuegos de Far Cry:

- Ahora, en la animación esto sería muy útil para movimientos naturales, por ejemplo, en una cinemática de un juego, una soga se balancea de forma realista cuando un personaje la sujeta, esto no tiene por qué hacerse a mano, con el uso de 
dot() y mag() se podrían calcular ángulos y fuerzas para lograr un movimiento oscilante realista.

Ejemplo en far cry (Enemigos alertados):
![image](https://github.com/user-attachments/assets/fad65c58-a248-4e8c-b63a-96fb10264485)


