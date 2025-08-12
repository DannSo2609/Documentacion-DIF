# Documentación del Plugin: Validación de Producto en Factura

## Nombre del Plugin

DC_APF_ProductoDeOperaciones.Proceso

## Acción

Update (Pre-Operation)

## Entidad Principal

dc_productosdeoperacion

## Descripción General

Este plugin se ejecuta sobre la entidad dc_productosdeoperacion y su
propósito es evitar que se modifiquen productos que ya están asociados a
una factura integrada. Valida si el producto pertenece a una factura y,
en caso afirmativo, lanza una excepción para impedir su modificación.

## Lógica Interna

1\. Verifica que la profundidad de ejecución sea 1 y que exista una
entidad objetivo.

2\. Recupera el registro del producto de operación y obtiene su
identificador.

3\. Llama al método ObtenerProductos para recuperar productos
relacionados a la transacción.

4\. Si se encuentra un producto relacionado con una factura, lanza una
excepción para evitar la modificación.

## Validaciones Incluidas

\- Verifica que la ejecución no sea recursiva (Depth \> 1).

\- Verifica que el producto esté relacionado a una factura antes de
permitir su modificación.

## Entidades Involucradas

\- dc_productosdeoperacion (producto de operación)

\- dc_productosfactura (detalle de productos en factura)

\- dc_factura (factura asociada)

## Métodos Auxiliares

\- ObtenerProductos: Recupera productos de factura relacionados a una
transacción específica.

## Manejo de Errores

El plugin captura excepciones durante la ejecución y las registra en el
servicio de trazabilidad. Lanza InvalidPluginExecutionException con un
mensaje descriptivo si el producto ya está asociado a una factura
integrada.
