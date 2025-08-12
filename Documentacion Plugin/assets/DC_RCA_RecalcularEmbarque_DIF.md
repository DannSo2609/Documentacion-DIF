**Documentación del Plugin: DC_RCA_RecalcularEmbarque_DIF**

**🧩 Nombre del Plugin**

**DC_RCA_RecalcularEmbarque_DIF.Proceso**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation) sobre la entidad dc_productosdeembarque**

**🗂️ Entidad Principal**

- **dc_productosdeembarque**

**🎯 Objetivo del Plugin**

**Cuando se actualiza un registro en dc_productosdeembarque, el
plugin:**

1.  **Marca como true el campo dc_calculartotales del embarque asociado
    (dc_embarques).**

2.  **Marca como true el campo dc_recalcularcomision de las comisiones
    (dc_transaccionescomisiones) relacionadas al embarque (por medio del
    dc_hbl como dc_transaccion).**

**📦 Campos Utilizados**

**Entidad: dc_productosdeembarque**

  -----------------------------------------------------------------------
  **Campo**               **Descripción**
  ----------------------- -----------------------------------------------
  **dc_embarque**         **Lookup hacia dc_embarques**

  -----------------------------------------------------------------------

**Entidad: dc_embarques**

  --------------------------------------------------------------------------
  **Campo**                **Descripción**
  ------------------------ -------------------------------------------------
  **dc_hbl**               **Número de HBL usado como transacción**

  **dc_calculartotales**   **Indicador para recalcular totales**
  --------------------------------------------------------------------------

**Entidad: dc_transaccionescomisiones**

  -------------------------------------------------------------------------------
  **Campo**                   **Descripción**
  --------------------------- ---------------------------------------------------
  **dc_transaccion**          **Identificador que debe coincidir con dc_hbl del
                              embarque**

  **statecode**               **Debe ser 0 (activo)**

  **dc_recalcularcomision**   **Indicador para recalcular comisión**
  -------------------------------------------------------------------------------

**🔄 Flujo de Lógica**

1.  **Verifica que el plugin no esté corriendo en profundidad mayor a 1
    (Depth \<= 1).**

2.  **Recupera el campo dc_embarque del registro
    dc_productosdeembarque.**

3.  **Si existe una referencia a dc_embarques, recupera el embarque:**

    - **Marca dc_calculartotales = true.**

4.  **Luego, obtiene todas las comisiones (dc_transaccionescomisiones)
    donde:**

    - **dc_transaccion = dc_hbl del embarque.**

    - **statecode = 0 (activo).**

5.  **Por cada comisión encontrada, marca dc_recalcularcomision =
    true.**

**🧪 Casos de Prueba Recomendados**

  -----------------------------------------------------------------------
  **Escenario**                             **Resultado Esperado**
  ----------------------------------------- -----------------------------
  **El campo dc_embarque está presente en   **Se actualiza el embarque y
  dc_productosdeembarque**                  las comisiones asociadas**

  **El campo dc_embarque es null**          **No hace nada**

  **No existen comisiones con               **Solo se actualiza el
  dc_transaccion = dc_hbl**                 embarque, no las comisiones**

  **statecode ≠ 0 en comisiones**           **No se actualiza esa
                                            comisión**

  **Plugin ejecutado con Depth \> 1**       **No se ejecuta**
  -----------------------------------------------------------------------
