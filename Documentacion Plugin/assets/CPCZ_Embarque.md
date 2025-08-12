**Documentación del Plugin: Copia Automática de Productos de Cotización
a Embarque**

**Nombre del Plugin**

**DC_CPCZ_Embarque.Proceso**

------------------------------------------------------------------------

**Acción**

- **Update (Post-Operation)**

**Entidad Principal**

- **dc_embarques**

------------------------------------------------------------------------

**Descripción General**

Este plugin se ejecuta automáticamente al actualizar un embarque
(dc_embarques). Su objetivo es vincular automáticamente todos los
productos de embarque asociados a la cotización origen (campo
dc_cotizacion) al embarque recién creado o actualizado, estableciendo la
relación entre productos y el embarque correspondiente.

La lógica se encarga de buscar todos los productos de embarque asociados
a la cotización y actualizar el campo de referencia al embarque,
asegurando que todas las líneas de producto estén correctamente
asociadas para su posterior gestión y facturación.

------------------------------------------------------------------------

**Lógica Interna Principal**

1.  **Prevención de Recursividad**

    - El plugin solo ejecuta su lógica si la profundidad de contexto es
      1 (evita loops infinitos).

2.  **Recuperación de Datos Clave**

    - Recupera el embarque actualizado y su referencia a cotización
      (dc_cotizacion).

3.  **Búsqueda de Productos de la Cotización**

    - Si existe una cotización asociada, consulta todos los productos de
      embarque (dc_productosdeembarque) relacionados a esa cotización.

4.  **Actualización de Productos**

    - Para cada producto encontrado, actualiza el campo de embarque
      (dc_embarque) apuntando al embarque actual.

------------------------------------------------------------------------

**Validaciones Incluidas**

- Verifica la existencia de la referencia a cotización antes de intentar
  vincular productos.

- Maneja errores en el proceso y los registra para depuración.

------------------------------------------------------------------------

**Entidades Involucradas**

- **dc_embarques** (embarque principal)

- **dc_cotizacion** (cotización origen)

- **dc_productosdeembarque** (productos a vincular)

------------------------------------------------------------------------

**Métodos Auxiliares Importantes**

- **Consulta de Productos de Embarque**\
  Busca todos los productos de embarque asociados a la cotización.

- **Actualización Masiva**\
  Actualiza cada producto para vincularlo al embarque actual.

------------------------------------------------------------------------

**Manejo de Errores**

- Registra todos los errores en el servicio de trazas (tracingService)
  para facilitar soporte y debugging.

- Lanza el error sin modificar para asegurar visibilidad en CRM.

------------------------------------------------------------------------

**Consideraciones Finales**

- El plugin automatiza la vinculación de productos de embarque desde la
  cotización al embarque, minimizando errores manuales y garantizando
  integridad de datos.

- Simplifica la administración y facturación al asegurar que todos los
  productos estén correctamente asociados al embarque.

------------------------------------------------------------------------

**✅ Resumen**

Este plugin vincula automáticamente todos los productos de embarque
originados en la cotización al embarque correspondiente, asegurando
integridad y trazabilidad en la gestión logística y contable.
