# Análisis de Manipulación Transaccional (Cantidades Negativas)

## ¿Por qué ocurre la falla?
El problema radica en una falla de lógica de negocio (Insecure Design) causada por la **falta de validación del lado del servidor** y la confianza excesiva en los datos enviados por el cliente. 

Cuando el servidor recibe la petición con `cantidad: -5`, no verifica que el valor sea lógicamente posible (mayor a cero). Al ejecutar el cálculo matemático interno (`Total = Precio * Cantidad`), el resultado es un número negativo. Posteriormente, al procesar el pago o actualizar el saldo del usuario aplicando la fórmula `Saldo = Saldo - Total`, un total negativo invierte la operación matemática, sumando dinero a la cuenta del atacante en lugar de restarle.

## ¿Cómo debería validar el desarrollador?
La validación debe ser **estricta, del lado del servidor** y aplicarse antes de interactuar con la base de datos o pasarelas de pago:

1. **Validación de Tipo y Rango:** Verificar que el campo sea un número entero y que su valor sea estrictamente mayor a cero (`cantidad > 0`). Rechazar valores negativos, cero o tipos de datos inesperados (strings, arrays).
2. **Límites Lógicos (Min/Max):** Establecer un límite máximo razonable por transacción (ej. `cantidad <= 10`) para prevenir abusos por volumen o desbordamiento de enteros.
3. **Nunca confiar en el cliente para los cálculos:** El servidor debe recalcular el precio total consultando la base de datos (Precio del producto) y multiplicarlo por la cantidad validada, ignorando cualquier precio o total que intente enviar el frontend.
4. **Validación de Stock:** Confirmar que la cantidad solicitada es menor o igual al stock real disponible en el inventario antes de procesar la compra.