#### Consolidación de lo aprendido

- *Gestión de la creación y desaparición de partículas y administración de memoria*

  En los diferentes sistemas de partículas de la undad, se utilizaron principalmente arreglos para gestionar la creación, desapareción y la memoria de los sistemas, aunque hubo ciertas variaciones, ya que en uno que otro
  habían interacciones, (Como por ejemplo en systems of systems, en donde poníamos los emisores con un click), todo se basa en conceptos bases clave, y es que estas salen del emisor, se almacenan en un array, y tienen
  unos ciclos de vida predefinidos limitados, cuando estos ciclos de vida acaban y la partícula se considera muerta, se eliminan de los arrays y así se evita la saturación de la memoria y se mantiene el código optimizado.

- *Aplicación del marco de movimiento Motion 101 en los sistemas de partículas*

  Realmente el marco de movimiento se aplica bastante normal, tal y como se ha aplicado en códigos de unidades anteriores, pero en este caso directamente a las partículas y no a los emisores, esto quiere decir que
  en cada actualización la aceleración acumulada se suma a la velocidad, y esta a su vez a la posición, generando un movimiento fluido y realista y por último
  la aceleración se reinicia a cero para permitir que las nuevas fuerzas del siguiente frame sean aplicadas correctamente. Nada del otro mundo, todo muy normal, pero en este caso aplicado directamente en la clase
  de particle.js, siempre se aplica ahí en la parte de update

- *Aplicación de fuerzas externas en los sistemas de partículas*

  La aplicación de las fuerzas externas también está en la clase de particle.js, y de hecho, justo arribita de la aplicación del marco 101, en la parte de run() y no es difícil notarlo, si se toma como ejemplo el código de
  system of systems (O de múltiples emisores), se ve como justo en esa parte está presenta la fuerza de gravedad, por obvias razones estas partículas deben seguir un comportamiento lógico, es lo que uno se espera, y en estos
  casos lo que podemos ver es esto, unas cuantas líneas que hacen que la partícula se vea afectada entre otras cosas por una fuerza gravitacional. También debo destacar que por ejemplo en el código en el cual hay un
  repeller, para lograr esta repulsión se calcula una fuerza similar a la gravitacional pero invertida, y luego están mis códigos, en donde aparte de esta fuerza gravitacional presente constantemente, se encuentran
  fuerzas hacia attractores, hacia el mouse, etc, todo modelado con una fuerza de atracción pero enfocadas en diferentes objetos o lugares, como ya he dicho, nada del otro mundo.

- *Uso de herencia y polimorfismo en los sistemas de partículas*

  Esto creo que sólo está presente en el sistema de partículas de "Particle System with Inheritance and Polymorphism" que pues, precisamente está hecho para hablar de esto e introducir los conceptos (O al menos darles
  un repaso) y en mi código de arte generativo, en todo caso, a través de la herencia, se definió una clase base Particle que contiene propiedades como la posición, velocidad, aceleración y métodos,
  y de esta clase se derivan otras subclases como confetti por ejemplo, es entonces cuando el polimorfismo se evidencia al tratar objetos de distintas subclases como si fueran de la clase base pero redefiniendo métodos
  según las distintas necesidades, como hice en mi caso que aparte de confetti también hice otra subclase de triangulitos






