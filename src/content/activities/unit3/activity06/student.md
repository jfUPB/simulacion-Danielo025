#### Aceleración controlada

- Esto tiene una explicación muy importante, y es que la aceleración es una fuerza acumulativa, si esta fuerza no se reinicia, las fuerzas anteriores seguirán afectando al objeto indefinidamente, y es que
una vez que la aceleración ha sido usada para actualizar la velocidad, ya no debe afectar el siguiente frame, esto sólo lograría que el objeto siga acelerando indefinidamente aunque no haya más fuerzas aplicadas,
algo que de hecho me llegó a pasar en la unidad anterior y por eso controlé la velocidad, ahora veo que realmente fue un "machetazo", pero como anteriormente dije, funcionó y en su momento, aunque ahora me doy cuenta que no era lo correcto, era lo importante.

- Ahora, ¿por qué al final de update? se hace justo al final de update porque la aceleración primero debe cumplir su propósito antes de ser reseteada, esto quiere decir que primero debe afectar la velocidad, la cual afectaría la posición,
una vez hecho esto, volvemos a lo anterior, se debe reiniciar para que no la siga afectando, pues ya hizo su trabajo, y de ser necesario, se le volvería a llamar, si por ejemplo se nos diera por reiniciarla antes,
eliminaríamos la aceleración antes de actualizar la velocidad, lo cual rompería el sistema de movimiento y tampoco es lo que queremos, la idea es lograr un sistema de movimiento coherente y natural.
