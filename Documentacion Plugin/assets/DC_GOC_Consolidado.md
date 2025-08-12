**Documentación del Plugin: Generación Automática de Órdenes de Compra
por Consolidado**

------------------------------------------------------------------------

**🧩 Nombre del Plugin**

**DC_GOC_Consolidado.Proceso**

------------------------------------------------------------------------

**⚙️ Evento que lo Dispara**

**Update (Post-Operation)**

------------------------------------------------------------------------

**🗂️ Entidad Principal**

**dc_consolidado**

------------------------------------------------------------------------

**🎯 Objetivo**

Este plugin tiene como finalidad generar automáticamente las órdenes de
compra correspondientes a un consolidado, en base a los cargos
consolidados, individuales y depósitos logísticos que estén pendientes
de procesar. Además, crea productos de embarque, productos de orden de
compra y actualiza el estado de los cargos.

------------------------------------------------------------------------

**🧾 Campos Involucrados**

**En la entidad dc_consolidado:**

  ---------------------------------------------------------------------------
  **Campo técnico**              **Descripción**
  ------------------------------ --------------------------------------------
  dc_crearordendecompra          Indicador para activar la creación de
                                 órdenes de compra

  dc_expediente                  Expediente asociado

  dc_consignatario               Cliente

  dc_moneda_cargosconsolidados   Moneda de los cargos consolidados

  dc_moneda_cargosindividuales   Moneda de los cargos individuales

  dc_moneda_cargosdepositos      Moneda de los depósitos logísticos

  dc_agente                      Suplidor para cargos consolidados

  dc_transportista               Suplidor para cargos individuales

  dc_depositologistico           Suplidor para depósitos logísticos

  dc_mawbmbl                     Referencia MAWB o MBL del embarque

  dc_tipodeoperacion             Tipo de operación

  dc_puertoorigen                Puerto de origen

  dc_puertodestino               Puerto de destino
  ---------------------------------------------------------------------------

**En las entidades de cargos:**

  -----------------------------------------------------------------------------
  **Entidad**             **Campo técnico**        **Descripción**
  ----------------------- ------------------------ ----------------------------
  dc_cargosconsolidado    dc_estadoordendecompra   Estado del cargo (procesado
                                                   o no)

  dc_cargosindividuales   dc_estadoordendecompra   Estado del cargo (procesado
                                                   o no)

  dc_productosdepositos   dc_estadoordendecompra   Estado del cargo (procesado
                                                   o no)
  -----------------------------------------------------------------------------

------------------------------------------------------------------------

**🔁 Lógica del Plugin**

1.  **Validación de Profundidad:**

    - El plugin se ejecuta solo si Depth \<= 1 para evitar llamadas
      recursivas.

2.  **Verificación del Indicador:**

    - Solo se ejecuta si dc_crearordendecompra = true.

3.  **Carga del Consolidado y Validaciones Iniciales:**

    - Valida que el consolidado tenga un dc_expediente y un
      dc_consignatario asignado. Si no los tiene, lanza una excepción.

4.  **Obtención de Cargos Pendientes por Procesar:**

    - Se consultan los siguientes cargos asociados al consolidado:

      - dc_cargosconsolidado

      - dc_cargosindividuales

      - dc_productosdepositos

    - Solo se incluyen los que tienen dc_estadoordendecompra = false.

5.  **Generación de la Orden de Compra y Productos:**

    - Para cada tipo de cargo que tenga registros:

      - Crea una orden de compra (dc_ordenesdecompra)

      - Por cada cargo:

        - Crea un producto de embarque (dc_productosdeembarque)

        - Crea un producto de orden de compra (dc_productosoc)

        - Marca el cargo y el producto de embarque como procesados
          (dc_estadoordendecompra = true)

6.  **Recalcular Totales de los Embarques:**

    - Por cada dc_hbl de los cargos procesados, actualiza
      dc_calculartotales = true en la entidad dc_embarques.

------------------------------------------------------------------------

**🧪 Procesos Internos Importantes**

  ---------------------------------------------------------------------------
  **Proceso**              **Descripción**
  ------------------------ --------------------------------------------------
  ObtenerCargos            Recupera los cargos de la entidad correspondiente
                           filtrando por consolidado y estado.

  CrearOrden               Crea la orden de compra, los productos de embarque
                           y productos OC.

  AgregarCargoAlEmbarque   Crea el producto de embarque
                           (dc_productosdeembarque) relacionado al cargo.

  RecalcularEmbarques      Recalcula los totales del embarque vinculado al
                           cargo.

  FieldDecimalValue        Copia valores decimales entre entidades si están
                           presentes.

  UpdateEntity             Método reutilizable para actualizar entidades
                           dinámicamente.
  ---------------------------------------------------------------------------

------------------------------------------------------------------------

**✅ Resultado Esperado**

- Por cada tipo de cargo con registros pendientes:

  - Se crea una orden de compra (dc_ordenesdecompra).

  - Se crean productos de embarque (dc_productosdeembarque) y productos
    OC (dc_productosoc) asociados.

  - Los cargos y productos de embarque se marcan como procesados
    (dc_estadoordendecompra = true).

  - Los embarques asociados se actualizan para recálculo de totales
    (dc_calculartotales = true).
