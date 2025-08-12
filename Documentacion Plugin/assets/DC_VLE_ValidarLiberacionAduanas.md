**Documentación del Plugin: Validación para Liberación Financiera -
Aduanas**

**🧩 Nombre del Plugin**

**DC_VLE_ValidarLiberacionAduanas_DIF.Proceso**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation)**

**🗂️ Entidad Principal**

- **dc_aduanas**

**🎯 Objetivo**

**Este plugin valida si un registro de Aduanas puede ser liberado por
Finanzas al momento de marcar la opción Facturar (dc_facturar = true).
Evalúa las condiciones de crédito y retrasos de pago del cliente
asociado.**

**🧾 Campos Evaluados**

**En dc_aduanas:**

  ---------------------------------------------------------------------------
  **Campo**                    **Descripción**
  ---------------------------- ----------------------------------------------
  **dc_facturara**             **Referencia al cliente que será facturado**

  **dc_totalventa**            **Monto total de la venta del embarque**

  **dc_facturar**              **Banderilla de ejecución del cálculo al
                               facturar**

  **dc_retenidoporfinanzas**   **Resultado de la validación, true si se
                               retiene**
  ---------------------------------------------------------------------------

**En dc_asociadodenegocios:**

  -----------------------------------------------------------------------------
  **Campo**                           **Descripción**
  ----------------------------------- -----------------------------------------
  **dc_noaplicareteneroperaciones**   **Si es true, no se aplica retención
                                      financiera**

  **dc_terminosdepago**               **Opción de términos: contado (0) o
                                      crédito (1)**

  **dc_limitedecredito**              **Límite de crédito asignado al cliente**

  **dc_creditorestanta**              **Crédito restante disponible del
                                      cliente**
  -----------------------------------------------------------------------------

**En dc_facturas:**

  ------------------------------------------------------------------------
  **Campo**              **Descripción**
  ---------------------- -------------------------------------------------
  **dc_diasderetraso**   **Días de retraso en el pago de la factura**

  ------------------------------------------------------------------------

**🔁 Lógica General del Plugin**

1.  **Profundidad: Solo ejecuta si Depth \<= 1.**

2.  **Condición de Ejecución Principal:\
    Se activa solo si el campo dc_facturar está en true.**

3.  **Validación de Cliente:**

    - **Se recupera la entidad dc_asociadodenegocios a partir de
      dc_facturara.**

    - **Si dc_noaplicareteneroperaciones = true, no se aplica
      validación.**

    - **Si el cliente paga al contado (terminosdepago = 0), se marca
      para retención.**

    - **Si el cliente tiene crédito, se valida que el credito restante ≥
      total de venta.**

    - **Si tiene crédito pero no tiene límite definido, se validan
      facturas activas con días de retraso \> 0.**

4.  **Resultado de la Validación:**

    - **Si falla alguna condición, se marca dc_retenidoporfinanzas =
      true.**

    - **Si todo está correcto, se marca dc_retenidoporfinanzas =
      false.**

**🔐 Reglas de Retención Financiera**

  ------------------------------------------------------------------------------
  **Condición Evaluada**                                         **¿Retener?**
  -------------------------------------------------------------- ---------------
  **Cliente tiene dc_noaplicareteneroperaciones = true**         **❌ No**

  **Cliente tiene terminosdepago = 0 (contado)**                 **✅ Sí**

  **Cliente tiene crédito y dc_creditorestanta \<                **✅ Sí**
  dc_totalventa**                                                

  **Cliente tiene terminosdepago = 1, sin límite definido, y     **✅ Sí**
  tiene facturas activas con dc_diasderetraso \> 0**             
  ------------------------------------------------------------------------------

**✅ Resultado Esperado**

**Al ejecutar el plugin con dc_facturar = true, el sistema debe:**

- **Evaluar el estado financiero del cliente.**

- **Establecer el campo dc_retenidoporfinanzas en true o false según
  corresponda.**

**🧪 Sugerencias de Prueba**

  -------------------------------------------------------------------------
  **Escenario**                                  **Resultado Esperado**
  ---------------------------------------------- --------------------------
  **Cliente con pago al contado y dc_totalventa  **dc_retenidoporfinanzas =
  \> 0**                                         true**

  **Cliente con crédito y crédito restante       **dc_retenidoporfinanzas =
  insuficiente**                                 true**

  **Cliente con crédito y facturas vencidas      **dc_retenidoporfinanzas =
  activas**                                      true**

  **Cliente sin restricción                      **dc_retenidoporfinanzas =
  (dc_noaplicareteneroperaciones = true)**       false**
  -------------------------------------------------------------------------
