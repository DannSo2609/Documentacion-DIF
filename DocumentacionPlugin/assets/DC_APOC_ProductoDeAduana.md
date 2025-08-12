# Documentación del Plugin: Proceso de Productos de Aduana

## Nombre del Plugin

DC_APOC_ProductoDeAduana.Proceso

## Acción

Update (Post-Operation)

## Entidad Principal

dc_productosaduana

## Descripción General

Este plugin se ejecuta sobre la entidad dc_productosaduana y su
propósito es actualizar los productos de orden de compra relacionados
con productos de aduana. Valida si el producto está asociado a una
aduana y si la orden de compra correspondiente no ha sido integrada en
Dynamics. Si es así, actualiza los datos del producto en la orden de
compra o lo elimina si no cumple con las condiciones requeridas.

## Lógica Interna

1\. Recupera el producto de aduana y verifica si está relacionado con
una aduana.\
2. Obtiene los productos de orden de compra relacionados a la
transacción.\
3. Verifica si la orden de compra está integrada en Dynamics.\
4. Si no está integrada, actualiza los datos del producto en la orden de
compra.\
5. Si el producto no cumple con las condiciones (estado inactivo o costo
unitario cero), lo elimina.\
6. Marca la orden de compra para que se recalculen los totales.

## Validaciones Incluidas

\- Verifica que el producto esté relacionado con una aduana.\
- Verifica que la orden de compra no esté integrada en Dynamics.\
- Verifica que el producto tenga un costo unitario distinto de cero y
esté activo.\
- Elimina el producto si no cumple con las condiciones.

## Entidades Involucradas

\- dc_productosaduana (origen de datos)\
- dc_productosoc (productos de orden de compra)\
- dc_ordenesdecompra (orden de compra relacionada)

## Métodos Auxiliares

\- ObtenerProductos: Recupera productos de orden de compra relacionados
a la transacción.\
- ActualizarProducto: Actualiza o elimina el producto de orden de compra
según condiciones.\
- FieldDecimalValue: Obtiene o asigna valores decimales entre
entidades.\
- CalcularTotales: Marca la orden de compra para que se recalculen los
totales.

## Manejo de Errores

El plugin captura excepciones durante la ejecución y las registra en el
servicio de trazabilidad. Lanza InvalidPluginExecutionException con
mensajes descriptivos cuando el producto no puede ser modificado por
estar en una orden de compra ya integrada.
