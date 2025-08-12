**Documentación del Plugin: Validación de Producto Seguro en Embarque**

**🧩 Nombre del Plugin**

**DC_LS_LlevaSeguro.Proceso**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation)**

**Aplicable cuando se actualiza el campo dc_llevaseguro o cualquier
campo relacionado del embarque.**

**🗂️ Entidad Principal**

- **dc_embarques**

**🎯 Objetivo**

**Validar que, cuando el embarque está marcado como "Lleva Seguro"
(dc_llevaseguro = true):**

- **Exista al menos un producto de seguro entre los productos
  relacionados al embarque (dc_productorelacionado).**

- **En caso de que el producto sea seguro (PS-00067 o PS-00069), se le
  asigna como cantidad la suma de dc_valorfob + dc_flete.**

**🧾 Campos Utilizados**

**En dc_embarques:**

  ----------------------------------------------------------------------------
  **Campo**            **Tipo**       **Descripción**
  -------------------- -------------- ----------------------------------------
  **dc_llevaseguro**   **Boolean**    **Indica si el embarque lleva seguro**

  **dc_valorfob**      **Decimal**    **Valor FOB del embarque**

  **dc_flete**         **Decimal**    **Valor del flete**

  **dc_etd**           **DateTime**   **Estimated Time of Departure**
  ----------------------------------------------------------------------------

**En dc_productosdeembarque:**

  -----------------------------------------------------------------------------------
  **Campo**                    **Tipo**      **Descripción**
  ---------------------------- ------------- ----------------------------------------
  **dc_productorelacionado**   **String**    **Código del producto (se valida si es
                                             seguro)**

  **dc_cantidad**              **Decimal**   **Se establece si el producto es
                                             seguro**
  -----------------------------------------------------------------------------------

**🔁 Lógica del Plugin**

1.  **Si dc_llevaseguro es false, no hace nada.**

2.  **Si es true:**

    - **Consulta los registros relacionados en dc_productosdeembarque
      vinculados al embarque actual.**

    - **Recorre cada producto:**

      - **Si dc_productorelacionado es PS-00067 o PS-00069, entonces:**

        - **Calcula dc_valorfob + dc_flete.**

        - **Asigna el valor resultante a dc_cantidad del producto.**

        - **Actualiza el producto.**

        - **Finaliza el ciclo.**

    - **Si no se encontró ningún producto seguro y dc_etd es diferente
      de la fecha por defecto (01/01/0001), lanza una excepción.**

**⚠️ Excepción que Puede Lanzar**

**\"No se ha encontrado ningún producto de seguro. Favor validar si el
producto aplica, y de lo contrario marcar que no lleva seguro.\"**

**Esto obliga al usuario a revisar el embarque si no se detectó el
producto seguro correspondiente.**

**✅ Resultado Esperado**

- **Garantiza que no se facturen embarques marcados con seguro sin tener
  un producto de seguro asignado correctamente.**

**🧪 Casos de Prueba Recomendados**

  -------------------------------------------------------------------------------
  **dc_llevaseguro**   **Productos Relacionados**   **Resultado Esperado**
  -------------------- ---------------------------- -----------------------------
  **false**            **---**                      **No hace nada**

  **true**             **Ningún producto seguro**   **❌ Lanza excepción**

  **true**             **Al menos 1 producto        **✅ Actualiza dc_cantidad y
                       PS-00067 o PS-00069**        permite continuar**
  -------------------------------------------------------------------------------

**🧹 Recomendaciones Técnicas**

- **La fecha InvalidDate = new DateTime(01, 01, 0001) es incorrecta
  (causa ArgumentOutOfRangeException).\
  ✅ Usa mejor:**

**csharp**

**CopyEdit**

**DateTime InvalidDate = DateTime.MinValue;**

- **Se podría mejorar la eficiencia evitando el context.Update(producto)
  dentro del foreach si hay más productos seguros a procesar.**
