# Documentación del Plugin: Proceso de Consolidado

## Nombre del Plugin

DC_ARP_Consolidado.Proceso

## Acción

Update (Post-Operation)

## Entidad Principal

dc_consolidado

## Descripción General

Este plugin se ejecuta sobre la entidad dc_consolidado y su propósito es
actualizar los campos de peso y volumen en el consolidado padre a partir
de los embarques hijos relacionados. Solo se consideran los embarques
que no tienen nietos asociados.

## Lógica Interna

1\. Verifica si el campo dc_actualizarpesosyvolumenes está marcado como
verdadero.\
2. Recupera todos los embarques relacionados al consolidado padre
(dc_mawbmbl).\
3. Suma los valores de peso, volumen y cantidad de piezas de los
embarques que no tienen nietos.\
4. Actualiza el consolidado padre con los valores acumulados.

## Validaciones Incluidas

\- Verifica que el campo dc_actualizarpesosyvolumenes esté activado.\
- Solo considera embarques sin nietos (dc_nietos == null).\
- Lanza una excepción si no hay embarques relacionados.

## Entidades Involucradas

\- dc_consolidado (registro padre que se actualiza)\
- dc_embarques (registros hijos que contienen los datos de peso y
volumen)

## Métodos Auxiliares

\- No se utilizan métodos auxiliares implementados en este plugin.
Existe un método GetEntityCollection no implementado.

## Manejo de Errores

El plugin captura excepciones durante la ejecución y las registra en el
servicio de trazabilidad. Lanza InvalidPluginExecutionException con
mensajes descriptivos cuando no hay embarques relacionados.
