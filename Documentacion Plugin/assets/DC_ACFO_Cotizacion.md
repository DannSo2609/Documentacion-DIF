# Documentación del Plugin: Proceso de Facturación desde Cotización

## Nombre del Plugin

DC_ACFO_Cotizacion.Proceso

## Acción

Update (Post-Operation)

## Entidad Principal

dc_cotizacion

## Descripción General

Este plugin se ejecuta sobre la entidad dc_cotizacion y su propósito es
generar facturas automáticamente a partir de una cotización aprobada. Se
encarga de validar los datos necesarios, separar productos por tipo y
moneda, y crear o actualizar facturas locales (NCF) e internacionales
(COL). También gestiona la creación de comisiones para los vendedores.

## Lógica Interna

1\. Verifica si el campo dc_facturar está marcado como verdadero.

2\. Recupera la cotización y valida que tenga los campos obligatorios:
Facturar A y Expediente.

3\. Verifica que la cotización esté aprobada.

4\. Recupera configuración del cliente y del sistema.

5\. Recupera productos de ventas y los separa en locales e
internacionales.

6\. Agrupa productos por moneda.

7\. Recupera facturas existentes y actualiza si corresponde.

8\. Crea nuevas facturas si no existen para las combinaciones de moneda
y tipo.

9\. Crea productos de factura y marca productos como facturados.

10\. Calcula totales y crea comisiones para el vendedor.

## Validaciones Incluidas

\- Verifica que la cotización tenga campos obligatorios: Facturar A y
Expediente.

\- Verifica que la cotización esté aprobada.

\- Valida que los productos tengan definidos: Producto, Moneda de Compra
y Suplidor.

\- Evita duplicidad de facturas por moneda y tipo de comprobante.

\- Solo se crean productos de factura si tienen precio unitario válido.

## Entidades Involucradas

\- dc_cotizacion (origen de datos)

\- dc_facturas (facturación generada)

\- dc_productosdeventas (productos relacionados)

\- dc_productosfactura (detalle de productos en factura)

\- dc_asociadodenegocios (cliente)

\- dc_configuracion (configuración del sistema)

\- dc_transaccionescomisiones (comisiones de vendedor)

## Métodos Auxiliares

\- ObtenerDatosCliente: Recupera OptionSet del cliente.

\- ObtenerConfiguracionCol: Recupera configuración del tipo de
comprobante COL.

\- ObtenerFacturas: Recupera facturas relacionadas a cliente y
expediente.

\- ObtenerProductosVentas: Recupera productos de ventas por tipo.

\- SepararPorTipo: Separa productos en locales e internacionales.

\- SepararPorMoneda: Agrupa productos por moneda.

\- ValidarProductos: Verifica que los productos tengan información
mínima.

\- CrearFacturaNCF / CrearFacturaCol: Crea facturas locales e
internacionales.

\- ActualizarFacturaNCF / ActualizarFacturaCol: Agrega productos a
facturas existentes.

\- CrearComisionVendedor / ActualizarComisionVendedor: Crea o actualiza
comisiones.

\- UpdateEntity: Actualiza un campo específico de una entidad.

## Manejo de Errores

El plugin captura excepciones durante la ejecución y las registra en el
servicio de trazabilidad. Lanza InvalidPluginExecutionException con
mensajes descriptivos cuando faltan datos obligatorios o cuando no se
cumplen condiciones necesarias para facturar.
