# Documentación del Plugin: Proceso de Embarque

## Nombre del Plugin

DC_CE_Embarque.Proceso

## Acción

Update (Post-Operation)

## Entidad Principal

dc_embarques o dc_facturas

## Descripción General

Este plugin se ejecuta sobre las entidades dc_embarques y dc_facturas.
Su propósito es actualizar el estado del embarque en función de los
datos disponibles, como fechas clave, instrucciones enviadas,
facturación y pagos. El flujo de estados refleja el avance del embarque
desde su asignación hasta su finalización.

## Lógica Interna

1\. Determina si el origen del evento es un embarque o una factura.\
2. Recupera el registro de embarque y extrae campos clave.\
3. Si el estado es Cancelado, no realiza ninguna acción.\
4. Si no tiene propietario, lo marca como Activo.\
5. Si tiene propietario, lo marca como Asignado y En Proceso.\
6. Si se enviaron instrucciones, lo marca como Instrucción Enviada.\
7. Si tiene fecha de zarpe (ATD), lo marca como Zarpe Confirmado.\
8. Si tiene fecha de arribo (ATA), lo marca como Carga Arribó a
Destino.\
9. Si tiene fecha de manifiesto, lo marca como Pendiente de Factura.\
10. Si está marcado como facturado, lo marca como Facturado.\
11. Si todas las facturas asociadas están pagadas, lo marca como
Completado.

## Validaciones Incluidas

\- Verifica que el plugin no se ejecute recursivamente.\
- Verifica que el estado no sea Cancelado.\
- Verifica que el embarque tenga propietario.\
- Verifica que las fechas clave no estén vacías.\
- Verifica que las facturas estén marcadas como pagadas (estado 33).

## Entidades Involucradas

\- dc_embarques (registro principal)\
- dc_facturas (relacionadas por dc_hblasociado)

## Métodos Auxiliares

ObtenerFactura(ContextBase context, Guid embarqueId):\
Recupera todas las facturas relacionadas con el embarque.\
\
VerificarFacturas(EntityCollection facturas):\
Verifica si todas las facturas tienen estado 33 (pagadas).\
\
UpdateStatus(ContextBase context, string entity, Guid entityId, int
value):\
Actualiza el campo dc_estadodelregistro con el valor especificado.\
\
UpdateEntity\<T\>(ContextBase context, string entity, Guid entityId,
string field, T value):\
Actualiza un campo específico de una entidad.

## Manejo de Errores

Cualquier excepción durante la ejecución del plugin es capturada y
relanzada como InvalidPluginExecutionException con un mensaje
personalizado para facilitar el diagnóstico.
