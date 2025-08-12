**📄 Documentación del Plugin: Cálculo de Total a Facturar en Productos
de Reembolso**

**Nombre del Plugin**

**DC_CTF_ProductosReembolso.Proceso**

**Acción**

- **Update (Post-Operation)**

**Entidad Principal**

- **dc_productosreembolso**

**Descripción General**

**Este plugin se ejecuta automáticamente al actualizar un producto de
reembolso. Su función principal es calcular el monto total a facturar al
cliente, realizando una conversión de moneda desde la moneda del
producto hasta la moneda del cliente utilizando las tasas y tipos de
cambio definidos en el sistema.**

**Lógica Interna Principal**

1.  **Prevención de Recursividad**

    - **Ejecuta solo si la profundidad del contexto es igual o menor a
      1.**

2.  **Recuperación y Validación del Producto**

    - **Se valida que el producto tenga:**

      - **Precio unitario**

      - **Cantidad**

      - **Moneda origen y destino**

3.  **Obtención de Tasa del Sistema**

    - **Consulta la entidad dc_tasa y obtiene la más reciente.**

4.  **Cálculo del Tipo de Cambio**

    - **Busca en dc_tipodecambio el registro más reciente que coincida
      con las monedas y tasa.**

5.  **Conversión de Monto**

    - **Multiplica precio unitario \* cantidad \* tipo de cambio para
      obtener el total a facturar.**

6.  **Actualización del Producto**

    - **Se actualiza el campo dc_totalafacturar con el monto
      convertido.**

**Validaciones Incluidas**

- **Valida existencia de moneda origen y destino.**

- **Verifica que precio unitario y cantidad no sean cero o nulos.**

- **Verifica existencia de tasa y tipo de cambio antes de calcular.**

**Entidades Involucradas**

- **dc_productosreembolso (producto actualizado)**

- **dc_tasa (configuración global de tasas)**

- **dc_tipodecambio (registros de conversión entre monedas)**

**Métodos y Cálculos Importantes**

- **ObtenerProducto: Recupera el producto con campos necesarios.**

- **ComprobarProducto: Verifica que los datos mínimos estén presentes.**

- **ObtenerTipoDeCambio: Trae la tasa de cambio entre dos monedas
  activas.**

- **ObtenerMonto: Realiza el cálculo de monto convertido.**

**Manejo de Errores**

- **Lanza excepciones personalizadas si faltan campos clave o
  configuraciones.**

- **Registra todos los errores en tracingService para debugging.**

**Consideraciones Finales**

- **Automatiza el cálculo del total en moneda del cliente desde un
  producto en moneda original.**

- **Evita errores humanos en la conversión de montos para facturación.**

- **Es extensible a múltiples monedas y reglas de negocio adicionales.**

**✅ Resumen**

**El plugin DC_CTF_ProductosReembolso.Proceso automatiza la conversión
de montos desde la moneda del producto hasta la moneda del cliente,
utilizando tasas y tipos de cambio configurados en el sistema,
asegurando precisión y eficiencia en la facturación de productos de
reembolso.**
