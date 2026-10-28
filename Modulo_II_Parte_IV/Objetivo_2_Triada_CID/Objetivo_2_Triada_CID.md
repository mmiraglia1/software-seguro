
La tríada CID (Confidencialidad, Integridad y Disponibilidad) representa tres principios fundamentales de la seguridad de la información. Se utilizan para evaluar qué aspectos de la información o de un sistema pueden verse afectados por una vulnerabilidad o un ataque.

## 1. Vulnerabilidad que compromete la Confidencialidad

La confidencialidad consiste en garantizar que la información solamente pueda ser consultada por usuarios que tengan los permisos correspondientes.

Un ejemplo sería una vulnerabilidad de control de acceso en una aplicación web. Si existe una ruta como `/users/1` y un usuario puede cambiar el ID para consultar los datos de otra persona sin tener autorización, se estaría comprometiendo la confidencialidad del sistema, ya que podría acceder a información que no debería ver.

## 2. Vulnerabilidad que compromete la Integridad

La integridad consiste en garantizar que la información no sea modificada de manera no autorizada y que se mantenga correcta.

Un ejemplo sería una vulnerabilidad en el sistema de transferencias de una aplicación bancaria. Si una petición permite visualizar el ID de la cuenta de origen, el ID de destino y el monto a transferir, y el servidor acepta modificaciones de esos datos sin comprobar los permisos correspondientes, un usuario podría alterar la operación para transferir dinero desde una cuenta que no le pertenece o modificar el monto. Esto comprometería la integridad de la información y de las transacciones.

## 3. Vulnerabilidad que compromete la Disponibilidad

La disponibilidad consiste en garantizar que los usuarios autorizados puedan acceder a un sistema o servicio cuando lo necesiten.

Un ejemplo sería un ataque de denegación de servicio (DoS), en el que un atacante envía una gran cantidad de solicitudes a una página web para saturar sus recursos. Como consecuencia, el servidor podría dejar de responder o volverse inaccesible para los usuarios legítimos, afectando la disponibilidad del servicio.