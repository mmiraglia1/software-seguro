# Objetivo 4: Manejo Inseguro de Errores (Stack Traces)

## ¿Qué es un mensaje de error genérico?

Cuando una aplicación presenta un error, debería mostrar al usuario un mensaje sencillo que indique que ocurrió un problema, sin revelar detalles internos del sistema.

Por ejemplo:

```text
Ocurrió un error al procesar la solicitud.
Intente nuevamente más tarde.
```

Este tipo de respuesta informa que algo falló, pero no expone cómo funciona internamente la aplicación.

## ¿Qué es un Stack Trace?

Un **Stack Trace (traza de la pila)** es un registro técnico que muestra la secuencia de funciones o métodos que se ejecutaron hasta el momento en que ocurrió un error.

Es utilizado por los desarrolladores para identificar dónde y por qué falló una aplicación.

Por ejemplo, un Stack Trace podría mostrar:

```text
Fatal error: Uncaught Exception
/var/www/html/app/login.php:25

#0 /var/www/html/app/database.php:48
#1 /var/www/html/app/login.php:25
#2 {main}
```

Esta información es útil para depurar errores, pero no debería quedar expuesta a los usuarios en un entorno de producción.

## ¿Por qué es peligroso exponer un Stack Trace?

El problema es que un atacante puede utilizar la información técnica revelada para conocer detalles internos de la aplicación y preparar ataques más específicos.

Un Stack Trace detallado puede revelar:

- **Rutas internas:** ubicación de archivos y estructura de directorios del servidor.
- **Tecnologías utilizadas:** lenguaje de programación, frameworks, bibliotecas y versiones.
- **Nombres de funciones y métodos:** permite comprender parte del funcionamiento interno de la aplicación.
- **Consultas y errores de base de datos:** pueden revelar nombres de tablas, columnas o información sobre la estructura de almacenamiento.
- **Datos sensibles:** en algunos casos, pueden aparecer tokens, credenciales o parámetros utilizados durante la ejecución.

Esta información facilita la fase de reconocimiento y explotación porque reduce la incertidumbre del atacante. En lugar de trabajar a ciegas, puede identificar tecnologías, componentes y posibles puntos débiles para orientar sus intentos de ataque.

## Diferencia entre ambos tipos de respuesta

| Mensaje genérico | Stack Trace expuesto |
|---|---|
| Informa que ocurrió un error. | Muestra detalles técnicos del error. |
| No revela información interna. | Puede revelar rutas, tecnologías y código interno. |
| Es adecuado para producción. | Debe reservarse para entornos de desarrollo o registros internos protegidos. |
| Reduce la información disponible para un atacante. | Facilita el reconocimiento y la preparación de ataques. |

## ¿Cómo se mitiga?

Las principales medidas para evitar esta exposición son:

- **Deshabilitar la visualización de errores detallados:** evitar que los Stack Traces aparezcan en las respuestas enviadas a los usuarios.
- **Utilizar mensajes genéricos:** mostrar información sencilla sin revelar detalles internos.
- **Registrar los errores internamente:** guardar los Stack Traces en logs protegidos, accesibles únicamente para personal autorizado.
- **Separar desarrollo y producción:** permitir información detallada durante el desarrollo, pero deshabilitarla en el entorno público.
- **Proteger los registros:** evitar que los archivos de logs sean accesibles desde Internet.

## Resumen

Un mensaje de error genérico informa que ocurrió un problema sin revelar información interna de la aplicación. En cambio, un Stack Trace muestra detalles técnicos sobre la ejecución del programa y el origen del error.

Si esta información queda expuesta en producción, un atacante puede descubrir rutas internas, tecnologías, componentes y posibles debilidades del sistema, facilitando el reconocimiento y la explotación de vulnerabilidades.

La mitigación principal consiste en **mostrar errores genéricos al usuario y almacenar los detalles técnicos en registros internos protegidos**.

**Idea clave para recordar:** los errores detallados son útiles para los desarrolladores, pero pueden convertirse en una fuente de información para los atacantes si se muestran públicamente.