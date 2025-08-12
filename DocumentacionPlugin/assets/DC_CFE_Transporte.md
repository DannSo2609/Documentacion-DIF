** Documentación del Plugin: Facturación Automática de Transporte**

**Nombre del Plugin**

**DC_CFE_Transporte.Proceso**

------------------------------------------------------------------------

**Acción**

- **Update (Post-Operation)**

**Entidad Principal**

- **dc_transporte**

------------------------------------------------------------------------

**Descripción General**

Este plugin se ejecuta automáticamente al actualizar un registro de
transporte (dc_transporte). Su propósito es automatizar la generación de
facturas correspondientes a cada transporte, garantizando que solo se
facture cuando se cumplen todas las condiciones de negocio y que las
facturas incluyan los productos correctos (locales, internacionales, a
colectar o reembolsos), además de registrar la comisión del vendedor
relacionada. La lógica abarca desde la validación de información mínima,
la recuperación y agrupación de productos, la generación de las
facturas, la actualización de estados y el manejo de comisiones.

------------------------------------------------------------------------

**Lógica Interna Principal**

1.  **Validación de Recursividad y Existencia del Registro**

    - El plugin inicia el proceso solo si el campo dc_facturar está
      marcado como verdadero en el registro de transporte.

2.  **Recuperación y Validación de Datos Clave**

    - Recupera el transporte y valida que tenga cliente, expediente y
      vendedor definidos. Si falta alguno, se detiene el proceso y
      muestra un mensaje claro.

3.  **Carga y Clasificación de Productos**

    - Obtiene los productos asociados al transporte y los clasifica por
      tipo (local, internacional, a colectar, reembolso).

4.  **Procesamiento de Facturación**

    - Recupera facturas existentes y, dependiendo de la moneda y el tipo
      de comprobante, actualiza o crea facturas nuevas.

    - Crea líneas de factura (dc_productosfactura) para cada producto
      válido.

    - Actualiza el estado de facturación de los productos procesados.

5.  **Cálculo y Registro de Comisión del Vendedor**

    - Crea o actualiza el registro de comisión
      en dc_transaccionescomisiones, calculando el valor proporcional
      según el total facturado.

6.  **Gestión de Errores y Validaciones**

    - Lanza mensajes claros si faltan datos obligatorios (cliente,
      expediente, vendedor, producto, moneda).

    - Controla la creación y actualización de comisiones para evitar
      errores de duplicidad.

------------------------------------------------------------------------

**Tipos de Facturación Soportados**

- **Crear/ActualizarFacturaNCF:** Factura productos locales
  (campo dc_ventaunitario).

- **Crear/ActualizarFacturaCol:** Factura productos internacionales
  (dc_ventaunitario) y productos a colectar (dc_preciounitario).

- **Crear/ActualizarFacturaReembolso:** Factura productos de reembolso
  (dc_totalafacturar, cantidad fija = 1).

------------------------------------------------------------------------

**Validaciones Incluidas**

- Verificación de que exista al menos un producto con precio unitario
  distinto de cero para cada tipo relevante.

- Validación de la existencia de campos obligatorios como referencias a
  productos, moneda y datos clave del transporte.

- Lanza excepciones específicas si faltan datos, con mensajes orientados
  al usuario.

- Evita la generación de facturas o actualizaciones innecesarias si no
  hay productos válidos.

------------------------------------------------------------------------

**Entidades Involucradas**

- **dc_transporte** (registro principal)

- **dc_facturas** (factura generada o actualizada)

- **dc_productostransporte**, **dc_productoscolectar**, **dc_productosreembolso** (productos
  a facturar)

- **dc_productosfactura** (líneas de la factura)

- **dc_transaccionescomisiones** (registro de comisión del vendedor)

------------------------------------------------------------------------

**Métodos Auxiliares Importantes**

- **ObtenerDatosCliente:** Recupera campos obligatorios del cliente,
  lanza error si faltan.

- **ObtenerConfiguracionCol:** Recupera el tipo de comprobante COL desde
  la configuración del sistema.

- **ObtenerFacturas:** Busca facturas existentes según cliente,
  expediente y transporte.

- **ValidarProductos:** Valida que cada producto tenga los campos clave
  completos.

- **BuscarProductosTransporte / ObtenerProductosTransporte:** Recuperan
  productos según tipo.

- **SepararPorMoneda:** Agrupa productos por moneda para facturación
  correcta.

- **ObtenerMonedas, ObtenerPorMoneda, ExisteLaMoneda:** Ayudan a
  organizar productos por moneda.

- **ValidarFactura:** Verifica si ya existe una factura válida para una
  combinación de moneda y comprobante.

- **CrearFacturaNCF / CrearFacturaCol / CrearFacturaReembolso:** Crea la
  factura y sus líneas.

- **ActualizarFacturaNCF / ActualizarFacturaCol /
  ActualizarFacturaReembolso:** Añade líneas a facturas existentes.

- **CrearComisionVendedor / ActualizarComisionVendedor:** Registra o
  actualiza la comisión del vendedor según la facturación.

- **FieldDecimalValue, UpdateEntity, CheckAtLeastOne:** Utilidades para
  manipulación de datos y validaciones rápidas.

- **GetEntityReferenceId:** Valida y recupera IDs de referencias, lanza
  error si no existen.

------------------------------------------------------------------------

**Manejo de Errores**

- Utiliza InvalidPluginExecutionException para informar de manera clara
  al usuario sobre datos faltantes o inconsistentes.

- Controla la búsqueda y actualización de comisiones con try-catch para
  evitar errores de secuencia y duplicados.

- Registra todos los errores en el servicio de trazas (tracingService)
  para facilitar el soporte y depuración.

------------------------------------------------------------------------

**Consideraciones Finales**

- El plugin está enfocado en automatizar y robustecer el flujo de
  facturación para transportes, minimizando errores humanos y asegurando
  integridad de datos.

- Permite múltiples escenarios de facturación (local, internacional,
  colectar, reembolso).

- Incluye lógica para la gestión de comisiones asociadas a la venta.

- Provee mensajes claros y acción preventiva ante datos incompletos.

- La estructura modular del código facilita futuras ampliaciones o
  adaptaciones a nuevas reglas de negocio.

------------------------------------------------------------------------

**Resumen:**

Este plugin automatiza la facturación de transportes y el cálculo de
comisiones, asegurando que sólo se facture cuando la información está
completa y agrupando los productos correctamente según el destinatario y
la moneda. Además, garantiza trazabilidad y robustez en el flujo,
generando registros claros para auditoría y gestión comercial.

------------------------------------------------------------------------

Espero que esto te sea de ayuda. Si necesitas más asistencia, no dudes
en decírmelo.
