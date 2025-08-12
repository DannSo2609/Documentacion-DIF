**Documentación del Plugin: DC_RCA_RecalcularAduanas_DIF**

**🧩 Nombre del Plugin**

**DC_RCA_RecalcularAduanas_DIF.Proceso**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation) sobre la entidad dc_productosdeaduana**

**🗂️ Entidad Principal**

- **dc_productosdeaduana**

**🎯 Objetivo del Plugin**

**Cuando se modifica un registro en dc_productosdeaduana, el plugin:**

1.  **Marca como true el campo dc_calculartotales de la aduana
    relacionada (dc_aduanas).**

2.  **Marca como true el campo dc_recalcularcomision de las comisiones
    (dc_transaccionescomisiones) relacionadas a la aduana (por medio del
    dc_id como dc_transaccion).**

**📦 Campos Utilizados**

**Entidad: dc_productosdeaduana**

  -----------------------------------------------------------------------
  **Campo**             **Descripción**
  --------------------- -------------------------------------------------
  **dc_aduana**         **Lookup hacia dc_aduanas**

  -----------------------------------------------------------------------

**Entidad: dc_aduanas**

  -----------------------------------------------------------------------
  **Campo**                 **Descripción**
  ------------------------- ---------------------------------------------
  **dc_id**                 **ID de transacción relacionado**

  **dc_calculartotales**    **Indicador para recalcular totales**
  -----------------------------------------------------------------------

**Entidad: dc_transaccionescomisiones**

  ------------------------------------------------------------------------------
  **Campo**                   **Descripción**
  --------------------------- --------------------------------------------------
  **dc_transaccion**          **Identificador que debe coincidir con dc_id de la
                              aduana**

  **dc_recalcularcomision**   **Indicador para recalcular comisión**
  ------------------------------------------------------------------------------

**🔄 Flujo de Lógica**

1.  **Si la profundidad (context.Context.Depth) es menor o igual a 1
    (para evitar recursividad):**

2.  **Recupera el campo dc_aduana del registro dc_productosdeaduana.**

3.  **Si existe una aduana asociada:**

    - **Recupera el registro dc_aduanas.**

    - **Marca dc_calculartotales = true.**

4.  **Luego, obtiene todas las comisiones (dc_transaccionescomisiones)
    cuyo dc_transaccion = dc_id de la aduana y cuyo statecode = 0
    (activo).**

5.  **Por cada comisión encontrada, marca dc_recalcularcomision =
    true.**

**🧪 Casos de Prueba Recomendados**

  -----------------------------------------------------------------------
  **Escenario**                     **Resultado Esperado**
  --------------------------------- -------------------------------------
  **dc_aduana está presente en      **Se marca dc_calculartotales = true
  dc_productosdeaduana**            en aduana y se actualizan
                                    comisiones**

  **dc_aduana es null**             **No hace nada**

  **No existen comisiones con       **No se actualiza ninguna comisión,
  dc_transaccion = dc_id**          pero sí la aduana**

  **statecode ≠ 0 en comisiones**   **Se ignoran esas comisiones**

  **Plugin se llama desde una       **No hace nada**
  ejecución profunda (Depth \> 1)** 
  -----------------------------------------------------------------------

**⚠️ Notas Técnicas**

**✅ Uso correcto de ColumnSet para evitar sobrecarga.\
✅ Prevención de loops infinitos mediante Depth \<= 1.\
✅ Manejo seguro de campos opcionales usando GetAttributeValue\<T\>().\
❗ Si dc_id no es único en dc_transaccionescomisiones, se podrían marcar
más comisiones de las necesarias.\
❗ Podrías optimizar el FilterExpression usando el constructor directo
del QueryExpression.**
