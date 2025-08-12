# Documentación del Plugin: Actualización de Estado de Despacho

## Nombre del Plugin

DC_CE_Despacho.Proceso

## Acción

Update (Post-Operation)

## Entidad Principal

dc_despacho

## Descripción General

Este plugin se ejecuta sobre la entidad dc_despacho y su propósito es
actualizar el campo dc_estadodelregistro según la información disponible
en el registro. Evalúa condiciones como la existencia de propietario,
conductor y fecha de salida para determinar el estado correspondiente
del despacho.

## Lógica Interna

1\. Recupera el registro de dc_despacho desde el contexto.\
2. Extrae los campos: estado actual, propietario, conductor y fecha real
de salida.\
3. Si el estado es Cancelado (7), no realiza ninguna acción.\
4. Si no hay propietario, actualiza el estado a Pendiente (18).\
5. Si hay propietario, actualiza el estado a En Proceso (10).\
6. Si no hay conductor, termina el flujo.\
7. Si hay conductor, actualiza el estado a Planificado (38).\
8. Si no hay fecha de salida, termina el flujo.\
9. Si hay fecha de salida válida, actualiza el estado a Despachado (27).

## Validaciones Incluidas

\- Verifica que la profundidad de ejecución sea 1.\
- Verifica que el estado no sea Cancelado.\
- Verifica la existencia de propietario, conductor y fecha de salida
antes de actualizar el estado.

## Entidades Involucradas

\- dc_despacho (registro principal que se evalúa y actualiza)

## Método Auxiliar

UpdateStatus(ContextBase context, string entity, Guid entityId, int
value):\
Actualiza el campo dc_estadodelregistro con el valor proporcionado.

## Manejo de Errores

Captura cualquier excepción lanzada durante el proceso y lanza una
InvalidPluginExecutionException con un mensaje personalizado para
facilitar el diagnóstico.
