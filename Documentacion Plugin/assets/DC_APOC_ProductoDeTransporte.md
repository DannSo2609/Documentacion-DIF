# Documentación del Plugin: Proceso de Transporte desde Producto

## Nombre del Plugin

DC_APOC_ProductoDeTransporte.Proceso

## Acción

Update (Post-Operation)

## Entidad Principal

dc_productodetransporte

## Descripción General

Este plugin se ejecuta sobre la entidad dc_productodetransporte y su
propósito es actualizar los productos de orden de compra relacionados
con un registro de transporte. Valida si el producto está vinculado a
una orden de compra no integrada y actualiza los campos correspondientes
o elimina el producto si no cumple con las condiciones.

## Lógica Interna

1\. Verifica si el campo dc_transporte está presente.\
2. Recupera el producto de transporte y sus atributos.\
3. Recupera productos de orden de compra relacionados a la transacción.\
4. Verifica si la orden de compra está integrada.\
5. Si no está integrada, actualiza el producto con los datos del
transporte.\
6. Si el producto no cumple condiciones, lo elimina.\
7. Marca la orden de compra para recalcular totales.

## Validaciones Incluidas

\- Verifica que el campo dc_transporte no sea nulo.\
- Verifica que el costo unitario sea distinto de cero.\
- Verifica que el estado del producto sea activo.\
- Verifica que la orden de compra no esté integrada.

## Entidades Involucradas

\- dc_productodetransporte (origen de datos)\
- dc_productosoc (productos de orden de compra)\
- dc_ordenesdecompra (orden de compra relacionada)

## Métodos Auxiliares

\- ObtenerProductos: Recupera productos de orden de compra relacionados
a la transacción.\
- ActualizarProducto: Actualiza o elimina el producto de orden de compra
según condiciones.\
- FieldDecimalValue: Asigna valores decimales entre entidades.\
- CalcularTotales: Marca la orden de compra para recalcular totales.

## Manejo de Errores

El plugin captura excepciones durante la ejecución y las registra en el
servicio de trazabilidad. Lanza InvalidPluginExecutionException con
mensajes descriptivos cuando el producto está vinculado a una orden de
compra integrada.
