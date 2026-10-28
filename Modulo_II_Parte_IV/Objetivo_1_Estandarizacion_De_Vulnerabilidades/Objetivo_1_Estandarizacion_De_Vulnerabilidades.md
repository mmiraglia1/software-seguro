1. ¿Qué es un CVE?

CVE significa Common Vulnerabilities and Exposures, que en español se traduce como Vulnerabilidades y Exposiciones Comunes.

Es un sistema que sirve para identificar vulnerabilidades de seguridad que se conocen públicamente. A cada una se le asigna un código único, por ejemplo, CVE-2024-12345, para que todos puedan referirse al mismo problema sin confusiones.

Esto facilita que los investigadores, las empresas y las herramientas de seguridad puedan compartir información sobre una vulnerabilidad. Es importante aclarar que el CVE sirve para identificarla, pero no indica por sí solo qué tan grave es ni cómo solucionarla.

2. ¿Qué es CWE?

CWE significa Common Weakness Enumeration, o Enumeración de Debilidades Comunes.

Es un catálogo que reúne distintos tipos de errores o debilidades que pueden aparecer durante el desarrollo de un software o en el diseño de un sistema. Algunos ejemplos son los errores al validar datos ingresados por un usuario y los problemas relacionados con la gestión de memoria.

Conocer estas debilidades ayuda a entender por qué pueden aparecer ciertas vulnerabilidades y qué aspectos se deberían mejorar para prevenirlas.

3. Diferencia entre CVE y CWE

Aunque están relacionados, no significan lo mismo. El CVE identifica una vulnerabilidad concreta, mientras que el CWE describe el tipo de debilidad que puede haber provocado ese problema.

Por ejemplo, una versión de un programa puede tener una vulnerabilidad específica identificada con un CVE. Esa vulnerabilidad podría estar relacionada con una debilidad clasificada dentro del catálogo CWE.

En otras palabras, el CVE permite identificar un problema puntual y el CWE ayuda a entender qué clase de error hay detrás.

4. ¿Quién asigna los CVE? (CNAs)

Los CVE son asignados por organizaciones autorizadas que forman parte del programa CVE. Estas organizaciones se conocen como CVE Numbering Authorities (CNAs).

Entre ellas puede haber fabricantes de software, empresas de tecnología, organizaciones de investigación y equipos especializados en seguridad informática. Cada CNA puede asignar identificadores dentro del ámbito que tiene autorizado.

Cuando alguien descubre una vulnerabilidad, puede informar del problema al fabricante o a una organización correspondiente para que se analice y se gestione su identificación.

5. ¿Dónde se pueden consultar?

Existen varios sitios públicos donde se puede buscar información sobre vulnerabilidades y debilidades:

CVE.org: https://www.cve.org/
Es el sitio oficial del programa CVE y permite buscar vulnerabilidades por su identificador.

NVD (National Vulnerability Database): https://nvd.nist.gov/vuln
Es una base de datos del NIST que ofrece información adicional sobre vulnerabilidades, como productos afectados y puntuaciones de gravedad cuando están disponibles.

CWE de MITRE: https://cwe.mitre.org/
Permite consultar los distintos tipos de debilidades de seguridad y conocer sus características.

6. Herramientas automatizadas para detectar vulnerabilidades

Hay herramientas que permiten analizar sistemas y programas para encontrar vulnerabilidades conocidas. Algunas de ellas son:

Nessus: es un escáner de vulnerabilidades que revisa sistemas y servicios en busca de problemas de seguridad. Sus resultados pueden incluir el identificador CVE correspondiente y recomendaciones para solucionar el problema.
Documentación: https://docs.tenable.com/nessus/Content/Vulnerabilities.htm

Greenbone Community Edition (OpenVAS): permite realizar análisis de vulnerabilidades en equipos y servicios. Puede detectar problemas conocidos y generar informes con información sobre los hallazgos.
Documentación: https://greenbone.github.io/docs/latest/

Trivy: es una herramienta de código abierto que analiza, entre otras cosas, imágenes de contenedores y componentes de software para detectar vulnerabilidades conocidas, incluidas las identificadas mediante CVE.
Documentación: https://trivy.dev/docs/latest/scanner/vulnerability/

Estas herramientas ayudan a encontrar problemas de seguridad de forma automatizada, aunque no garantizan detectar todas las vulnerabilidades. Los resultados dependen de la configuración del análisis, los componentes examinados y la actualización de sus bases de datos.