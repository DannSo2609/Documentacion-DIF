**Documentación del Plugin: Validación para Liberación Financiera -
Transporte**

**🧩 Nombre del Plugin**

**DC_VLE_ValidarLiberacionTransporte_DIF.Proceso**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation)**

**🗂️ Entidad Principal**

- **dc_transporte**

**🎯 Objetivo**

**Este plugin valida si un registro de Transporte puede ser liberado por
Finanzas al momento de marcar la opción Facturar (dc_facturar = true).
Evalúa las condiciones de crédito y pagos vencidos del cliente.**

**🧾 Campos Evaluados**

**En dc_transporte:**

  ------------------------------------------------------------------------
  **Campo**                    **Descripción**
  ---------------------------- -------------------------------------------
  **dc_facturara**             **Cliente a quien se le facturará**

  **dc_totalventa**            **Monto total de la venta**

  **dc_facturar**              **Indica si debe ejecutarse la lógica**

  **dc_retenidoporfinanzas**   **Resultado de la evaluación financiera**
  ------------------------------------------------------------------------

**En dc_asociadodenegocios (cliente):**

  -----------------------------------------------------------------------------
  **Campo**                           **Descripción**
  ----------------------------------- -----------------------------------------
  **dc_noaplicareteneroperaciones**   **Si es true, se excluye de evaluación
                                      financiera**

  **dc_terminosdepago**               **0 = Contado, 1 = Crédito**

  **dc_limitedecredito**              **Límite de crédito asignado al cliente**

  **dc_creditorestanta**              **Crédito restante disponible del
                                      cliente**
  -----------------------------------------------------------------------------

**En dc_facturas:**

  -----------------------------------------------------------------------
  **Campo**               **Descripción**
  ----------------------- -----------------------------------------------
  **dc_diasderetraso**    **Número de días de retraso de pago**

  **statecode**           **Estado de la factura (0 = activa)**
  -----------------------------------------------------------------------

**🔁 Lógica General del Plugin**

1.  **Control de profundidad:\
    El plugin solo ejecuta si context.Context.Depth \<= 1.**

2.  **Ejecución por bandera:\
    Solo ejecuta si dc_facturar = true.**

3.  **Lógica de retención financiera:**

    - **Si el cliente está marcado como exento
      (dc_noaplicareteneroperaciones = true), no se retiene.**

    - **Si el pago es de contado (terminosdepago = 0), se retiene.**

    - **Si el cliente tiene crédito pero su crédito restante es menor
      que el total de venta, se retiene.**

    - **Si el cliente tiene crédito sin límite definido, se revisan sus
      facturas activas y si alguna tiene dc_diasderetraso \> 0, también
      se retiene.**

4.  **Resultado:\
    Se asigna dc_retenidoporfinanzas = true si alguna condición aplica.\
    De lo contrario, se asigna false.**

**🧪 Casos de Prueba Recomendados**

  ------------------------------------------------------------------------
  **Escenario**                                 **Resultado Esperado**
  --------------------------------------------- --------------------------
  **Cliente con dc_noaplicareteneroperaciones = **dc_retenidoporfinanzas =
  true**                                        false**

  **Cliente con pago contado**                  **dc_retenidoporfinanzas =
                                                true**

  **Cliente con crédito y dc_creditorestanta \< **dc_retenidoporfinanzas =
  dc_totalventa**                               true**

  **Cliente con crédito sin límite definido y   **dc_retenidoporfinanzas =
  facturas vencidas**                           true**

  **Cliente con crédito suficiente y sin        **dc_retenidoporfinanzas =
  facturas vencidas**                           false**
  ------------------------------------------------------------------------

**✅ Resultado Esperado**

**Una vez ejecutado el plugin con dc_facturar = true, el campo
dc_retenidoporfinanzas en el registro de Transporte quedará actualizado
con true o false según el análisis financiero del cliente.**
