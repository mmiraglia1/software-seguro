# Objetivo 2: Ingeniería Inversa a JSON Web Tokens (JWT)

## 1. Obtención del JWT

Para realizar el laboratorio se utilizó **TotalShiftLeft API Sandbox**, una aplicación web de pruebas que permite simular procesos de autenticación y generar tokens JWT sin utilizar cuentas ni información personal real.

Sitio utilizado: https://demo.totalshiftleft.ai/ui

El procedimiento fue el siguiente:

1. Se ingresó a la interfaz web de API Sandbox.
2. Se localizó el endpoint de autenticación `POST /auth/token`.
3. Se utilizaron las credenciales de prueba proporcionadas por el entorno.
4. Se ejecutó la solicitud de autenticación para generar un token.
5. Se inspeccionó la respuesta HTTP y se identificó el campo que contenía el JWT generado (`access_token`).
6. Finalmente, se decodificó el token en jwt.io para examinar su Header y Payload.


## 2. Análisis del Header

El Header contiene información técnica sobre el tipo de token y el algoritmo utilizado para firmarlo.

Contenido obtenido:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

**Interpretación:**

- `alg`: indica el algoritmo de firma utilizado. En este caso, `HS256`, que corresponde a HMAC utilizando SHA-256.
- `typ`: indica el tipo de token, en este caso JWT.

Por lo tanto, el token utiliza un mecanismo de firma simétrica basado en una clave secreta compartida entre quien genera el token y quien verifica su firma.

## 3. Análisis del Payload

El Payload contiene los datos o atributos asociados al usuario y al contexto de autenticación.

Contenido obtenido:

```json
{
  "sub": "usr_bob",
  "name": "Bob",
  "role": "user",
  "iss": "api-sandbox",
  "aud": "sandbox-client",
  "iat": 1795667079,
  "exp": 1795667679
}
```

**Interpretación de los campos:**

- `sub`: identifica al sujeto al que pertenece el token. En este caso, `usr_bob`.
- `name`: contiene el nombre del usuario, en este ejemplo, Bob.
- `role`: indica el rol o nivel de permisos asignado. El valor observado es `user`.
- `iss`: identifica al emisor del token (`api-sandbox`).
- `aud`: identifica al destinatario previsto del token (`sandbox-client`).
- `iat`: indica la fecha y hora en que fue emitido el token.
- `exp`: indica la fecha y hora de vencimiento del token.

## 4. Información expuesta en texto plano

El análisis permite comprobar que el Payload puede ser decodificado sin conocer la clave secreta utilizada para firmar el JWT.

En este ejemplo quedan visibles:

- Un identificador de usuario (`usr_bob`).
- El nombre asociado al usuario (`Bob`).
- Su rol dentro de la aplicación (`user`).
- Información sobre el emisor, el destinatario y la vigencia del token.

Aunque los datos utilizados son ficticios, esto demuestra que si una aplicación incorpora información personal, identificadores internos o roles en el Payload, esos datos pueden ser leídos por cualquier persona que tenga acceso al token.

Es importante aclarar que la firma permite verificar la integridad y autenticidad del token, pero no cifra su contenido.

## 5. Conclusión

A partir del análisis realizado se identificó que el JWT utiliza el algoritmo de firma `HS256` y contiene información del usuario y de su contexto de autenticación en el Payload.

Se comprobó que los datos del Payload están codificados, pero no cifrados, por lo que pueden ser interpretados sin disponer de la clave secreta. También se identificó el atributo `role`, que permite conocer el rol asignado al usuario.

Como medida de seguridad, las aplicaciones deberían evitar incluir información sensible innecesaria dentro del Payload y proteger adecuadamente los tokens, ya que cualquier persona que obtenga uno puede leer su contenido. Además, el servidor debe verificar siempre la firma, la expiración y los permisos antes de autorizar una operación.