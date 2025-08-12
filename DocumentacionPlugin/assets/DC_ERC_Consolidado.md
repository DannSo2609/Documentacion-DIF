# Documentación funcional del plugin DC_ERC_Consolidado 

## Descripción general

El plugin DC_ERC_Consolidado  es un componente para Microsoft Dynamics 365 que automatiza el envío de reportes de carga detallados a los clientes (consignatarios) asociados a los recibos de un consolidado. Cada cliente recibe un correo electrónico personalizado con una tabla que resume todos sus recibos y los totales de piezas, peso y volumen.

## Propósito

El objetivo principal de este plugin es agilizar y estandarizar la comunicación de información relevante de la carga a cada cliente involucrado en un consolidado, asegurando que reciban datos precisos, estructurados y fáciles de consultar. El proceso se ejecuta de forma automática y está diseñado para ser robusto ante errores.

## Flujo general

1. **Condición de ejecución:**  
   El plugin se activa únicamente si el campo de control para enviar reporte a los clientes está marcado en el consolidado.

2. **Recuperación del consolidado:**  
   Se obtiene el registro de consolidado actual para usar su identificador y otros datos relevantes en los reportes y correos.

3. **Búsqueda y agrupamiento de recibos:**  
   Se consultan todos los recibos relacionados con el consolidado.  
   Los recibos se agrupan por consignatario interno. Si algún recibo no tiene consignatario, el proceso lanza una excepción para advertir del problema.

4. **Generación de correos electrónicos:**  
   Para cada consignatario:
   - Se crea un correo electrónico personalizado.
   - El asunto del correo contiene el nombre del consignatario.
   - El destinatario es el consignatario y, si existen, se agregan en copia (CC) los contactos asociados relevantes.
   - El cuerpo del correo incluye un saludo, una breve explicación y una tabla HTML con los datos de los recibos de ese consignatario.
   - La tabla incluye una fila por recibo con columnas para índice, estado, número, fecha, embarcador, consignatario, piezas, peso, volumen, notas y estado de aprobación.
   - Al final de la tabla se agrega una fila con los totales de piezas, peso y volumen de todos los recibos de ese consignatario.
   - Se añade un logotipo institucional al pie del correo para identidad visual.

5. **Envío de correos electrónicos:**  
   Se guarda y se envía cada correo como actividad de Dynamics 365, gestionando posibles errores de forma interna para no detener el proceso global.

6. **Gestión de errores:**  
   Todos los errores durante la creación, agrupamiento, consulta o envío de correos se almacenan en una lista interna de errores, permitiendo que el proceso continúe con el siguiente consignatario.

7. **Inclusión de contactos asociados en copia:**  
   El plugin incluye un método auxiliar para buscar y agregar como copia (CC) a los contactos relevantes del consignatario, según su tipo de contacto.

8. **(Opcional, comentado en el código) Notificación al usuario:**  
   El código incluye, aunque comentado, un bloque para crear una notificación visual en Dynamics al usuario que ejecutó el proceso, informando que los correos fueron enviados exitosamente.

## Consideraciones adicionales

- El plugin valida que todos los recibos tengan consignatario interno, lo que es obligatorio para el correcto funcionamiento.
- Los datos numéricos de piezas, peso y volumen se suman y presentan como totales en la tabla del correo para facilitar la consulta del cliente.
- Los errores en la consulta o envío se gestionan internamente para que no afecten a la experiencia del usuario ni detengan el proceso completo.
- Los correos están diseñados con estilos visuales para ser claros, legibles y profesionales.
- La lógica del plugin es extensible y puede adaptarse para incluir más información o funcionalidades adicionales si es necesario.

## Resumen

El plugin DC_ERC_Consolidado automatiza la comunicación de reportes de carga a los clientes involucrados en un consolidado, enviando un resumen detallado de sus recibos por correo electrónico. Garantiza la integridad de la información, la presentación visual y la continuidad del proceso ante posibles errores puntuales.