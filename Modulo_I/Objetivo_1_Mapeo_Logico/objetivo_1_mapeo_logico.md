# Diagrama de arquitectura

```mermaid
flowchart LR
    U["Usuario<br/>(PC, Celular, Tablet, etc.)"]
    C["Cliente<br/>(Navegador web)"]
    S["Servidor<br/>(Servidor web)"]
    DB[("Base de datos")]

    U -->|"Ingresa a Google<br/>desde el navegador"| C

    C -->|"Solicitud HTTP GET<br/>a través de HTTPS"| S
    S -->|"Respuesta HTTP<br/>(200 OK, 403 Forbidden, 500 Internal Server Error, etc.)"| C

    S -->|"Consulta datos"| DB
    DB -->|"Devuelve datos"| S