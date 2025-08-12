**Documentación del Plugin: Creación Automática de Productos OC desde
Productos de Aduana**

**🧩 Nombre del Plugin**

**DC_GOC_ProductosAduana.Proceso**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation)**

**🗂️ Entidad Principal**

**dc_productosaduana**

**🎯 Objetivo**

**Este plugin tiene como objetivo registrar automáticamente productos en
órdenes de compra (dc_ordenesdecompra) basados en los productos de
aduana (dc_productosaduana). Si no existe una orden de compra abierta
con las condiciones requeridas, se crea una nueva. Además, se actualiza
el estado del producto para marcarlo como procesado (dc_estadodeoc =
1).**

**🧾 Campos Involucrados**

**En la entidad dc_productosaduana:**

  -----------------------------------------------------------------------------
  **Campo técnico**      **Descripción**
  ---------------------- ------------------------------------------------------
  **dc_aduana**          **Referencia al registro de aduana**

  **dc_producto**        **Producto relacionado**

  **dc_costounitario**   **Precio unitario del producto**

  **dc_cantidad**        **Cantidad del producto**

  **dc_subtotalcosto**   **Subtotal estimado del costo**

  **dc_totalcosto**      **Total estimado del costo**

  **dc_monedacompra**    **Moneda de compra**

  **dc_estadodeoc**      **Estado de generación de orden de compra (0 =
                         pendiente, 1 = creado)**

  **dc_suplidor**        **Suplidor del producto**

  **dc_puertoorigen**    **Puerto de origen**

  **dc_puertodestino**   **Puerto de destino**
  -----------------------------------------------------------------------------

**En la entidad dc_aduanas:**

  -----------------------------------------------------------------------
  **Campo técnico**            **Descripción**
  ---------------------------- ------------------------------------------
  **dc_tipodeoperacion**       **Tipo de operación (OptionSet)**

  **dc_expediente**            **Expediente relacionado**

  **dc_consignatario**         **Cliente**

  **dc_cotizacion**            **Cotización relacionada**

  **dc_mawbmbl**               **Consolidado**

  **dc_hbl**                   **HBL asociado**

  **dc_hbldefinitivo**         **Código definitivo del HBL**
  -----------------------------------------------------------------------

**🔁 Lógica del Plugin**

1.  **Validación de Profundidad:**

    - **El plugin solo se ejecuta si Depth \<= 1.**

2.  **Carga del Registro de Producto:**

    - **Recupera el producto de aduana (dc_productosaduana) y valida
      que:**

      - **dc_costounitario sea distinto de 0.**

      - **dc_estadodeoc = 0.**

3.  **Verificación de Información Requerida:**

    - **Se valida que el producto tenga:**

      - **Producto relacionado (dc_producto)**

      - **Moneda de compra (dc_monedacompra)**

      - **Suplidor**

4.  **Obtención de la Aduana Asociada:**

    - **Carga el registro de dc_aduanas asociado al producto.**

5.  **Consulta de Ordenes Existentes:**

    - **Busca si existe alguna orden de compra activa y no integrada a
      Dynamics que cumpla:**

      - **Suplidor, expediente, moneda, unidad de negocios = 2
        (aduanas), etc.**

6.  **Creación o Actualización:**

    - **Si existe una orden de compra, se agrega el producto a la
      misma.**

    - **Si no existe, se crea una nueva orden y se agrega el producto.**

    - **En ambos casos:**

      - **Se marca el producto como procesado (dc_estadodeoc = 1)**

      - **Se marca la orden para recálculo de totales
        (dc_calculartotales = true)**

**🧪 Validaciones Críticas**

  -----------------------------------------------------------------------
  **Validación**                                   **Acción**
  ------------------------------------------------ ----------------------
  **Producto sin dc_producto**                     **Lanza excepción**

  **Producto sin dc_monedacompra**                 **Lanza excepción**

  **Aduana sin dc_expediente**                     **Lanza excepción**

  **Producto con dc_costounitario = 0 o            **No se procesa**
  dc_estadodeoc ≠ 0**                              

  **Suplidor nulo o sin nombre**                   **Se omite
                                                   procesamiento**
  -----------------------------------------------------------------------

**✅ Resultado Esperado**

- **Si no hay errores:**

  - **Se crea o actualiza una orden de compra para el producto de
    aduana.**

  - **Se marca el producto como procesado (dc_estadodeoc = 1).**

  - **Se solicita el recálculo de totales para la orden
    (dc_calculartotales = true).**
