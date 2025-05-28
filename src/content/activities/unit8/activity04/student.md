#### Visual Final

[Visual Final - TNE Remix](https://youtu.be/FcsGaWsJs_w)

![TestEvee](https://github.com/user-attachments/assets/c2be177a-9524-4020-bf28-4126e92f5b0a)

----

- *Aclaraciones:* Primero que todo me gustaría hacer un par de aclaraciones, el ejercicio final fue hecho en Blender, utilizando los Geometry nodes, formas básicas y materiales PBR, esto combinado con interacción con
  la música y una serie de parámetros configurables dio como resultado el final que está enlazado al principio de este mensaje, lo quise hacer en Blender ya que es el software con el que más familiarizado estoy,
  y quería aprovechar la oportunidad con este proyecto, de aprender un poco más acerca de cómo funciona la interacción con la música en Blender, aprender sobre funcionalidades de nodos, combinaciones, etc.

  Dicho esto, continuemos con la explicación de cómo fue que se llevó a cabo este visualizer:

- En mi caso lo que hice fue dividir los tonos de la canción en 3, los bajos, en frecuencias de 0 a 300, los medios, de 300 a 4000 y los altos, de 4000 en adelante, estas frecuencias las vinculé con 3 cubos los cuales,
  al fondo del visualizer se pueden ver animados, dando una especie de saltos en el eje z, esto para tener una visión adicional de los picos de la canción, ya en el apartado de geometry nodes la cosa fue mucho más compleja,
  se seleccionaron figuras dependiendo de como quería que se vieran distribuidas las partículas que se iban a mostrar, entre ellas una Ico Sphere, una UV Sphere y un Cube, la idea de estas figuras era limitar y controlar
  la generación de partículas con su tamaño, caras y orientación de normales, también, se crearon 3 materiales PBR bastante sencillos en Blender para las 3 partículas (Bajas, medias y altas), todo esto se fue
  organizando para que diera lugar a una animación visualmente atractiva, no muy cansada de ver, y en la cual se distinguieran los 3 tipos de picos

----

- Para el efecto del bajo, lo que hice fue generar partículas esféricas (IcoSpheres), sobre una geometría, en este caso otra esfera, cuando el objeto referenciado (Dos de los cubos del fondo) baja de cierta altura,
  lo cual fue hecho utilizando el componente de escala en Z, animado con la frecuencia baja, de esta forma, el objeto se escala en Z, más grande o pequeño, dependiendo de los picos alcanzados en las frecuencias bajas,
  Estas partículas crecen, se mantienen un tiempo (Si el pico sigue), y luego desaparecen disminuyendo su radio con una combinación de nodos de simulación. Todo esto es automático, se pueden modificar parámetros,
  pero no animación, se controla con simmulación y tiempos dinámicos (Frames o segundos, a elección).

- Algo muy similar al bajo hice con los altos, de hecho casi que lo mismo, sólo que se cambió el material, la geometría sobre la cual se generaba para abarcar un espacio diferente, más distribuido, y el tamaño, pero
  en esencia fue prácticamente lo mismo, de hecho se copió y pegó el árbol de nodos de los bajos, pero se ajustó a las frecuencias altas, de esta forma, en lugar de aparecer si bajan de cierta frecuencia, es si suben
  de la frecuencia establecida. De resto, más de lo mismo.

- En cuanto a los medios si fue algo diferente, este es una generación constante de partículas las cuales tienen una velocidad y un "age", que es básicamente el tiempo de vida, también es ajustable, estas se crean sobre
  una geometría, que en este caso fue una IcoSphere alargada que cubría casi toda la pantalla, y aquí hay algo particular, y es que estas aparecen y salen en dirección de las normales, pero yo no quería que surgieran de
  un lugar y salieran de pantalla, sino que abrazaran la escena, así que le volteé las normales a la geometría para lograr ese efecto de "agujero negro" en medio de la pantalla, estas como ya mencioné desaparecían
  dependiendo del tiempo puesto en el nodo de age, y su cantidad dependía de la cantidad de caras y de un factor multiplicador.

  Con estas configuraciones logré esta simulación, la cual como ya he mencionado, se puede ajutar, de hecho, pueden generarse geometrías específicas, se pueden cambiar las geometrías base que generan las partículas,
  se puede cambiar el por qué se generan, cuál es el input, el material es algo que fácilmente se cambia creando otro y cambiándolo en un nodo, en general es una plantilla reutilizable la cual puede dar lugar a un
  montón de visualizers mucho más complejos, o incluso se puede añadir a una escena animado como fuente de partículas al interactuar con x o y objeto, las posibilidades son infinitaaas.

  ----

  A continuación mostraré los dos árboles de nodo principales (bajos y altos) los cuales tienen ligeras modificaciones, y los materiales utilizados, al igual que los objetos dentro de la escena de Blender:

### *Escena de animación en Blender*


![image](https://github.com/user-attachments/assets/c8d7bb32-a46f-4010-a987-5bbd9f969891)

----

### *Material PBR para efecto de Bajo*

![image](https://github.com/user-attachments/assets/db6bb894-f267-4ae5-9561-d2d89289ae14)

----

### *Material PBR para efecto de Medios*

![image](https://github.com/user-attachments/assets/53afd43d-ab92-459c-b146-daf8e213c0f0)

----

### *Arbol de nodo de Altos (Muy similar al de bajos)*

![image](https://github.com/user-attachments/assets/2e82f4f3-cde6-4be1-bf5f-885ac310c3a2)

----

### *Arbol de nodo de Medios, Diferente*

![image](https://github.com/user-attachments/assets/95062e0c-f7f0-4ac5-b1d3-58a8a53ca024)







  
