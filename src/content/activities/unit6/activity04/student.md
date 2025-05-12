#### Aplicación de agentes autónomos 

###  *Comportamiento de la sociedad*

  En este caso lo que haré será integrar ambos conceptos, los campos de flujo y el comportamiento de enjambre, para mostrar una representación de cómo funciona la sociedad, de como en un 
  principio a las personas se nos guía por un camino específico, en conjunto, pero que a medida que crecemos, y tomamos nuestras propias decisiones a pesar de ser individuos con metas 
  específicas, nuestra necesidad de pertenecer a algo, y también por el hecho de ser seres sociales, nos vamos formando en grupos, siguiendo a aquellos cuyas ideas se conectan con las 
  nuestras, esas conexiones son más fuertes que cualquier otra influencia externa, pues al final del día nos alineamos con aquello que nos llena más, que nos hace más feliz, aquello para lo que "estamos hechos".

  Esta pequeña reflexión fue resultado directo de ver ambos comportamientos, y es que me parece que es una analogía interesante, el sistema está organizado de tal manera que hasta cierto punto
  de nuestra vida todos tenemos en general experiencias bastante similares, estudiamos, pasamos por cambios en nuestro cuerpo, llegan las críticas sociales y la expectativa de nuestros 
  familiares, y eventualmente tomamos la decisión, si queremos hacer algo bueno, algo malo, estudiar una cosa u otra, trabajar en un lugar u otro, y es entonces cuando todos esos campos de 
  flujo (la educación, la familia y la sociedad) se convierten en comportamientos de enjambres (las carreras universitarias, los puestos de trabajo, los equipos de deporte, etc).

  Así que lo que haré será una obra que integre eso de una forma visual, lo que se me ocurre es un comportamiento de campos de flujo en que seamos un boid, uno específico, tendremos la 
  posibilidad de controlarlo con las flechas, y aunque seremos atraídos por los campos de flujo y eventualmente por el comportamiento de enjambre, seremos nosotros quienes trataremos de encajar
  en uno o en otro, es decir, estando en la fase de campos de flujo, decidiremos a cuál entrar, y aunque en un principio nuestro color sea negro, al entrar en uno u otro adquiriremos el color 
  que estos tengan, adicionalmente habrá un interruptor en la parte superior, el cual al presionarlo, estos voids dejarán de seguir los campos de flujo (desaparecerán) y empezarán a 
  comportarse como enjambre, formándose nuevamente en distintos enjambres de colores (como el código original, pero con 4 colores limitados, cada uno representando realidades sociales 
  istintas), y nuevamente tendremos la oportunidad de encajar en uno o en otro, de movernos a nuestra merced entre ellos, pero si nos metemos en alguno y nos dejamos de mover, seremos parte
  de ese sistema, debido a su evidente atracción.

  ---

#### *Documentación de proceso*

- Algoritmo elegido
  
  Para esta pieza decidí combinar los dos algoritmos estudiados: Flow Fields y Flocking. Me interesa cómo ambos pueden representar aspectos distintos pero complementarios del comportamiento social: uno como
  estructura invisible que guía, y otro como la respuesta colectiva que emerge de los individuos.

- Concepto de aplicación inesperada

  Mi propuesta representa el comportamiento de la sociedad humana desde una perspectiva simbólica. En lugar de simular fluidos o criaturas, uso los algoritmos para explorar cómo las personas buscan pertenecer a
  grupos, influenciadas por contextos sociales invisibles. El flujo representa fuerzas externas como educación, familia o cultura. El flocking representa la forma en que nos alineamos con otros, en búsqueda de identidad y conexión.

- Adaptación de la lógica del algoritmo

  Los campos de flujo guían los movimientos de todos los agentes en la escena, simulando esa estructura social que muchas veces no vemos, pero que nos empuja a movernos en ciertas direcciones. Al mismo
  tiempo, los agentes siguen un comportamiento de enjambre (flocking), buscando alinearse con otros cercanos. Cuando un agente entra en una zona de color, su comportamiento cambia: adquiere un nuevo
  color (representando una identidad) y se alinea con agentes del mismo color, reforzando esa identidad grupal.

- Interacción implementada

  El usuario controla un agente individual con las flechas del teclado. Este agente puede desplazarse libremente por los campos de flujo, pero al entrar en las "zonas de identidad" , adopta un nuevo comportamiento e
  identidad visual. A partir de ahí, se comporta como parte del grupo con el que se alineó. La pieza permite explorar activamente cómo tomamos decisiones y cómo estas decisiones nos convierten en parte de una estructura mayor.




  
