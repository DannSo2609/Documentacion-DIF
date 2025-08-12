# Documentación del Plugin: Actualización de Estado de Declaración

## Nombre del Plugin

DC_CE_Declaracion.Proceso

## Acción

Update (Post-Operation)

## Entidad Principal

dc_declaracion

## Descripción General

Este plugin se ejecuta sobre la entidad dc_declaracion y su propósito es
actualizar el estado del registro según una serie de condiciones
relacionadas con el propietario, la aprobación, el número de declaración
y otros campos. El flujo de estados sigue una lógica condicional que
permite establecer el estado correcto del proceso de declaración.

## Lógica Interna

1\. Verifica que la profundidad de ejecución sea 1 y que el Target no
sea nulo.

2\. Recupera el registro de declaración con los campos necesarios.

3\. Evalúa si el estado actual está en la lista de estados sin acción
(por ejemplo, Cancelado).

4\. Si no hay propietario, actualiza el estado a Activo (1).

5\. Si hay propietario, actualiza el estado a En Proceso (10).

6\. Si el campo \'digitado no presentado\' es verdadero, actualiza el
estado a Digitado No Presentado (37).

7\. Si la declaración está aprobada, actualiza el estado a Aprobado (3).

8\. Si el número de declaración no está vacío, actualiza el estado a
Declarado (26).

## Validaciones Incluidas

\- Verifica que el Target no sea nulo.

\- Verifica que la profundidad de ejecución sea 1.

\- Verifica que el estado actual no esté en la lista de estados sin
acción.

\- Verifica si el propietario está asignado.

\- Verifica si los campos booleanos y de texto tienen valores válidos
antes de actualizar el estado.

## Entidades Involucradas

\- dc_declaracion (entidad principal sobre la que se ejecuta el plugin)

## Método Auxiliar

UpdateStatus(ContextBase context, string entity, Guid entityId, int
value):

Actualiza el campo dc_estadodelregistro con el valor proporcionado para
la entidad y el ID especificado.

## Manejo de Errores

El plugin captura cualquier excepción que ocurra durante la ejecución y
lanza una InvalidPluginExecutionException con un mensaje personalizado
para facilitar el diagnóstico y la trazabilidad del error.
