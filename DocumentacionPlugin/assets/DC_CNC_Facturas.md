** Documentación del Plugin: Creación Automática de Nota de Crédito
desde Factura**

**Nombre del Plugin**

**DC_CNC_Facturas.Proceso**

**Acción**

- **Update (Post-Operation)**

**Entidad Principal**

- **dc_facturas**

**Descripción General**

**Este plugin se ejecuta automáticamente al actualizar una factura
(dc_facturas). Su objetivo es crear de forma automática una Nota de
Crédito (NC) y sus líneas de productos asociadas, copiando la
información relevante de la factura original, cuando el usuario marca el
campo dc_crearnotadecredito como verdadero. Finalmente, la factura se
actualiza para quedar referenciada con la nueva NC. Este proceso elimina
errores de captura manual y facilita la trazabilidad y auditoría
financiera.**

**Lógica Interna Principal**

1.  **Prevención de Recursividad**

    - **El plugin solo ejecuta su lógica si la profundidad de contexto
      es 1 (evita loops infinitos).**

2.  **Verificación de Condición para Crear Nota de Crédito**

    - **Solo procede si el campo booleano dc_crearnotadecredito está
      marcado como verdadero en la factura.**

3.  **Recuperación de Datos de Factura**

    - **Recupera los datos relevantes de la factura: cliente,
      expediente, HBL, tipo de comprobante, operación, ingreso, moneda,
      etc.**

4.  **Creación de Entidad Nota de Crédito**

    - **Crea una nueva entidad dc_notasdecredito y copia los campos
      necesarios desde la factura.**

    - **Si el tipo de comprobante es "11" (caso especial), lo asigna
      especialmente.**

5.  **Copia de Productos de Factura a Nota de Crédito**

    - **Recupera todos los productos ligados a la factura
      (entidad dc_productosfactura).**

    - **Por cada producto, crea un nuevo registro en dc_productosnc,
      copiando los campos clave y asociándolo a la nueva nota de
      crédito.**

6.  **Actualización de Factura**

    - **Actualiza el campo dc_notadecredito de la factura original para
      dejarla ligada a la nueva nota de crédito.**

**Validaciones Incluidas**

- **El plugin solo se ejecuta si el campo dc_crearnotadecredito es
  verdadero.**

- **Valida la existencia de datos clave antes de copiar.**

- **Para cada producto, valida existencia de valores antes de asignarlos
  (evita nulos).**

**Entidades Involucradas**

- **dc_facturas (factura principal)**

- **dc_notasdecredito (nota de crédito creada)**

- **dc_productosfactura (productos originales de la factura)**

- **dc_productosnc (productos copiados a la nota de crédito)**

**Métodos Auxiliares Importantes**

- **Copia de Campos: Copia uno a uno los valores de los productos de
  factura a los productos de NC, validando presencia de datos.**

- **Actualización de Factura: Actualiza el campo de referencia a la nota
  de crédito después de crearla.**

**Manejo de Errores**

- **Utiliza InvalidPluginExecutionException para dar mensajes claros si
  ocurre algún problema de datos.**

- **Registra todos los errores en el servicio de trazas (tracingService)
  para facilitar soporte y debugging.**

- **Lanza el error sin modificar para asegurar visibilidad en CRM.**

**Consideraciones Finales**

- **El plugin automatiza el proceso de creación de notas de crédito,
  asegurando coherencia en la información y minimizando el riesgo de
  errores humanos.**

- **Facilita la auditoría, ya que deja todas las referencias y
  relaciones correctamente establecidas.**

- **Puede adaptarse fácilmente si se agregan nuevos campos a las
  entidades involucradas.**

**Resumen:**

**Este plugin permite la creación automática y confiable de notas de
crédito a partir de una factura, copiando los productos y dejando toda
la información correctamente relacionada para fines comerciales y
contables.**

**Espero que esto te sea de ayuda. Si necesitas más asistencia, no dudes
en decírmelo.**
