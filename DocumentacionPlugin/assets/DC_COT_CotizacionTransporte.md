# 📄 Documentación del Plugin: Creación Automática de Transporte desde Cotización

## Nombre del Plugin

DC_COT_CotizacionTransporte.Proceso

## Acción

Update (Post-Operation)

## Entidad Principal

dc_cotizacion

## Descripción General

Este plugin se ejecuta automáticamente al actualizar una cotización
(dc_cotizacion). Su objetivo es crear un nuevo registro de transporte y
asociar documentos y productos relacionados, basándose en la información
de la cotización, sólo cuando la cotización es aprobada y se solicita la
creación de órdenes de trabajo de transporte.\
\
La lógica automatiza la creación del transporte, la vinculación de
productos y documentos, y la actualización de las entidades
relacionadas, eliminando procesos manuales y garantizando integridad de
datos.

## Lógica Interna Principal

1\. Prevención de Recursividad: El plugin solo ejecuta su lógica si la
profundidad de contexto es 1 (evita loops infinitos).

2\. Validación de Condiciones: Solo procede si la cotización está
aprobada (dc_aprobarcotizacion) y si se solicita crear órdenes de
trabajo de transporte (dc_crearordenesdetrabajotransporte).

3\. Creación de Transporte: Recupera los datos de la cotización y valida
la existencia de todos los campos obligatorios. Crea el registro de
transporte (dc_transporte) con los datos y relaciones requeridas.

4\. Vinculación y Actualización de Entidades Relacionadas: Actualiza la
cotización, documentos y productos para asociarlos al transporte. Marca
el transporte para cálculo de totales. Actualiza embarque o aduana si
aplica.

5\. Validaciones de Productos: Valida que todos los productos tengan
moneda de compra y venta definida.

6\. Asignación de Responsable y Dueño: Define el ejecutivo de transporte
como dueño si está asignado, y marca el transporte como asignado.

## Validaciones Incluidas

\- Verifica aprobación de cotización antes de proceder.

\- Asegura existencia de todos los campos obligatorios para la creación
del transporte.

\- Valida que todos los productos tengan moneda de compra y venta
definida.

\- Lanza excepciones claras y orientadas al usuario si falta algún dato.

## Entidades Involucradas

\- dc_cotizacion (cotización principal)

\- dc_transporte (transporte creado)

\- dc_productostransporte (productos asociados)

\- dc_documentosoperaciones (documentos asociados)

\- dc_embarques, dc_aduanas (entidades relacionadas, si aplica)

## Métodos Auxiliares Importantes

\- CrearTransportePorCotizacion: Crea el registro de transporte a partir
de la cotización, validando todos los campos requeridos.

\- ComprobarProductos: Valida que todos los productos tengan moneda de
compra y de venta.

\- ObtenerProductos / ObtenerDocumentos: Recupera productos y documentos
operativos asociados a la cotización.

\- ActualizarProductos / ActualizarDocumentos: Actualiza los productos y
documentos para relacionarlos al transporte.

\- FieldDecimalValue: Copia valores decimales de una entidad a otra si
el campo existe.

\- UpdateEntity: Método genérico para actualizar cualquier campo en
cualquier entidad.

## Manejo de Errores

\- Utiliza InvalidPluginExecutionException para dar mensajes claros si
ocurre algún problema de datos o condiciones no cumplidas.

\- Registra todos los errores en el servicio de trazas (tracingService)
para facilitar soporte y debugging.

\- Lanza el error sin modificar para asegurar visibilidad en CRM.

## Consideraciones Finales

El plugin automatiza la creación y asociación del transporte y sus
productos/documentos desde una cotización aprobada, minimizando errores
y garantizando cumplimiento de reglas de negocio.\
\
Facilita la trazabilidad y la gestión eficiente entre áreas de ventas y
operaciones.\
\
Puede ampliarse fácilmente para soportar nuevos campos o entidades
relacionadas.

## Resumen

Este plugin automatiza la creación de registros de transporte,
vinculando productos y documentos asociados desde una cotización
aprobada, asegurando integridad de datos y eliminando procesos manuales
en la gestión de órdenes de trabajo de transporte.
