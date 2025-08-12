**Documentación del Plugin: Validación de Cliente en Aduanas (Profit
Negativo Comentado)**

**🧩 Nombre del Plugin**

**DC_VPN_ValidacionProfitNegativoAduanas_DIF.Proceso**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation)**

**🗂️ Entidad Principal**

- **dc_aduanas**

**🎯 Objetivo**

**Este plugin valida que el campo dc_facturara (cliente) esté
correctamente marcado como cliente (dc_cliente = true) antes de permitir
la facturación de un registro de aduanas.**

**⚠️ Nota: La validación de profit negativo está comentada y actualmente
no está activa.**

**🧾 Campos Evaluados**

**En dc_aduanas:**

  ---------------------------------------------------------------------------
  **Campo**          **Descripción**
  ------------------ --------------------------------------------------------
  **dc_facturar**    **Bandera que indica si se desea facturar**

  **dc_facturara**   **Cliente al que se facturará (referencia a entidad)**
  ---------------------------------------------------------------------------

**En dc_asociadodenegocios:**

  --------------------------------------------------------------------------
  **Campo**        **Descripción**
  ---------------- ---------------------------------------------------------
  **dc_cliente**   **Indica si la cuenta está marcada como cliente**

  --------------------------------------------------------------------------

**🔁 Lógica de Validación**

1.  **Validación de Cliente:**

    - **Si dc_facturar = true y el cliente en dc_facturara no tiene
      dc_cliente = true:**

**text**

**CopyEdit**

**Error: El Facturar a no está marcado como cliente, favor revisar.**

2.  **(No activa) Validación de Profit Negativo:**

    - **Código presente pero comentado. Si se activa, verificaría si el
      profit es negativo y si no ha sido aprobado para facturación.**

**✅ Resultado Esperado**

**Previene la facturación de registros de Aduanas si el cliente
referenciado no está marcado como cliente.**

**🧪 Casos de Prueba Recomendados**

  -----------------------------------------------------------------------
  **Escenario**                                **Resultado Esperado**
  -------------------------------------------- --------------------------
  **Cliente referenciado no está marcado como  **Se lanza excepción**
  dc_cliente**                                 

  **Cliente referenciado sí está marcado como  **Permite continuar**
  dc_cliente**                                 

  **dc_facturar = false**                      **No se realiza ninguna
                                               validación**
  -----------------------------------------------------------------------

**🧹 Recomendaciones de Mejora**

- **✅ Eliminar el else vacío.**

- **✅ Eliminar o implementar el método GetEntityCollection si no será
  utilizado.**

- **✅ Si el cálculo de profit negativo será necesario, considerar
  descomentar y mover esa lógica a una validación propia si aplica.**
