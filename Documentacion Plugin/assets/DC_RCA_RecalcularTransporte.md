**Documentación del Plugin: DC_RCA_RecalcularTransporte_DIF**

**🧩 Nombre del Plugin**

**DC_RCA_RecalcularTransporte_DIF.Proceso**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation) sobre la entidad dc_productosdetransporte**

**🗂️ Entidad Principal**

- **dc_productosdetransporte**

**🎯 Objetivo del Plugin**

**Cuando se actualiza un registro en dc_productosdetransporte, el
plugin:**

1.  **Marca como true el campo dc_calculartotales del transporte
    asociado (dc_transporte).**

2.  **Marca como true el campo dc_recalcularcomision en los registros de
    dc_transaccionescomisiones relacionados con el transporte (por medio
    del dc_id como dc_transaccion).**

**📦 Campos Utilizados**

**Entidad: dc_productosdetransporte**

  -----------------------------------------------------------------------
  **Campo**               **Descripción**
  ----------------------- -----------------------------------------------
  **dc_transporte**       **Lookup hacia dc_transporte**

  -----------------------------------------------------------------------

**Entidad: dc_transporte**

  -----------------------------------------------------------------------
  **Campo**                **Descripción**
  ------------------------ ----------------------------------------------
  **dc_id**                **ID usado como referencia externa**

  **dc_calculartotales**   **Bandera para indicar recálculo**
  -----------------------------------------------------------------------

**Entidad: dc_transaccionescomisiones**

  ---------------------------------------------------------------------------
  **Campo**                   **Descripción**
  --------------------------- -----------------------------------------------
  **dc_transaccion**          **Debe coincidir con el dc_id del transporte**

  **statecode**               **Debe ser 0 (activo)**

  **dc_recalcularcomision**   **Bandera para disparar recálculo de comisión**
  ---------------------------------------------------------------------------

**🔄 Flujo de Lógica**

1.  **Verifica que el plugin no se esté ejecutando recursivamente (Depth
    \<= 1).**

2.  **Recupera el campo dc_transporte del registro
    dc_productosdetransporte.**

3.  **Si existe una referencia válida, obtiene el transporte y su
    dc_id.**

4.  **Actualiza el transporte marcando dc_calculartotales = true.**

5.  **Busca las comisiones (dc_transaccionescomisiones) relacionadas al
    dc_id del transporte:**

    - **Solo comisiones con statecode = 0.**

6.  **Por cada comisión encontrada, actualiza dc_recalcularcomision =
    true.**

**🧪 Casos de Prueba Recomendados**

  -----------------------------------------------------------------------
  **Escenario**                          **Resultado Esperado**
  -------------------------------------- --------------------------------
  **dc_transporte tiene valor**          **Se actualiza el transporte y
                                         las comisiones relacionadas**

  **dc_transporte es null**              **No se realiza ninguna acción**

  **No existen comisiones con            **Solo se actualiza el
  dc_transaccion = dc_id del             transporte**
  transporte**                           

  **Comisiones con statecode ≠ 0**       **Se omiten en la
                                         actualización**

  **Plugin ejecutado con Depth \> 1**    **No se ejecuta**
  -----------------------------------------------------------------------

**⚠️ Notas Técnicas**

**✅ Buen control de profundidad para evitar loops.\
✅ Recuperación defensiva de datos con GetAttributeValue\<T\>().\
✅ Lógica clara para actualización de múltiples registros.\
❗ El campo dc_id debe estar correctamente poblado en dc_transporte para
que las comisiones se relacionen correctamente.\
❗ La tabla dc_transaccionescomisiones debe tener sus claves de relación
adecuadamente configuradas.**
