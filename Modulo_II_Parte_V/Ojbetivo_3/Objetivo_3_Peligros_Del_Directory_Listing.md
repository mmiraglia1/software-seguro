# Objetivo 3: Peligros del Directory Listing

## ¿Qué es Directory Listing?

El **Directory Listing** es una funcionalidad del servidor web que permite mostrar automáticamente el contenido de un directorio cuando un visitante accede a él y no existe un archivo de índice, como `index.html` o `index.php`.

Normalmente, cuando alguien accede a una carpeta de un sitio web, el servidor busca un archivo de inicio. Si no lo encuentra y el listado de directorios está habilitado, genera una página que muestra los archivos y subdirectorios disponibles.

Por ejemplo, al ingresar a:

`https://ejemplo.com/archivos/`

El servidor podría mostrar un listado como este:

```text
Index of /archivos/

documento.pdf
usuarios.csv
backup.sql
configuracion.txt
```

Esto permite que cualquier visitante pueda conocer los nombres de los archivos y, si tienen permisos de acceso público, descargarlos directamente.

## ¿Por qué representa un riesgo de seguridad?

El problema es que un atacante podría descubrir información que no estaba destinada a ser pública.

Algunos archivos sensibles que podrían quedar expuestos son:

- **Archivos de configuración:** pueden contener credenciales, claves API, usuarios y contraseñas de bases de datos.
- **Copias de seguridad:** archivos `.bak`, `.zip`, `.tar` o `.sql` que podrían contener información de la aplicación o de sus usuarios.
- **Archivos de logs:** pueden revelar errores, rutas internas, direcciones IP y otros datos del sistema.
- **Archivos temporales:** pueden contener información que no debería estar disponible públicamente.
- **Código fuente:** puede revelar cómo funciona la aplicación y mostrar detalles de su implementación.

Incluso cuando un archivo no contiene información sensible, conocer la estructura de directorios y los nombres de los recursos puede facilitar el reconocimiento del sistema.

## ¿Cómo se mitiga?

La principal medida es **deshabilitar el listado automático de directorios** en la configuración del servidor web.

Algunos ejemplos:

**Apache**

En la configuración del servidor se puede utilizar:

```apache
<Directory /var/www/html>
    Options -Indexes
</Directory>
```

La opción `-Indexes` deshabilita la generación automática de listados.

**Nginx**

Dentro del bloque `server` o `location` correspondiente:

```nginx
location /archivos/ {
    autoindex off;
}
```

La directiva `autoindex off` evita que Nginx genere listados de directorios.

Otras medidas recomendadas:

- No almacenar archivos sensibles dentro de directorios accesibles públicamente.
- Restringir el acceso a archivos de configuración, copias de seguridad y registros.
- Evitar publicar archivos temporales o respaldos en el directorio web.
- Revisar periódicamente los permisos y la configuración del servidor.

## Resumen

El Directory Listing puede convertirse en un problema de seguridad cuando permite que cualquier visitante vea el contenido de directorios del servidor.

Un atacante podría aprovecharlo para descubrir archivos, obtener información sensible y recopilar datos útiles para futuros ataques.

La mitigación principal consiste en **deshabilitar el listado de directorios**, restringir el acceso a los archivos sensibles y evitar almacenar información privada dentro de directorios públicos.

**Idea clave para recordar:** si un directorio no tiene un archivo de índice y el Directory Listing está habilitado, el servidor puede mostrar su contenido en lugar de denegar el acceso o devolver un error.