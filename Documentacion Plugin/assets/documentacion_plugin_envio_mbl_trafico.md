# Documentación del Plugin: Envío de MBL a Tráfico

## Nombre del Plugin

DC_ADP_Consolidado.Proceso

## Acción

Update (Post-Operation)

## Entidad Principal

dc_consolidado

## Descripción General

Este plugin se ejecuta sobre la entidad dc_consolidado y su propósito es
validar que los embarques relacionados puedan ser enviados a tráfico.
Para ello, verifica que cada embarque tenga una fecha ATD válida y que
esté facturado o marcado como permitido para tráfico. Si todas las
validaciones se cumplen, el consolidado es reasignado al equipo de
tráfico definido en la configuración del sistema.

## Lógica Interna

1\. Recupera el registro de dc_consolidado desde el contexto.

2\. Verifica si el campo dc_enviaratrafico está marcado como verdadero.

3\. Recupera todos los embarques relacionados mediante el campo
dc_mawbmbl.

4\. Para cada embarque, valida que tenga fecha ATD y que esté facturado
o permitido para tráfico.

5\. Si todas las validaciones se cumplen, actualiza el propietario del
consolidado al equipo de tráfico.

## Validaciones Incluidas

\- Verifica que cada embarque tenga una fecha ATD válida.

\- Verifica que cada embarque esté facturado o marcado como permitido
para tráfico.

\- Lanza excepciones con mensajes específicos si alguna validación
falla.

## Entidades Involucradas

\- dc_consolidado (origen del proceso)

\- dc_embarques (validación de condiciones)

\- dc_configuracion (para obtener el equipo de tráfico)

## Métodos Auxiliares

ObtenerEquipoDeTrafico(ContextBase context):

- \- Recupera el equipo de tráfico desde la entidad dc_configuracion.

ObtenerEmbarques(ContextBase context, Guid Consolidado):

- \- Recupera los embarques activos relacionados con el consolidado
  mediante el campo dc_mawbmbl.

## Manejo de Errores

El plugin captura cualquier excepción durante el proceso y la registra
en el trazador. Si ocurre un error, se lanza una excepción para que el
sistema la maneje adecuadamente.
