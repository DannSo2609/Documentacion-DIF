# Documentación del Plugin: Proceso de Embarque

## Nombre del Plugin

DC_APOC_ProductoDeEmbarque.Proceso

## Acción

Update (Post-Operation)

## Entidad Principal

dc_productodeembarque

## Descripción General

Este plugin se ejecuta sobre la entidad dc_productodeembarque y su
propósito es actualizar los productos de orden de compra relacionados
con un embarque. Valida si el producto está vinculado a una orden de
compra no integrada y actualiza sus campos con la información del
producto de embarque. Si el producto no cumple con las condiciones, se
elimina. Finalmente, marca la orden de compra para que se recalculen sus
totales.

## Lógica Interna

1\. Recupera el producto de embarque y valida que esté relacionado con
un embarque.\
2. Recupera los productos de orden de compra relacionados a la
transacción.\
3. Verifica si la orden de compra está integrada en Dynamics.\
4. Si no está integrada, actualiza los campos del producto de orden de
compra.\
5. Si el producto no cumple condiciones (estado inactivo o costo cero),
lo elimina.\
6. Marca la orden de compra para que se recalculen los totales.

## Validaciones Incluidas

\- Verifica que el producto esté relacionado con un embarque.\
- Verifica que la orden de compra no esté integrada.\
- Solo actualiza productos con costo unitario distinto de cero y estado
activo.\
- Elimina productos que no cumplen condiciones.

## Entidades Involucradas

\- dc_productodeembarque (origen de datos)\
- dc_productosoc (productos de orden de compra)\
- dc_ordenesdecompra (orden de compra relacionada)

## Métodos Auxiliares

\- ObtenerProductos: Recupera productos de orden de compra relacionados
a la transacción.\
- ActualizarProducto: Actualiza o elimina el producto de orden de compra
según condiciones.\
- FieldDecimalValue: Asigna valores decimales entre entidades.\
- CalcularTotales: Marca la orden de compra para que se recalculen los
totales.

## Manejo de Errores

El plugin captura excepciones durante la ejecución y las registra en el
servicio de trazabilidad. Lanza InvalidPluginExecutionException con
mensajes descriptivos cuando el producto está vinculado a una orden de
compra integrada.
