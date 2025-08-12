# Documentación del Plugin DC_CPCZ_Aduana

## Sistema: Dynacorp

---

## Descripción General

El plugin `DC_CPCZ_Aduana` automatiza la carga y vinculación de productos de una Cotización a una Aduana en Dynacorp, siempre que la bandera `dc_cargarproductosdelacotizacion` esté activa en el registro de Aduana. Esto permite agilizar y asegurar que los productos de la cotización se reflejen correctamente en el módulo de Aduanas.

---

## Propósito

- Automatizar la transferencia y/o vinculación de productos asociados a una Cotización dentro del registro de una Aduana.
- Evitar duplicidades y asegurar que solo productos no existentes se agreguen o actualicen.
- Facilitar la gestión de productos aduaneros y su trazabilidad en los procesos de importación/exportación.

---

## Funcionamiento Detallado

### 1. Condición de Ejecución

- El plugin solo ejecuta su lógica si la profundidad de ejecución (`Depth`) es 1, evitando ejecuciones recursivas.
- Solo actúa si el campo `dc_cargarproductosdelacotizacion` es verdadero en la Aduana.

### 2. Validación de Cotización

- Recupera el registro actual de Aduana y verifica que tenga asociada una Cotización.
- Si no existe una Cotización, arroja una excepción de validación.

### 3. Recuperación de Productos

- Obtiene todos los productos ya existentes relacionados con la Aduana.
- Obtiene todos los productos asociados a la Cotización.

### 4. Comparación y Selección

- Compara los productos de la Cotización con los productos existentes en la Aduana según combinación de Lista de Precios y Producto.
- Solo los productos que no existen previamente se marcan para ser procesados.

### 5. Procesamiento

- El método principal de procesamiento es `ActualizarProductos`, que vincula los productos seleccionados a la Aduana actual.
  - Puede adaptarse fácilmente para usar `CrearProductos` si se requiere la creación de nuevos registros en vez de actualización.
- Si se agregaron productos, actualiza la bandera `dc_cargarproductosdelacotizacion` a falso para evitar reprocesos.

### 6. Notificaciones

- Existe lógica comentada para enviar notificaciones tipo "appnotification" al usuario, informando sobre la actualización y permitiendo acceso directo al registro actualizado.

### 7. Manejo de Errores

- Todos los errores no críticos durante la ejecución se almacenan en una lista interna, permitiendo el diagnóstico posterior sin detener el proceso principal.

---

## Beneficios

- **Automatización:** Elimina el proceso manual de agregar productos de una cotización a una aduana.
- **Prevención de duplicidades:** Solo se agregan productos que aún no están presentes en la Aduana.
- **Eficiencia:** Facilita la gestión aduanera y comercial, disminuyendo tiempos y errores humanos.
- **Trazabilidad:** Permite rastrear qué productos han sido vinculados y cuándo.

---

## Casos de Uso

- **Carga masiva de productos:** Cuando una nueva Aduana se asocia a una Cotización, los productos son cargados automáticamente.
- **Actualización de productos aduaneros:** Si se actualizan productos en la Cotización, asegura su transferencia eficiente a la Aduana.
- **Gestión de operaciones logísticas:** Agiliza los procesos de importaciones/exportaciones que requieren control aduanero.

---

## Consideraciones

- El plugin depende de la correcta configuración de las entidades de Aduanas, Cotizaciones y Productos en Dynacorp.
- La lógica puede ser adaptada para crear o solo vincular productos, según las necesidades del negocio.
- Es recomendable revisar y personalizar la lógica de notificación según los requerimientos internos de comunicación.

---

## Resumen

El plugin `DC_CPCZ_Aduana` es clave para optimizar la gestión de productos en procesos aduaneros, asegurando que los productos de la Cotización se reflejen correctamente en la Aduana, evitando duplicidades y facilitando la trazabilidad en Dynacorp.