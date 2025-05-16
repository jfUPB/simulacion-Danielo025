### Conceptos Fundamentales

#### **1) Simulación para el manejo de ángulos**

  - ¿Qué está pasando en esta simulación? ¿Cuál es la interacción?

  Esta es una simulación de una barra girando sobre su centro, con un círculo en cada lado, en cada frame se actualiza el ángulo de rotación, aumentando este y haciendo que la barra gire constantemente, aparte de esto
  parece habre una interacció al presionar una tecla, aumentando aún más el ángulo y acelerando un poco más la rotación, si se aumenta arbitrariamente este valor se puede notar aún más esta interacción.

  - En cada frame se está trasladando el origen del sistema de coordenadas al centro de la pantalla. ¿Por qué crees que se hace esto?

  Como tal el origen del sistema de coordenadas sería: (0,0), es decir, la esquina superior izquierda, si se dejara la simulación ahí realmente sólo podríamos ver un cuarto de ella, la esquina inferior derecha, las otras
  partes estarían escondidas fuera del canvas, es por esto que con la función translate, este origen se traslada a la mitad del canvas, permitiéndonos así tener una mejor visualización, se usa la división de la pantalla
  y no las coordenadas de la mitad para que en caso tal de modificar el tamaño del canvas, siempre nos refiramos al centro del mismo y no un lugar específico.

  - ¿Cuál es la relación entre el sistema de coordenadas y la función rotate().

  Básicamente rotate() rota el sistema de coordenadas entero, lo cual significa que todo lo que se dibuja después de rotate() estará girado respecto al nuevo sistema de coordenadas. 
  Por eso es importante aplicar primero translate() (para mover el origen al punto deseado) y luego rotate() (para girar el sistema en torno a ese nuevo origen).

  - Al dibujar los elementos gráficos parece que se está dibujando en la posición (0, 0) del sistema de coordenadas. ¿Por qué crees que se hace esto? y ¿Por qué aunque en cada frame se hace lo mismo, los elementos gráficos rotan?

  El tema aquí es que aunque las coordenadas están fijas respecto al origen, el sistema de coordenadas completo ya fue transformado antes del draw, primero con translate, moviendo el punto de origen, y luego con
  rotate, rotando todo el sistema, haciendo así que siempre se dibuje la misma línea y los mismos círculos relativos al origen, y al rotar el sistema de coordenadas en cada frame lo que el usuario ve es una rotación no de 
  los objetos individuales como tal, sino del espacio.

#### **2) Simulación que apunta en dirección de movimiento**

  - Identifica el marco motion 101. ¿Qué es lo que se está haciendo en este marco?

  En este caso el marco motion 101 tiene tres pasos y un límite, primero se calcula la aceleración (en este caso, hacia el mouse), luego esa aceleración se suma a la velocidad, y finalmente la velocidad se suma a la posición del objeto. 
  De esta manera el objeto se mueve como si estuviera siendo jalado por el cursor y al final también se limita la velocidad para que no se mueva demasiado rápido. 

  - ¿Qué hace la función heading()?

  La función heading() devuelve el ángulo de dirección de un vector en radianes, en este caso, this.velocity.heading() nos da el ángulo hacia donde se está moviendo el objeto,
  lo cual se usa para que el rectángulo apunte visualmente hacia esa dirección.

  - ¿Qué hace la función push() y pop()? 

  Push() y pop() se usan para guardar y restaurar el sistema de coordenadas actual, esto significa que cualquier transformación como translate() o rotate() que se haga entre push() y pop() solo afecta a ese bloque de código.

  - Experimento: En este caso lo que haré será dibujar algo dentro y fuera del sistema de coordenadas, es decir, dentro del push y pop, para mostrar la diferencia

  En este caso se puede ver que dibujé un cuadrado fuera del pop, lo que hace que el cuadrado simplemente esté ahí, existiendo, con su propio sistema de coordenadas aparte, de hecho, la "navecita" que sigue el mouse lo atraviesa, ya que es un sistema aparte, de hecho se podría ver como una capa aparte.
![image](https://github.com/user-attachments/assets/c08cd4f4-f3ae-456b-adf3-41b4019550e1)

  Mientras que por otro lado, si  se dibuja dentro del sistema de coordenadas, también seguiría el mouse, junto a nuestra navecita, siguiendo la dirección y siguiendo al mouse
  ![image](https://github.com/user-attachments/assets/138dee5d-f3b3-45df-b01a-ed4f527cd655)


  - ¿Qué hace rectMode(CENTER)?

  Por defecto, rect(x, y, w, h) dibuja el rectángulo desde la esquina superior izquierda. Pero al usar rectMode(CENTER), el punto (x, y) se convierte en el centro del rectángulo.

  - Experimento: Aquí el experimento es básico, lo único que muestro es que si al modo del rectángulo le ponemos (CORNER), este se dibuja desde la esquina, mientras que si le ponemos (CENTER), se dibuja desde el centro, a pesar de que ambos estén en el mismo punto de referencia

  ![image](https://github.com/user-attachments/assets/520cb051-a0a3-46f9-a6a0-d88493ae4aca)

  - ¿Cuál es la relación entre el ángulo de rotación y el vector de velocidad?

  El ángulo que se calcula con heading() representa hacia dónde se dirige el objeto según su velocidad, al rotar el rectángulo por ese ángulo, hacemos que visualmente apunte en la misma dirección que se está moviendo.
  
  Digamos que el rectángulo es como una flecha, y su vector de velocidad es como una línea que sale de su centro y apunta hacia donde se mueve. El heading() de ese vector nos da el ángulo para girar la flechita, de modo que siempre mire en la dirección de su movimiento.

  

  

