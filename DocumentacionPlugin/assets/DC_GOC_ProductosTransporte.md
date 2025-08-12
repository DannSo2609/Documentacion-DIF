**📄 Documentación del Plugin: Creación Automática de Productos OC desde
Productos de Transporte**

------------------------------------------------------------------------

**🧩 Nombre del Plugin**

**DC_GOC_ProductosTransporte.Proceso**

------------------------------------------------------------------------

**⚙️ Evento que lo Dispara**

**Update (Post-Operation)**

------------------------------------------------------------------------

**🗂️ Entidad Principal**

**dc_productostransporte**

------------------------------------------------------------------------

**🎯 Objetivo**

Este plugin tiene como objetivo crear automáticamente productos en
órdenes de compra (dc_ordenesdecompra) a partir de los productos de
transporte (dc_productostransporte). Si ya existe una orden con las
condiciones requeridas, el producto se agrega a dicha orden; si no, se
crea una nueva orden. También actualiza el estado del producto a
procesado (dc_estadodeoc = 1).

------------------------------------------------------------------------

**🧾 Campos Involucrados**

**En la entidad dc_productostransporte:**

  -------------------------------------------------------------------------
  **Campo técnico**  **Descripción**
  ------------------ ------------------------------------------------------
  dc_transporte      Referencia al transporte

  dc_producto        Producto relacionado

  dc_costounitario   Precio unitario del producto

  dc_cantidad        Cantidad del producto

  dc_subtotalcosto   Subtotal estimado del costo

  dc_totalcosto      Total estimado del costo

  dc_monedacompra    Moneda de compra

  dc_estadodeoc      Estado de generación de orden de compra (0 =
                     pendiente, 1 = creado)

  dc_suplidor        Suplidor del producto

  dc_puertoorigen    Puerto de origen

  dc_puertodestino   Puerto de destino
  -------------------------------------------------------------------------

**En la entidad dc_transporte:**

  -----------------------------------------------------------------------
  **Campo técnico**            **Descripción**
  ---------------------------- ------------------------------------------
  dc_tipodeoperacion           Tipo de operación (OptionSet)

  dc_expediente                Expediente relacionado

  dc_consignatario             Cliente

  dc_cotizacion                Cotización relacionada

  dc_mawbmbl                   Consolidado

  dc_hbl                       HBL asociado

  dc_hbldefinitivo             Texto HBL final

  dc_id                        ID del transporte
  -----------------------------------------------------------------------

------------------------------------------------------------------------

**🔁 Lógica del Plugin**

1.  **Validación de Profundidad:**

    - Solo se ejecuta si Depth \<= 1.

2.  **Carga del Producto de Transporte:**

    - Recupera el producto y verifica:

      - dc_costounitario distinto de 0.

      - dc_estadodeoc == 0.

3.  **Verificación de Información Obligatoria:**

    - Se valida que el producto tenga definidos:

      - Producto (dc_producto)

      - Moneda de compra (dc_monedacompra)

      - Suplidor (dc_suplidor)

4.  **Carga del Transporte Asociado:**

    - Se obtiene el transporte asociado y sus campos relacionados.

5.  **Consulta de Ordenes Existentes:**

    - Se busca si ya hay una orden de compra activa que:

      - Tenga el mismo suplidor, expediente, unidad de negocio (3 para
        transporte), moneda, etc.

6.  **Creación o Actualización:**

    - Si **existe** una orden, se **agrega el producto** a ella.

    - Si **no existe**, se **crea una nueva orden** de compra y se
      agrega el producto.

    - En ambos casos:

      - Se actualiza el estado del producto (dc_estadodeoc = 1).

      - Se marca la orden para que recalcule totales (dc_calculartotales
        = true).

------------------------------------------------------------------------

**🧪 Validaciones Críticas**

  -----------------------------------------------------------------------
  **Validación**                                         **Acción**
  ------------------------------------------------------ ----------------
  Producto sin dc_producto                               Lanza excepción

  Producto sin dc_monedacompra                           Lanza excepción

  Producto sin dc_suplidor                               Lanza excepción

  Transporte sin dc_expediente                           Lanza excepción

  Producto con dc_costounitario = 0 o dc_estadodeoc ≠ 0  No se procesa
  -----------------------------------------------------------------------

------------------------------------------------------------------------

**✅ Resultado Esperado**

- Si no hay errores:

  - Se crea o actualiza una orden de compra con el producto de
    transporte.

  - Se marca el producto como procesado (dc_estadodeoc = 1).

  - Se actualiza la orden para recálculo de totales (dc_calculartotales
    = true).
