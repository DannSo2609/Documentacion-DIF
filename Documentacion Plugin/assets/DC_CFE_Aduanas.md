# Nombre del Plugin

DC_CFE_Aduanas.Proceso

# Acción

Update (Post-Operation)

# Entidad Principal

dc_aduanas

# Descripción General

Este plugin se ejecuta cuando se actualiza un registro de la entidad
dc_aduanas y su propósito es generar de manera automática la factura
correspondiente a una transacción, dependiendo del tipo de producto
involucrado: productos locales, internacionales, a colectar o
reembolsos.\
\
La lógica de negocio se basa en examinar los productos relacionados con
la aduana, filtrar los que son válidos (según precio y cantidad), y
generar líneas de factura (dc_productosfactura), además de registrar la
comisión del vendedor correspondiente.

# Lógica Interna

1\. Detección de productos válidos para facturación.

2\. Generación de registros en dc_productosfactura para cada producto
válido.

3\. Actualización del estado de facturación en los productos procesados.

4\. Cálculo de totales de la factura y asignación de la aduana.

5\. Registro o actualización de la comisión del vendedor.

# Tipos de Facturación Soportados

• ActualizarFacturaNCF: Factura productos locales (campo
dc_ventaunitario).

• ActualizarFacturaCol: Factura productos internacionales
(dc_ventaunitario) y productos a colectar (dc_preciounitario).

• ActualizarFacturaReembolso: Factura productos de reembolso
(dc_totalafacturar, cantidad fija = 1).

# Validaciones Incluidas

• Verificación de que al menos un producto tenga un precio unitario
distinto de cero.

• Validación de existencia de campos obligatorios como referencias a
productos, puertos y aduanas.

• Evita actualizaciones innecesarias si no hay productos válidos.

# Entidades Involucradas

• dc_aduanas (origen)

• dc_facturas (factura relacionada)

• dc_productosfactura (líneas de la factura)

• dc_transaccionescomisiones (registro de comisión del vendedor)

# Métodos Auxiliares Importantes

• FieldDecimalValue: Copia valores decimales entre campos de entidades.

• CheckAtLeastOne: Verifica que exista al menos un producto con valor
válido.

• UpdateEntity\<T\>: Actualiza un campo en una entidad específica.

• ObtenerTransaccionesComision: Busca comisiones existentes por vendedor
y transacción.

• CrearComisionVendedor: Registra una nueva comisión con cálculo
proporcional.

• ActualizarComisionVendedor: Lógica para crear o actualizar una
comisión.

• GetEntityReferenceId: Valida la existencia de un EntityReference,
lanza error si no existe.

# Manejo de Errores

• Se controla la búsqueda de comisiones con try-catch para evitar
errores de secuencia.

• Se lanza InvalidPluginExecutionException con mensajes específicos si
faltan campos clave.

# Consideraciones Finales

• El plugin está enfocado a automatizar la facturación según el tipo de
producto procesado.

• Asume que la factura ya existe y está relacionada con la aduana
correspondiente.

• Incluye lógica robusta para crear líneas de factura y registrar
comisión del vendedor.
