# Documentación Técnica del Plugin de Validación de Listas de Precios

## 1. Introducción

Este documento describe el funcionamiento técnico del plugin
desarrollado para el proceso de validación de cotizaciones en Microsoft
Dynamics 365. El plugin se activa cuando se aprueba una cotización y
verifica que todos los productos asociados tengan listas de precios
activas.

## 2. Descripción General

El plugin se ejecuta al momento de aprobar una cotización (campo
\'dc_aprobarcotizacion\' marcado como verdadero). Su función principal
es validar que los productos asociados a la cotización (aduana,
embarque, transporte y ventas) tengan listas de precios vigentes.

## 3. Flujo del Proceso

1\. Verificar que la aprobación esté marcada.

2\. Obtener todos los productos asociados a la cotización para cada
tipo.

3\. Obtener la lista de precios asociada a cada producto.

4\. Verificar si la lista de precios está vigente según la fecha actual.

5\. Lanzar una excepción si alguna lista está vencida.

## 4. Entidades Involucradas

• dc_productosaduana

• dc_productosdeembarque

• dc_productostransporte

• dc_productosdeventas

• dc_listasdeprecios

## 5. Validación de Fechas

Cada lista de precios debe tener fechas de validez que abarquen la fecha
actual. Se valida con la condición:\
DateTime.Today \>= dc_fechadesde && DateTime.Today \<= dc_fechahasta\
Si no se cumple, se lanza una InvalidPluginExecutionException deteniendo
el proceso.

## 6. Manejo de Errores

Si ocurre una excepción, se registra el error en el trazado del plugin
(tracingService.Trace) y se lanza un mensaje genérico de error indicando
contactar soporte.

## 7. Consideraciones

• Las listas de precios deben mantenerse actualizadas para permitir la
aprobación de cotizaciones.

• El plugin no realiza actualizaciones, solo validaciones.

• La validación se hace solo cuando la aprobación cambia a verdadero.
