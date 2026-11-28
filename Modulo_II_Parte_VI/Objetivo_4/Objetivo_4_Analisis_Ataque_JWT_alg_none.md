# Ataque de Algoritmo "None" en JWT

## Qué es un JWT

Un JSON Web Token tiene tres partes separadas por puntos:

```
<header>.<payload>.<signature>
```

- **Header**: indica el algoritmo de firma (`alg`) y el tipo (`typ`).
- **Payload**: contiene los *claims* (identidad, rol, expiración).
- **Signature**: firma criptográfica del `header.payload` usando una clave secreta.

La seguridad se basa en que el servidor **verifica la firma** antes de confiar en el payload. Sin la clave secreta, un atacante no puede producir una firma válida.

---

## El algoritmo `none`

El estándar JWT (RFC 7519 / RFC 7518) definió `none` como un valor válido para casos donde la firma se omite (por ejemplo, tokens internos en redes aisladas). Con `alg: "none"`, **la firma queda vacía**:

```
eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJ1c2VyIjoiYWRtaW4ifQ.
```

> Notar el punto final sin nada después.

---

## Por qué un backend vulnerable acepta identidades falsas

La falla no está en el estándar, sino en cómo el servidor procesa el token. Un backend es vulnerable cuando:

1. **Lee el algoritmo desde el propio token** (`header.alg`) en lugar de fijarlo por configuración.
2. **Acepta `none`** como valor válido, o compara de forma laxa (`None`, `NONE`, `null`, ausencia de `alg`).
3. **Saltea la verificación de firma** cuando detecta `none` ("no hay firma que verificar").
4. **No valida** que el algoritmo del token coincida con el esperado para el endpoint.

Bajo estas condiciones, el ataque es:

1. El atacante captura un JWT legítimo propio.
2. Decodifica header y payload (Base64 es reversible, no cifra).
3. Cambia el header a `{"alg":"none"}`.
4. Modifica el payload con la identidad que quiera suplantar (ej. `{"user":"admin"}`).
5. Elimina la firma.
6. Envía el token: el backend no verifica nada y confía en el payload.

**Resultado**: el atacante falsifica cualquier identidad **sin conocer la clave secreta**, porque el servidor nunca llega a comprobar que la firma corresponda al contenido.

---

## Causa raíz: *algorithm confusion*

El diseño de JWT exige que **el cliente no pueda influir en cómo se verifica la integridad del mensaje**. Cuando el backend deja que el *header* dicte el algoritmo, rompe la separación entre datos y metadatos de seguridad. La firma deja de ser un mecanismo de autenticación y se vuelve un campo opcional que el atacante puede simplemente apagar.

De la misma familia derivan otros ataques:

- **RS256 → HS256**: se firma con la clave pública (que es pública) como si fuera un secreto HMAC.
- **Inyección en `jku` / `jwk` / `kid`**: se apunta la verificación a material controlado por el atacante.

Todos comparten la misma raíz: **el servidor confía en el header para decidir cómo verificar**.

---

## Casos reales

- **CVE-2015-9235**: la librería `jsonwebtoken` de Node.js aceptaba `alg: "none"`.
- **CVE-2016-5431 / CVE-2016-10555**: múltiples librerías afectadas por el mismo patrón.
- Documentado en **OWASP JWT Cheat Sheet** y como laboratorio en **PortSwigger Web Security Academy**.

---

## Mitigación

1. **Fijar el algoritmo esperado en el servidor**: nunca leerlo del token (ej. `algorithms: ["HS256"]`).
2. **Rechazar `none` explícitamente** y cualquier variante de mayúsculas/formatos.
3. **Verificar la firma siempre**, sin excepciones: si no hay firma, el token es inválido por definición.
4. **Validar claims críticos** (`iss`, `aud`, `exp`, `nbf`, `sub`) contra el recurso solicitado.
5. **Usar librerías actualizadas** cuya API obligue a declarar el algoritmo esperado.
6. **Monitorear** tokens con `alg` inesperado o firma vacía (señal clara de ataque).

---

## Conclusión

El algoritmo `none` no es una falla criptográfica del estándar, sino una **falla de implementación** del backend que permite que el cliente decida cómo se verifica la integridad del token. Al aceptar `alg: "none"`, el servidor renuncia a la verificación de firma y convierte al JWT en un contenedor Base64 manipulable. Un atacante puede reescribir el payload con cualquier identidad y el servidor la aceptará, sin necesidad de conocer la clave secreta.