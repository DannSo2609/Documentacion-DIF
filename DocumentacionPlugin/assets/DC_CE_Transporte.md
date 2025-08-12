# Documentación del Plugin: Proceso de Transporte

## Nombre del Plugin

DC_CE_Transporte.Proceso

## Acción

Update (Post-Operation)

## Entidad Principal

dc_transporte

## Descripción General

Este plugin se ejecuta sobre la entidad dc_transporte y su propósito es
actualizar el estado del transporte en función de la información de los
despachos relacionados. Evalúa condiciones como la asignación de
propietario, fechas de salida y recepción, y si el transporte ha sido
facturado. El flujo de estados sigue una lógica secuencial desde Activo
hasta Facturado, pasando por estados intermedios como Asignado, En
Proceso, Despachado, Equipo Retornado y Pendiente de Factura.

## Lógica Interna

1\. Verifica que el plugin no se ejecute recursivamente y que haya una
entidad válida.

2\. Recupera el transporte desde el contexto o desde un despacho
relacionado.

3\. Evalúa el estado actual y si está cancelado, termina el proceso.

4\. Si no tiene propietario, se actualiza el estado a Activo.

5\. Si tiene propietario, se marca como Asignado y se actualiza el campo
dc_asignado.

6\. Recupera los despachos relacionados al transporte.

7\. Si no hay despachos, termina el proceso.

8\. Si hay despachos, se actualiza el estado a En Proceso.

9\. Verifica que todos los despachos tengan fecha de salida para marcar
como Despachado.

10\. Verifica que al menos un despacho tenga fecha de recibido para
marcar como Equipo Retornado.

11\. Verifica que todos los despachos tengan fecha de recibido para
marcar como Pendiente de Factura.

12\. Si el transporte está marcado como facturado, se actualiza el
estado a Facturado.

## Validaciones Incluidas

\- Verifica que el estado no sea Cancelado.

\- Verifica si el transporte tiene propietario.

\- Verifica si existen despachos relacionados.

\- Verifica fechas de salida y recepción en los despachos.

\- Verifica si el transporte está marcado como facturado.

## Entidades Involucradas

\- dc_transporte (entidad principal)

\- dc_despachotransporte (entidad relacionada)

## Métodos Auxiliares

ObtenerTransporte(ContextBase context):

> Recupera el transporte desde el contexto o desde un despacho
> relacionado.

ObtenerDespachos(ContextBase context, Guid transporteId):

> Recupera los despachos asociados a un transporte.

ValidarFechaDespachos(EntityCollection despachos, string campoFecha):

> Verifica que todos los despachos tengan una fecha válida en el campo
> especificado.

ContarDespachosConFecha(EntityCollection despachos, string campoFecha):

> Cuenta cuántos despachos tienen una fecha válida en el campo
> especificado.

ActualizarEstado(ContextBase context, Entity transporte, EstadoRegistro
nuevoEstado):

> Actualiza el estado del transporte.

ActualizarCampo\<T\>(ContextBase context, Entity transporte, string
campo, T valor):

> Actualiza un campo específico del transporte.

## Manejo de Errores

El plugin captura cualquier excepción durante la ejecución y lanza una
InvalidPluginExecutionException con un mensaje personalizado para
facilitar el diagnóstico y la trazabilidad del error.
