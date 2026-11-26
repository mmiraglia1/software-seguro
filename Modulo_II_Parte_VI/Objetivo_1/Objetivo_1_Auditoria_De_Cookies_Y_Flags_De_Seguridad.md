# Objetivo 1: Auditoría de Cookies y Banderas de Seguridad

## 1. Objetivo de la investigación

Analizar las cookies de tres sitios web de uso público mediante el inspector del navegador, identificando si implementan las banderas de seguridad `HttpOnly` y `Secure`.

Los sitios seleccionados fueron Google, YouTube y Mercado Libre Argentina.

El análisis se realizó desde las herramientas de desarrollo del navegador, en la sección **Application → Storage → Cookies**, observando el nombre, dominio, fecha de expiración y atributos de seguridad de las cookies.

**Importante:** los resultados incluyen todas las cookies visibles de los dominios analizados, no exclusivamente las utilizadas para mantener sesiones. Por eso, los porcentajes representan la configuración general de las cookies observadas y no una evaluación definitiva de la seguridad de autenticación de cada sitio.

## 2. Resultados por sitio web

### A. Google

**Sitio:** https://www.google.com/

Se identificaron 22 cookies.

| Indicador | Resultado |
|---|---:|
| Cookies analizadas | 22 |
| HttpOnly activado | 14 (63,6 %) |
| HttpOnly desactivado | 8 (36,4 %) |
| Secure activado | 17 (77,3 %) |
| Secure desactivado | 5 (22,7 %) |

**Cookies identificadas:**

`__Secure-BUCKET`, `__Secure-OSID`, `__Secure-STRP`, `AEC`, `NID`, `OSID`, `SEARCH_SAMESITE`, `SNID`, `__Secure-1PAPISID`, `__Secure-1PSID`, `__Secure-1PSIDCC`, `__Secure-1PSIDTS`, `__Secure-3PAPISID`, `__Secure-3PSID`, `__Secure-3PSIDCC`, `__Secure-3PSIDTS`, `APISID`, `HSID`, `SAPISID`, `SID`, `SIDCC` y `SSID`.

**Observaciones:**

- Se encontraron cookies sin `HttpOnly`, entre ellas `APISID`, `SAPISID` y `SID`.
- Se identificaron cookies sin `Secure`, como `APISID`, `HSID`, `SID`, `SIDCC` y `SEARCH_SAMESITE`.
- Varias cookies con el prefijo `__Secure-` presentan la bandera `Secure` activada.

Esto demuestra que no todas las cookies tienen las mismas restricciones de acceso y transmisión. Las cookies relacionadas con identificación y autenticación merecen especial atención.

### B. YouTube

**Sitio:** https://www.youtube.com/

Se identificaron 26 cookies correspondientes al dominio de YouTube.

| Indicador | Resultado |
|---|---:|
| Cookies analizadas | 26 |
| HttpOnly activado | 16 (61,5 %) |
| HttpOnly desactivado | 10 (38,5 %) |
| Secure activado | 19 (73,1 %) |
| Secure desactivado | 7 (26,9 %) |

**Cookies identificadas:**

`APISID`, `HSID`, `LOGIN_INFO`, `NID`, `PREF`, `SAPISID`, `SID`, `SIDCC`, `SSID`, `VISITOR_INFO1_LIVE`, `VISITOR_PRIVACY_METADATA`, `YSC`, `__Secure-1PAPISID`, `__Secure-1PSID`, `__Secure-1PSIDCC`, `__Secure-1PSIDTS`, `__Secure-3PAPISID`, `__Secure-3PSID`, `__Secure-3PSIDCC`, `__Secure-3PSIDTS`, `__Secure-BUCKET`, `__Secure-ROLLOUT_TOKEN`, `__Secure-YNID`, `_ga`, `_ga_5JSYX2Q357` y `_gcl_au`.

**Observaciones:**

- Se encontraron cookies sin `HttpOnly`, entre ellas `APISID`, `SAPISID` y `SID`.
- Se identificaron cookies sin `Secure`, como `APISID`, `HSID`, `SID`, `SIDCC` y algunas relacionadas con analítica.
- Las cookies con el prefijo `__Secure-` presentan la bandera `Secure` activada.

Algunas cookies están relacionadas con identificación y autenticación, mientras que otras cumplen funciones de preferencias, funcionamiento de la plataforma o medición. Por eso, la ausencia de una bandera no implica automáticamente que exista una vulnerabilidad.

### C. Mercado Libre Argentina

**Sitio:** https://www.mercadolibre.com.ar/

Se identificaron 20 cookies.

| Indicador | Resultado |
|---|---:|
| Cookies analizadas | 20 |
| HttpOnly activado | 8 (40 %) |
| HttpOnly desactivado | 12 (60 %) |
| Secure activado | 19 (95 %) |
| Secure desactivado | 1 (5 %) |

**Cookies identificadas:**

`_csrf`, `_d2id` (dos registros de dominios diferentes), `_mldataSessionId`, `c_Z2fAvvr`, `c_ZnjOaT`, `cookiesPreferencesLogged`, `cookiesPreferencesLoggedFallback`, `cookiesPreferencesNotLogged`, `cp`, `ftid`, `g_state`, `hide-cookie-banner`, `inferredZipcode`, `ml_cart-quantity`, `nsa_rotok`, `orgnickp`, `orguserid`, `orguseridp` y `ssid`.

**Observaciones:**

- Se encontraron cookies sin `HttpOnly`, entre ellas `_mldataSessionId`, `orguserid`, `orguseridp` y `orgnickp`.
- La única cookie sin `Secure` identificada fue `g_state`.
- La cookie `_csrf` presenta ambas banderas activadas.
- La cookie `nsa_rotok` también presenta ambas protecciones.

Mercado Libre presentó el mayor porcentaje de cookies con `Secure` activado, aunque también el menor porcentaje de cookies con `HttpOnly`.

## 3. Comparación general

| Indicador | Google | YouTube | Mercado Libre |
|---|---:|---:|---:|
| Cookies analizadas | 22 | 26 | 20 |
| HttpOnly activado | 63,6 % | 61,5 % | 40 % |
| Secure activado | 77,3 % | 73,1 % | 95 % |
| Sin HttpOnly | 8 | 10 | 12 |
| Sin Secure | 5 | 7 | 1 |

**Conclusiones de la comparación:**

- Google y YouTube presentan porcentajes similares de implementación de ambas banderas.
- Mercado Libre presenta el mayor porcentaje de cookies con `Secure` activado.
- Mercado Libre presenta el menor porcentaje de cookies con `HttpOnly` activado.
- En los tres sitios se encontraron cookies sin una o ambas banderas de seguridad.

Estos resultados no permiten establecer por sí solos cuál de los tres sitios es más seguro, porque las cookies cumplen funciones diferentes y no todas contienen identificadores de sesión.

## 4. Preguntas de la consigna

### ¿Qué protege exactamente la bandera HttpOnly?

La bandera `HttpOnly` impide que los scripts ejecutados en el navegador, como JavaScript, puedan acceder directamente al contenido de una cookie mediante mecanismos como `document.cookie`.

Su objetivo principal es reducir el riesgo de robo de identificadores de sesión ante vulnerabilidades XSS (Cross-Site Scripting).

Por ejemplo, si un atacante consigue ejecutar JavaScript en el navegador de una víctima, no podrá leer directamente una cookie que tenga activada esta bandera.

Sin embargo, `HttpOnly` no elimina las vulnerabilidades XSS ni impide que un atacante realice acciones desde el navegador comprometido utilizando la sesión de la víctima.

### ¿Qué riesgo corre el usuario si la cookie de sesión no tiene activada la bandera Secure?

La bandera `Secure` indica que el navegador solamente debe enviar la cookie mediante conexiones HTTPS.

Si no está activada, la cookie también podría enviarse mediante una conexión HTTP sin cifrar, dependiendo de las condiciones de navegación y de las protecciones adicionales del sitio.

En ese escenario, un atacante que pueda interceptar el tráfico de red podría obtener el contenido de la cookie. Si contiene un identificador de sesión válido, podría intentar utilizarlo para suplantar al usuario y acceder a su cuenta.

Por lo tanto, la bandera `Secure` ayuda a proteger la confidencialidad de las cookies durante su transmisión.

## 5. Conclusión final

A partir del análisis realizado, se comprobó que Google, YouTube y Mercado Libre utilizan cookies con diferentes configuraciones de seguridad.

Si bien los tres sitios presentan cookies con las banderas `HttpOnly` y `Secure` activadas, también se identificaron otras que no cuentan con alguna de estas protecciones.

La bandera `HttpOnly` es importante para reducir el riesgo de exposición de identificadores de sesión mediante JavaScript, mientras que `Secure` evita que las cookies se envíen mediante conexiones HTTP sin cifrar.

La investigación también permitió observar que no todas las cookies cumplen la misma función: algunas intervienen en la autenticación, mientras que otras almacenan preferencias, identificadores de funcionamiento o información de analítica.

Por este motivo, para determinar si existe una configuración insegura es necesario identificar primero cuáles cookies participan realmente en la gestión de sesiones y evaluar sus atributos de seguridad.

## 6. Fuentes de consulta

- MDN Web Docs – HTTP Cookies: https://developer.mozilla.org/es/docs/Web/HTTP/Guides/Cookies
- MDN Web Docs – Set-Cookie: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie
- OWASP – Session Management Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html
- OWASP – Testing for Cookies Attributes: https://owasp.org/www-project-web-security-testing-guide/