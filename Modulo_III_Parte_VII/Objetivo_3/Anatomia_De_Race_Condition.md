# Anatomía de una Condición de Carrera (Race Condition)

## ¿Por qué ocurre la falla? (El concepto TOCTOU)
El problema radica en el tiempo que existe entre que el servidor **verifica** una condición y **ejecuta** la acción (Time-of-Check to Time-of-Use o TOCTOU). 

Cuando un usuario canjea un cupón, el servidor normalmente hace dos pasos:
1. **Verifica (Check):** Consulta la base de datos para ver si el código existe y no ha sido usado.
2. **Actúa (Use):** Aplica el descuento y marca el código como "usado" en la base de datos.

Si el atacante envía 50 peticiones en el mismo instante, las 50 llegan al paso de "Verifica" antes de que la primera termine el paso de "Actúa". La base de datos le responde "Válido" a las 50 peticiones, y el atacante logra aplicar el descuento 50 veces.

## ¿Cómo lo automatiza el atacante? (Turbo Intruder)
Las herramientas tradicionales (o el navegador) envían peticiones de forma secuencial; la latencia de red hace que la primera petición termine antes de que salga la segunda. 

Para explotar esto, se usa **Turbo Intruder** (o la función *Send group in parallel* de Burp Suite). El atacante intercepta la petición de canje, la envía a la herramienta y configura un script para que realice un **Single-Packet Attack**. Esta técnica empaqueta las 50 peticiones HTTP/2 en un único paquete TCP o sincroniza el último byte de cada petición. Al viajar juntas, llegan al servidor exactamente en el mismo milisegundo, anulando el *jitter* (la variación de latencia de la red) y colisionando la lógica del servidor.

## El flujo del ataque
1. Se captura la petición legítima de canje (`POST /aplicar-cupon` con el código).
2. Se envía a Turbo Intruder.
3. El script configura 50 conexiones concurrentes y encola 50 peticiones idénticas.
4. Al disparar, el servidor recibe las 50, todas pasan la validación y el saldo se acredita múltiples veces.
5. En las respuestas, el atacante observa múltiples `200 OK` en lugar de un solo éxito seguido de errores `400 (Cupón ya usado)`.