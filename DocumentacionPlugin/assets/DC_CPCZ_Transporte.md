# Documentación del Plugin DC_CPCZ_Transporte

## Sistema: Dynacorp

---

## Descripción General

El plugin `DC_CPCZ_Transporte` automatiza la carga y vinculación de productos de una Cotización a un Transporte en Dynacorp, siempre que la bandera `dc_cargarproductosdelacotizacion` esté activa en el registro de Transporte. Su función es agilizar la transferencia de productos entre la cotización y el transporte, asegurando la consistencia de la información y evitando duplicidades.

---

## Propósito

- Automatizar la transferencia y/o vinculación de productos de una Cotización al registro de Transporte correspondiente.
- Prevenir la duplicación de productos, asegurando que solo los productos no existentes se agreguen o se actualicen.
- Facilitar la gestión y trazabilidad de productos transportados en los procesos logísticos y comerciales.

---

## Funcionamiento Detallado

### 1. Condición de Ejecución

- El plugin ejecuta su lógica únicamente si la profundidad de ejecución (`Depth`) es 1, para evitar ejecuciones recursivas.
- Solo actúa si el campo `dc_cargarproductosdelacotizacion` es verdadero en el Transporte.

### 2. Validación de la Cotización

- Recupera el registro actual de Transporte y verifica que tenga una Cotización asociada.
- Si no existe una Cotización asociada, arroja una excepción de validación.

### 3. Recuperación de Productos

- Obtiene todos los productos ya existentes relacionados con el Transporte.
- Obtiene todos los productos asociados a la Cotización.

### 4. Comparación y Selección

- Compara los productos de la Cotización con los productos existentes en el Transporte utilizando la combinación de Lista de Precios y Producto.
- Solo los productos que no están previamente relacionados con el Transporte se marcan para ser procesados.

### 5. Procesamiento

- El método principal de procesamiento es `ActualizarProductos`, que vincula los productos seleccionados al Transporte actual.
  - Puede adaptarse fácilmente para usar `CrearProductos` si se requiere la creación de nuevos registros en vez de actualización.
- Si se han vinculado productos, actualiza la bandera `dc_cargarproductosdelacotizacion` a falso para evitar reprocesos innecesarios.

### 6. Notificaciones

- Existe lógica comentada para enviar notificaciones tipo appnotification al usuario, informando sobre la actualización y permitiendo acceso directo al registro actualizado.

### 7. Manejo de Errores

- Todos los errores no críticos durante la ejecución se almacenan en una lista interna, permitiendo el diagnóstico posterior sin detener el proceso principal.

---

## Beneficios

- **Automatización:** Elimina el proceso manual de agregar productos de una cotización a un transporte.
- **Prevención de duplicidades:** Solo se agregan productos que aún no están presentes en el Transporte.
- **Eficiencia:** Facilita la gestión logística y comercial, disminuyendo tiempos y errores humanos.
- **Trazabilidad:** Permite rastrear qué productos han sido vinculados y cuándo.

---

## Casos de Uso

- **Carga masiva de productos:** Cuando un nuevo Transporte se asocia a una Cotización, los productos son vinculados automáticamente.
- **Actualización de productos transportados:** Si se actualizan productos en la Cotización, asegura su transferencia eficiente al Transporte.
- **Gestión de operaciones logísticas:** Agiliza el proceso de preparación y despacho de transportes, asegurando que los productos estén correctamente relacionados.

---

## Consideraciones

- El plugin depende de la correcta configuración de las entidades de Transporte, Cotizaciones y Productos en Dynacorp.
- La lógica puede ser adaptada para crear o solo vincular productos, según las necesidades del negocio.
- Es recomendable revisar y personalizar la lógica de notificación según los requerimientos internos de comunicación.

---

## Resumen

El plugin `DC_CPCZ_Transporte` es fundamental para optimizar la gestión de productos en procesos logísticos, asegurando que los productos de la Cotización se reflejen correctamente en el Transporte, evitando duplicidades y facilitando la trazabilidad en Dynacorp.