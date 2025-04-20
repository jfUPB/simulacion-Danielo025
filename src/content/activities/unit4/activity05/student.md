#### Coordenadas polares

- Primera modificación: En este código se crea un vector que apunta en la dirección del ángulo theta, pero con una longitud predeterminada de 1,
ya que no se especifica el radio (r). Esto hace que el círculo que se dibuja con ese vector se mantenga muy cerca del centro, trazando una trayectoria muy
pequeña, casi imperceptible, peeero esta ni se llega a ver, ya que el código intenta dibujar una línea usando las variables "x" y "y", que no están definidas en esta versión, lo que directamente causa un error.

-Segunda modificación: Este código mejora la implementación anterior generando un vector con una dirección determinada por el ángulo theta y una magnitud igual al radio r, exactamente
como en las coordenadas polares. Esto permite que el círculo se mueva en una trayectoria circular con el mismo radio definido originalmente. Además, tanto la línea como el círculo se dibujan correctamente
usando las componentes "x" y "y" del vector, asegurando que el movimiento sea suave, preciso y visualmente correcto. Esta es la versión más limpia y adecuada del código original, usando vectores en lugar de
trigonometría manual.
