# Documentación funcional del plugin dc_era_consolidado

## Descripción general

El plugin dc_era_consolidado está diseñado para Dynacorp y su función principal es automatizar el envío de un correo electrónico al agente asignado a un consolidado, incluyendo una tabla detallada de los recibos no aprobados asociados a ese consolidado. De esta manera, mejora la comunicación y seguimiento de las instrucciones de embarque para los agentes, permitiendo que reciban la información relevante de manera estructurada y visualmente adecuada.

## Propósito

Automatizar el envío de instrucciones de embarque y el reporte de recibos pendientes al agente relacionado con un consolidado, presentando los datos en formato tabla HTML y gestionando errores de forma interna para no detener el flujo de trabajo.

## Flujo general

1. **Condición de ejecución:**  
   El plugin se activa únicamente si el campo 'enviar reporte para agente' está marcado en el consolidado.

2. **Búsqueda de recibos no aprobados:**  
   Se realiza una consulta para obtener los recibos asociados al consolidado actual cuyo estado de aprobación aún es falso, seleccionando los datos relevantes de cada recibo.

3. **Validación del agente:**  
   Se verifica que el consolidado tenga un agente asignado. En caso contrario, el proceso se detiene y se lanza una excepción.

4. **Construcción del correo electrónico:**  
   Se crea una actividad de correo electrónico en Dynamics que incluye:
   - Asunto con el número de consolidado y el nombre del agente.
   - Destinatario configurado como el agente del consolidado.
   - Cuerpo del mensaje que inicia con un saludo y una breve explicación.
   - Una tabla HTML estilizada, donde cada fila corresponde a un recibo no aprobado e incluye columnas para el estado, número, fecha, embarcador, consignatario, piezas, peso, volumen, notas, descripción y estado de aprobación.
   - Un logotipo corporativo al final de la tabla.

5. **Envío del correo electrónico:**  
   Se guarda la actividad de correo en Dynamics y se intenta enviar el correo utilizando el mecanismo estándar de Dynamics. Cualquier error en estas operaciones se almacena para diagnóstico, pero no detiene el proceso completo.

6. **Gestión de errores:**  
   Los errores encontrados durante la creación o el envío del correo se gestionan agregándolos a una lista interna, permitiendo que el plugin continúe su ejecución y no afecte la experiencia del usuario final.

7. **(Opcional, comentado en el código) Notificación al usuario:**  
   Existe un bloque comentado que, si se activa, permitiría crear una notificación tipo toast para el usuario que ejecutó la acción, informando del éxito del envío y permitiendo acceder al registro del correo electrónico recién generado.

## Consideraciones adicionales

- El plugin formatea los datos numéricos como piezas, kilogramos y volumen con dos decimales y muestra valores vacíos si no existen datos.
- El estado "Aprobado" se muestra en la tabla para todos los recibos, aunque realmente solo se están listando aquellos no aprobados.
- El logotipo y los estilos visuales aseguran que el mensaje sea profesional y fácil de leer para el destinatario.
- Toda la lógica de búsqueda, validación, construcción y envío de información está orientada a asegurar la consistencia y automatización de los procesos internos de reportes para agentes.

## Resumen

El plugin dc_era_consolidado facilita y estandariza la comunicación de instrucciones de embarque a los agentes vinculados a un consolidado, presentando la información de manera estructurada y visual, y asegurando que los posibles errores durante el proceso no interrumpan el flujo normal de trabajo en Dynacorp.