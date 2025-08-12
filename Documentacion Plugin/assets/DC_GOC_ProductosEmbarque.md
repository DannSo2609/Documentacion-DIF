**Documentación del Plugin: Creación Automática de Productos OC desde
Productos de Embarque**

**🧩 Nombre del Plugin**

**DC_GOC_ProductosEmbarque.Proceso**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation)**

**🗂️ Entidad Principal**

**dc_productosdeembarque**

**🎯 Objetivo**

**Este plugin tiene como objetivo crear automáticamente productos en
órdenes de compra (dc_ordenesdecompra) basados en los productos de
embarque (dc_productosdeembarque). Si ya existe una orden con las
condiciones necesarias, el producto se agrega a la misma; de lo
contrario, se crea una nueva orden. Además, actualiza el estado del
producto para marcarlo como procesado (dc_estadosdeoc = 1).**

**🧾 Campos Involucrados**

**En la entidad dc_productosdeembarque:**

  ------------------------------------------------------------------------------
  **Campo técnico**         **Descripción**
  ------------------------- ----------------------------------------------------
  **dc_embarque**           **Referencia al registro de embarque**

  **dc_producto**           **Producto relacionado**

  **dc_costounitario**      **Precio unitario del producto**

  **dc_cantidad**           **Cantidad del producto**

  **dc_subtotalcosto**      **Subtotal estimado del costo**

  **dc_totalcosto**         **Total estimado del costo**

  **dc_monedacompra**       **Moneda de compra**

  **dc_estadosdeoc**        **Estado de generación de orden de compra (0 =
                            pendiente, 1 = creado)**

  **dc_suplidor**           **Suplidor del producto**

  **dc_puertoorigen**       **Puerto de origen**

  **dc_puertodestino**      **Puerto de destino**

  **dc_cargoconsolidado**   **Indica si el producto está consolidado**
  ------------------------------------------------------------------------------

**En la entidad dc_embarques:**

  -----------------------------------------------------------------------
  **Campo técnico**            **Descripción**
  ---------------------------- ------------------------------------------
  **dc_tipodeoperacion**       **Tipo de operación (OptionSet)**

  **dc_expediente**            **Expediente relacionado**

  **dc_consignatario**         **Cliente**

  **dc_cotizacion**            **Cotización relacionada**

  **dc_mawbmbl**               **Consolidado**

  **dc_hbl**                   **HBL asociado**
  -----------------------------------------------------------------------

**🔁 Lógica del Plugin**

1.  **Validación de Profundidad:**

    - **El plugin solo se ejecuta si Depth \<= 1.**

2.  **Carga del Registro de Producto:**

    - **Recupera el producto de embarque (dc_productosdeembarque) y
      valida que:**

      - **dc_costounitario sea distinto de 0.**

      - **dc_estadosdeoc = 0.**

      - **No tenga dc_cargoconsolidado.**

3.  **Verificación de Información Requerida:**

    - **Se valida que el producto tenga:**

      - **Producto definido (dc_producto)**

      - **Moneda de compra (dc_monedacompra)**

      - **Suplidor definido**

4.  **Obtención del Embarque Asociado:**

    - **Carga el registro de dc_embarques asociado al producto.**

5.  **Consulta de Ordenes Existentes:**

    - **Busca si existe alguna orden de compra activa y no integrada a
      Dynamics que cumpla:**

      - **Suplidor, expediente, moneda, unidad de negocios = 1
        (embarques), etc.**

6.  **Creación o Actualización:**

    - **Si existe una orden de compra, se agrega el producto a la
      misma.**

    - **Si no existe, se crea una nueva orden y se agrega el producto.**

    - **En ambos casos:**

      - **Se marca el producto como procesado (dc_estadosdeoc = 1)**

      - **Se marca la orden para recálculo de totales
        (dc_calculartotales = true)**

**🧪 Validaciones Críticas**

  -----------------------------------------------------------------------
  **Validación**                                            **Acción**
  --------------------------------------------------------- -------------
  **Producto sin dc_producto**                              **Lanza
                                                            excepción**

  **Producto sin dc_monedacompra**                          **Lanza
                                                            excepción**

  **Producto sin dc_suplidor**                              **Lanza
                                                            excepción**

  **Embarque sin dc_expediente**                            **Lanza
                                                            excepción**

  **Producto con dc_costounitario = 0, dc_estadosdeoc ≠ 0 o **No se
  tiene dc_cargoconsolidado**                               procesa**
  -----------------------------------------------------------------------

**✅ Resultado Esperado**

- **Si no hay errores:**

  - **Se crea o actualiza una orden de compra para el producto de
    embarque.**

  - **Se marca el producto como procesado (dc_estadosdeoc = 1).**

  - **Se solicita el recálculo de totales para la orden
    (dc_calculartotales = true).**
