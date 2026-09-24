# TLS Handshake

El **TLS Handshake** es el proceso previo de comunicación entre Cliente-Servidor antes de realizar la primera interacción HTTP, donde se ponen de acuerdo en cómo va a ser la seguridad y cifrado en la cual se va a establecer la comunicación entre ambos.

## Flujo de Comunicación

```
sequenceDiagram
    autonumber
    actor Cliente
    actor Servidor

    Cliente->>Servidor: 1. Solicito comunicarme de forma segura con tus servicios
    Servidor-->>Cliente: 2. Te paso mi certificado digital
    Cliente->>Servidor: 3. Verifico que el certificado sea valido y corresponda al servidor
    Servidor-->>Cliente: 4. Mecanismos de criptografia asimetrica para intercambio seguro de claves
    Cliente->>Servidor: 5. Establecen una clave de sesion compartida
    Servidor-->>Cliente: 6. Cifrado simetrico para proteger la comunicacion HTTP
    Cliente->>Servidor: HTTP Request
    Servidor-->>Cliente: HTTP Response

```

## Pasos del Proceso

1. **Solicitud de inicio:**

   > *Solicito comunicarme de forma segura con tus servicios*

2. **Envío de certificado:**

   > *Te paso mi certificado digital*

3. **Validación del certificado:**

   > *Verifico que el certificado sea válido y corresponda al servidor al que quiero conectarme. Si la validación es correcta, puedo continuar con el Handshake; si no es válido, se interrumpe la conexión.*

4. **Intercambio de claves (Criptografía Asimétrica):**

   > *Una vez validado el certificado, cliente y servidor utilizan mecanismos de criptografía asimétrica para realizar de forma segura el intercambio de claves necesarias para la comunicación.*

5. **Clave de sesión compartida:**

   > *Establecen una clave de sesión compartida para proteger la comunicación.*

6. **Cifrado Simétrico:**

   > *Utilizan la clave de sesión mediante cifrado simétrico para proteger la comunicación HTTP.*

7. **Tráfico HTTP Seguro:**

   * **Cliente → Servidor:** `HTTP Request`

   * **Servidor → Cliente:** `HTTP Response`