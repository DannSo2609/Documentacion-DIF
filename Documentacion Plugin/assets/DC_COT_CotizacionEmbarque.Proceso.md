# 📄 Documentación del Plugin: Creación Automática de Embarque desde Cotización

## Nombre del Plugin

DC_COT_CotizacionEmbarque.Proceso

## Acción

Update (Post-Operation)

## Entidad Principal

dc_cotizacion

## Descripción General

Este plugin se ejecuta automáticamente al actualizar una cotización
(dc_cotizacion). Su objetivo es crear un nuevo registro de embarque y
asociar documentos y productos relacionados, basándose en la información
de la cotización, sólo cuando la cotización es aprobada y se solicita la
creación de órdenes de trabajo de embarque.\
\
La lógica automatiza la creación del embarque, la vinculación de
productos y documentos, y la actualización de las entidades
relacionadas, eliminando procesos manuales y garantizando integridad de
datos.

## Lógica Interna Principal

- Prevención de Recursividad:

El plugin solo ejecuta su lógica si la profundidad de contexto es 1
(evita loops infinitos).

- Validación de Condiciones:

Solo procede si la cotización está aprobada (dc_aprobarcotizacion) y si
se solicita crear órdenes de trabajo de embarque
(dc_crearordenesdetrabajoembarque).

- Creación de Embarque:

Recupera los datos de la cotización y valida la existencia de todos los
campos obligatorios (expediente, puertos, países, agente, embarcador,
incoterms, etc). Crea el registro de embarque (dc_embarques) con los
datos y relaciones requeridas, asignando responsable y tipo de registro
según corresponda.

- Vinculación y Actualización de Entidades Relacionadas:

Actualiza la cotización para vincularla con el nuevo embarque. Actualiza
los documentos y productos de la cotización para asociarlos al embarque
recién creado. Marca el embarque para cálculo de totales
(dc_calculartotales). Si existen referencias a aduana o transporte,
actualiza esas entidades para incluir el embarque como HBL.

- Validaciones de Productos:

Antes de asociar productos, valida que todos tengan moneda de compra y
venta definida. Si falta alguna, detiene el proceso y notifica al
usuario.

- Asignación de Responsable y Dueño:

Si hay ejecutivo de embarque asignado, lo define como dueño del registro
y marca el embarque como asignado. Si no, mantiene el dueño original de
la cotización.

## Validaciones Incluidas

- Verifica aprobación de cotización antes de proceder.

- Asegura existencia de todos los campos obligatorios para la creación
  del embarque.

- Valida que todos los productos tengan moneda de compra y venta
  definida.

- Lanza excepciones claras y orientadas al usuario si falta algún dato.

## Entidades Involucradas

- dc_cotizacion (cotización principal)

- dc_embarques (embarque creado)

- dc_productosdeembarque (productos asociados)

- dc_documentosoperaciones (documentos asociados)

- dc_aduanas, dc_transporte (entidades relacionadas, si aplica)

## Métodos Auxiliares Importantes

- CrearEmbarquePorCotizacion: Crea el registro de embarque a partir de
  la cotización, validando todos los campos requeridos.

- ComprobarProductos: Valida que todos los productos tengan moneda de
  compra y de venta.

- ObtenerProductos / ObtenerDocumentos: Recupera productos y documentos
  operativos asociados a la cotización.

- ActualizarProductos / ActualizarDocumentos: Actualiza los productos y
  documentos para relacionarlos al embarque.

- FieldDecimalValue: Copia valores decimales de una entidad a otra si el
  campo existe.

- UpdateEntity: Método genérico para actualizar cualquier campo en
  cualquier entidad.

## Manejo de Errores

- Utiliza InvalidPluginExecutionException para dar mensajes claros si
  ocurre algún problema de datos o condiciones no cumplidas.

- Registra todos los errores en el servicio de trazas (tracingService)
  para facilitar soporte y debugging.

- Lanza el error sin modificar para asegurar visibilidad en CRM.

## Consideraciones Finales

El plugin automatiza la creación y asociación del embarque y sus
productos/documentos desde una cotización aprobada, minimizando errores
y garantizando cumplimiento de reglas de negocio. Facilita la
trazabilidad y la gestión eficiente entre áreas de ventas y operaciones.
Puede ampliarse fácilmente para soportar nuevos campos o entidades
relacionadas.

## Resumen

Este plugin automatiza la creación de registros de embarque, vinculando
productos y documentos asociados desde una cotización aprobada,
asegurando integridad de datos y eliminando procesos manuales en la
gestión de órdenes de trabajo de embarque.
