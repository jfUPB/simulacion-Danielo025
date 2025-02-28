#### Aceleraciones

- Aceleración constante: Realmente en la actividad anterior prácticamente realicé el experimento con la aceleración constante, cosa que honestamente no fue a propósito, pero claramente la conclusión es la misma, debo decir que es un hecho que 
en un principio todo parece normal, el objeto inicia con una velocidad determinada y empieza a acelerar muy lentamente debido a los valores, se ve bien, se ve cómodo y pues se nota la aceleración, sin embargo llega un
punto en que esto se vuelve incontrolable y el objeto debido a limitaciones visuales y por otro lado falta de límites de velocidad, llega un punto en que su visualización se vuelve errática, parece estarse teletrasportando
por todos lados en la pantalla.

Todo esto se debe a que la aceleración suma y suma, y en ningún momento se limita su velocidad. **No hay ningún control prácticamente.**

- Aceleración aleatoria: En este caso fue algo bastante más cómodo de ver, es cierto que puede que el objeto llege a ser bastante rápido, sin embargo al ser una aceleración aleatoria, es muy probable que sea entre un rango,
lo cual hace que el objeto acelere y frene constantemente, incluso puede que cambie de de trayectoria y se devuelva, sin necesidad de llamar a la función checkEdges(), esto siempre y cuando las aceleraciones se pongan en
un rango negativo, positivo; también se puede decir que con el simple hecho de tener una aceleración aleatoria, este control pasa a ser mínimo, ya que aunque acelere más o menos, sigue acelerando, lo ideal para lograr
el efecto antes mencionado, es la técnica antes mencionada, es decir, una aceleración aleatoria entre un rango negativo, positivo, para así tener ciclos de aceleración y frenados constantes, de manera aleatoria.

Esto depende completamente de los rangos puestos en la aleatoreidad, es decir, puede ser una aceleración tirando a constante, pero más o menos rápida, o puede ser un movimiento aleatorio de un lado a otro. **Hay un control mediano.**

- Aceleración hacia el mouse: En este caso tenemos un control mucho más avanzado, e incluso podemos llegar a tener prácticamente limitaciones de velocidad, al tener una aceleración hacia el mouse podemos hacer que el objeto
se acelere constamente hacia una posición, o no tiene que ser hacia una, pero si podemos hacer que acelere constantemente, también, podemos hacer que el objeto acelere y frene, hacia donde queremos todo el tiempo, podemos
ponerlo a dar vueltas en un lugar, a trazar una cierta línea con una especie de suavizado dependiendo de la aceleración que tenga en el momento, etc, es algo que podría servir bastante como aplicación interactiva,
y probablemente este sea uno de los principios que siguen aplicaciones como **Photoshop para realizar el suavizado de trazos al momento de estar dibujando.**

Esto depende plenamente de hacia donde movamos el mouse y qué tanta ventaja le dejemos coger a la aceleración. **Tenemos un control casi total.**
