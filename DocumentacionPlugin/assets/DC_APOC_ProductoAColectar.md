# Documentación del Plugin: Proceso de Actualización de Producto a Colectar

## Nombre del Plugin

DC_APOC_ProductoAColectar.Proceso

## Acción

Update (Post-Operation)

## Entidad Principal

dc_productoacolectar

## Descripción General

Este plugin se ejecuta sobre la entidad dc_productoacolectar y su
propósito es actualizar los productos asociados a órdenes de compra en
función de su relación con aduanas, embarques o transportes. Valida si
el producto está vinculado a una orden de compra no integrada y
actualiza sus datos, incluyendo campos como costo, cantidad, traducción
y producto relacionado. También recalcula los totales de la orden de
compra.

## Lógica Interna

1\. Recupera el producto a colectar y sus atributos clave.\
2. Verifica si está relacionado con aduana, embarque o transporte.\
3. Por cada relación encontrada, obtiene los productos relacionados.\
4. Verifica si la orden de compra asociada no ha sido integrada.\
5. Si no está integrada, actualiza el producto con los datos del
producto a colectar.\
6. Si está integrada, lanza una excepción para evitar modificaciones.\
7. Recalcula los totales de la orden de compra.

## Validaciones Incluidas

\- Verifica si el producto está relacionado con aduana, embarque o
transporte.\
- Verifica si la orden de compra ha sido integrada en Dynamics.\
- Solo actualiza productos con costo distinto de cero y estado activo.\
- Cambia el estado del producto a inactivo si no cumple condiciones.

## Entidades Involucradas

\- dc_productoacolectar (origen de datos)\
- dc_productosoc (productos de orden de compra)\
- dc_ordenesdecompra (orden de compra relacionada)

## Métodos Auxiliares

\- ObtenerProductos: Recupera productos de orden de compra relacionados
a la transacción.\
- ComprobarProductos: Verifica si la orden de compra está integrada y
actualiza el producto si es necesario.\
- ActualizarProducto: Actualiza los campos del producto de orden de
compra y recalcula totales.\
- FieldDecimalValue: Obtiene o asigna valores decimales entre
entidades.\
- CalcularTotales: Marca la orden de compra para que se recalculen sus
totales.

## Manejo de Errores

El plugin captura excepciones durante la ejecución y las registra en el
servicio de trazabilidad. Lanza InvalidPluginExecutionException con
mensajes descriptivos cuando el producto está vinculado a una orden de
compra ya integrada, evitando así modificaciones no permitidas.
