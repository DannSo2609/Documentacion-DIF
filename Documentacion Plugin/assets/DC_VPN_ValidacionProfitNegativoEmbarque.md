**Documentación del Plugin: Validación de Profit Negativo en Embarque**

**🧩 Nombre del Plugin**

**DC_VPN_ValidacionProfitNegativoEmbarque_DIF.Proceso**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation)**

**🗂️ Entidad Principal**

- **dc_embarques**

**🎯 Objetivo**

**Este plugin evita la facturación de un embarque si:**

1.  **El campo dc_facturara no tiene marcada la opción Cliente.**

2.  **El valor del campo dc_profit es negativo y no ha sido aprobado por
    administración.**

**🧾 Campos Evaluados**

**En dc_embarques:**

  ----------------------------------------------------------------------------
  **Campo**            **Descripción**
  -------------------- -------------------------------------------------------
  **dc_facturar**      **Bandera que indica si el embarque será facturado**

  **dc_facturaradm**   **Bandera que indica si se aprobó la facturación por
                       admin**

  **dc_profit**        **Valor de ganancia/profit calculado**

  **dc_facturara**     **Referencia al cliente a quien se facturará**
  ----------------------------------------------------------------------------

**En dc_asociadodenegocios:**

  --------------------------------------------------------------------------
  **Campo**        **Descripción**
  ---------------- ---------------------------------------------------------
  **dc_cliente**   **Bandera booleana que indica si es un cliente**

  --------------------------------------------------------------------------

**🔁 Lógica de Validación**

1.  **Validación de Cliente:**

    - **Si dc_facturar = true y el cliente referenciado en dc_facturara
      no tiene marcada la opción dc_cliente = true, entonces:**

**text**

**CopyEdit**

**Error: El Facturar a no está marcado como cliente, favor revisar.**

2.  **Validación de Profit Negativo sin Aprobación:**

    - **Si dc_facturar = true y dc_facturaradm = false y dc_profit \< 0,
      entonces:**

**text**

**CopyEdit**

**Error: El embarque presenta un saldo negativo en la categoría de
beneficios (PROFIT).**

**Por favor, valide los cargos agregados; de ser necesario, proceda con
su facturación tras solicitar aprobación de su supervisor.**

**✅ Resultado Esperado**

**El plugin previene la ejecución del proceso de facturación si alguna
de las dos validaciones descritas falla.**

**🧪 Casos de Prueba Recomendados**

  -----------------------------------------------------------------------
  **Escenario**                         **Resultado Esperado**
  ------------------------------------- ---------------------------------
  **dc_facturara sin dc_cliente         **Se lanza excepción**
  marcado**                             

  **dc_profit \< 0 y dc_facturaradm =   **Se lanza excepción**
  false**                               

  **dc_profit \< 0 y dc_facturaradm =   **Permite continuar**
  true**                                

  **dc_profit \>= 0**                   **Permite continuar**

  **dc_facturar = false**               **No se ejecuta ninguna
                                        validación**
  -----------------------------------------------------------------------
