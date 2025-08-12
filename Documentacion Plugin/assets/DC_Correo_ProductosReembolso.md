**Documentación del Plugin: Envío Automático de Correo a Finanzas para
Productos de Reembolso**

**Nombre del Plugin**

**DC_Correo_ProductosReembolso.Proceso**

**Acción**

- **Update (Post-Operation)**

**Entidad Principal**

- **dc_productosreembolso**

**Descripción General**

**Este plugin se ejecuta automáticamente al actualizar un producto de
reembolso (dc_productosreembolso). Su objetivo es notificar al equipo de
Finanzas mediante correo electrónico cada vez que se procesa un producto
de reembolso, enviando la solicitud de pago con todos los detalles
relevantes del registro. También genera una notificación in-app para el
usuario que disparó la acción, indicando que el proceso fue exitoso. El
flujo cubre la recuperación de la configuración del equipo de Finanzas,
la obtención de los usuarios miembros, la creación y envío de correos
personalizados, y la notificación al usuario.**

**Lógica Interna Principal**

1.  **Prevención de Recursividad**

    - **El plugin solo ejecuta su lógica si la profundidad de contexto
      es 1 (evita loops infinitos).**

2.  **Recuperación de Configuración y Equipo de Finanzas**

    - **Busca el registro de configuración para obtener el equipo de
      Finanzas y la URL base para armar el enlace de detalle.**

3.  **Recuperación de Datos del Producto de Reembolso**

    - **Recupera todos los atributos relevantes del producto de
      reembolso: producto, suplidor, monto, concepto, entidad
      relacionada, expediente, unidad de negocios.**

4.  **Identificación de la Entidad Relacionada**

    - **Determina si el producto está relacionado a Aduanas, Embarques o
      Transporte y obtiene los datos de expediente y unidad de
      negocios.**

5.  **Obtención de Usuarios del Equipo**

    - **Consulta todos los usuarios pertenecientes al equipo de
      Finanzas.**

6.  **Envío de Correos Individuales**

    - **Por cada usuario, crea y envía un correo electrónico con la
      información detallada del producto de reembolso y un enlace
      directo al registro en CRM.**

7.  **Notificación al Usuario**

    - **Crea una notificación in-app informando que los correos se
      enviaron correctamente y ofrece acceso rápido a uno de los correos
      enviados.**

**Validaciones Incluidas**

- **Verifica la existencia de equipo de Finanzas en la configuración.**

- **Valida la existencia de los datos clave de expediente y entidad
  relacionada antes de armar los mensajes.**

- **Si algún dato falta, lo muestra como "No definido" en el cuerpo del
  correo.**

**Entidades Involucradas**

- **dc_productosreembolso (registro principal)**

- **dc_configuracion (contiene el equipo de Finanzas y la URL base)**

- **systemuser (usuarios a notificar)**

- **teammembership (relación usuarios-equipo)**

- **email (actividad de correo electrónico creada y enviada)**

- **appnotification (notificación in-app para el usuario)**

**Métodos Auxiliares Importantes**

- **Consulta de Configuración: Recupera el equipo de Finanzas y la URL
  base desde la entidad de configuración.**

- **Búsqueda de Usuarios de Equipo: Recorre la tabla de membresía de
  equipo para obtener los usuarios destinatarios.**

- **Creación y Envío de Email: Genera una actividad de correo
  personalizada y la envía usando SendEmailRequest.**

- **Notificación In-App: Crea una notificación para el usuario que
  disparó el proceso, con acceso rápido a un registro de correo
  enviado.**

**Manejo de Errores**

- **Utiliza InvalidPluginExecutionException para dar mensajes claros si
  ocurre algún problema de datos.**

- **Registra todos los errores en el servicio de trazas (tracingService)
  para facilitar soporte y debugging.**

- **Lanza el error sin modificar para asegurar visibilidad en CRM.**

**Consideraciones Finales**

- **El plugin automatiza la notificación a Finanzas, asegurando que
  ningún producto de reembolso pase desapercibido y mejorando la
  trazabilidad del proceso de pagos.**

- **Facilita la auditoría y seguimiento de solicitudes de pago, ya que
  toda la información queda registrada y notificada.**

- **Puede ampliarse fácilmente para soportar otras entidades o tipos de
  notificación.**

**Resumen:**

**Este plugin envía automáticamente una solicitud de pago por correo
electrónico al equipo de Finanzas cada vez que se procesa un producto de
reembolso, incluyendo todos los datos clave y un enlace al registro en
CRM. Además, notifica al usuario sobre la acción realizada para completa
visibilidad y control.**
