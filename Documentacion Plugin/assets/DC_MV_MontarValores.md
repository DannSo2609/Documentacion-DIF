**Documentación del Plugin: Montar Valores Declaración**

**🧩 Nombre del Plugin**

**DC_MV_MontarValores.Proceso**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation)**

**Se activa cuando se actualiza un registro de tipo dc_declaraciones.**

**🗂️ Entidad Principal**

- **dc_declaraciones**

**🎯 Objetivo**

**Ajustar los valores FOB (dc_fobtotal) de los productos relacionados
(dc_productosdeclaraciones) redistribuyendo proporcionalmente el monto
de CIF (Seguro + Flete) o solo el Flete, según las banderas marcadas:**

- **dc_desmontarvalorcif → Desmontar y distribuir CIF.**

- **dc_desmontarflete → Desmontar y distribuir solo Flete.**

**🧾 Campos Usados**

**En dc_declaraciones:**

  ------------------------------------------------------------------------------
  **Campo**                       **Tipo**      **Uso**
  ------------------------------- ------------- --------------------------------
  **dc_valordeclaradooriginal**   **Boolean**   **Activa la ejecución del
                                                plugin**

  **dc_valorfob**                 **Decimal**   **Valor FOB original**

  **dc_flete**                    **Decimal**   **Valor de flete total**

  **dc_seguro**                   **Decimal**   **Valor del seguro**

  **dc_desmontarvalorcif**        **Boolean**   **Indica si desmonta CIF**

  **dc_desmontarflete**           **Boolean**   **Indica si desmonta solo
                                                flete**
  ------------------------------------------------------------------------------

**En dc_productosdeclaraciones:**

  -------------------------------------------------------------------------
  **Campo**         **Tipo**      **Descripción**
  ----------------- ------------- -----------------------------------------
  **dc_id**         **String**    **Identificador de producto**

  **dc_fobtotal**   **Decimal**   **Valor FOB total del producto**
  -------------------------------------------------------------------------

**🔁 Lógica del Plugin**

1.  **Se activa solo si dc_valordeclaradooriginal = true.**

2.  **Recupera valores de flete, seguro y FOB original.**

3.  **Valida que ni el dc_flete ni el dc_valorfob sean cero → si alguno
    lo es, lanza excepción.**

4.  **Busca los productos relacionados a la declaración.**

5.  **Si hay productos:**

    - **Si dc_desmontarvalorcif = true:**

      - **Por cada producto, suma proporcional de FOB + CIF.**

    - **Si dc_desmontarflete = true:**

      - **Por cada producto, suma proporcional de FOB + Flete.**

    - **Se actualiza:**

      - **Cada producto con el nuevo dc_fobtotal.**

      - **La declaración con el nuevo total de dc_valorfob.**

6.  **Si no hay productos relacionados, lanza excepción.**

**⚠️ Excepciones que Puede Lanzar**

1.  **Valores FOB o Flete nulos o cero:**

**\"El registro actual posee un Flete o un Valor FOB definido en
0\...\"**

2.  **Sin productos relacionados:**

**\"El registro actual no posee Productos Relacionados como para
realizar este procedimiento.\"**

**🧪 Casos de Prueba Recomendados**

  -----------------------------------------------------------------------
  **Condición**                **Resultado Esperado**
  ---------------------------- ------------------------------------------
  **Flete o ValorFOB en 0**    **❌ Lanza excepción**

  **No hay productos           **❌ Lanza excepción**
  relacionados**               

  **Lleva CIF marcado**        **✅ Recalcula dc_fobtotal proporcional +
                               CIF**

  **Lleva solo Flete marcado** **✅ Recalcula dc_fobtotal proporcional +
                               Flete**

  **Ambos marcados**           **✅ Aplica los dos cálculos**
  -----------------------------------------------------------------------
