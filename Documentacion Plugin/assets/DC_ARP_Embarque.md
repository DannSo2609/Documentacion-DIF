# Documentación del Plugin: Proceso de Actualización de Embarque

## Nombre del Plugin

DC_ARP_Embarque.Proceso

## Acción

Update (Post-Operation)

## Entidad Principal

dc_embarques

## Descripción General

Este plugin se ejecuta sobre la entidad dc_embarques y su propósito es
actualizar los valores de peso y volumen en un embarque padre a partir
de los embarques hijos relacionados. Se activa cuando el campo
\'dc_actualizarpesosyvolumenes\' está marcado como verdadero.

## Lógica Interna

1\. Verifica si el campo \'dc_actualizarpesosyvolumenes\' está marcado
como verdadero.\
2. Recupera los embarques hijos relacionados mediante el campo
\'dc_nietos\'.\
3. Suma los valores de peso y volumen de los hijos: toneladas,
kilogramos, libras, metros cúbicos, pies cúbicos, centímetros cúbicos y
cantidad de piezas.\
4. Actualiza el embarque padre con los valores acumulados.\
5. Si no hay embarques hijos, lanza una excepción.

## Validaciones Incluidas

\- Verifica que el campo \'dc_actualizarpesosyvolumenes\' esté
activado.\
- Verifica que existan embarques hijos relacionados.\
- Maneja campos nulos o no presentes con valores predeterminados (0).

## Entidades Involucradas

\- dc_embarques (tanto como entidad principal como hijos relacionados)

## Métodos Auxiliares

\- No se utilizan métodos auxiliares adicionales en este plugin.

## Manejo de Errores

El plugin captura excepciones durante la ejecución y las registra en el
servicio de trazabilidad. Lanza InvalidPluginExecutionException con
mensajes descriptivos cuando no se encuentran embarques hijos.
