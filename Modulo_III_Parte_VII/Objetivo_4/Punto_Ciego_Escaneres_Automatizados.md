# El Punto Ciego de los Escáneres Automatizados

## ¿Por qué la lógica de negocio es el mayor "punto ciego"?
Las herramientas automatizadas (como Nessus, Acunetix o Burp Scanner) funcionan mediante reglas, firmas y heurísticas. Buscan patrones técnicos conocidos: inyecciones SQL, scripts XSS, cabeceras de seguridad faltantes o configuraciones erróneas. 

El problema es que **los escáneres no entienden el contexto del negocio**. Para un escáner, enviar un parámetro `cantidad: -5` es técnicamente válido porque es un número entero. La herramienta no sabe que en el mundo real, un carrito de compras no puede tener cantidades negativas. No comprende la diferencia entre un dato sintácticamente correcto y uno lógicamente válido.

## ¿Por qué la auditoría manual es indispensable?
Un pentester humano puede mapear el flujo de negocio, entender cómo *debería* funcionar la aplicación y luego idear formas de romper esas reglas. 

Los escáneres no pueden detectar:
- **Abusos de flujo:** Saltarse pasos en un proceso de pago o aprobación.
- **Condiciones de carrera:** Enviar peticiones simultáneas para duplicar beneficios.
- **Manipulación de estados:** Cambiar un precio o forzar un saldo a favor.
- **Lógica de roles:** Realizar acciones que el rol de usuario no debería permitir aunque el endpoint responda `200 OK`.

## Conclusión
La automatización es excelente para encontrar vulnerabilidades de **implementación** (errores de código). Sin embargo, las fallas de **diseño** o lógica de negocio solo pueden ser descubiertas por la mente humana, que es capaz de entender el valor y el propósito de la transacción, no solo el código HTTP de por medio.