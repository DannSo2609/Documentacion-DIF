**Documentación del Plugin: Validación de Cliente en Transporte (Profit
Negativo Comentado)**

**🧩 Nombre del Plugin**

**DC_VPN_ValidacionProfitNegativoTransporte_DIF.Proceso**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation)**

**🗂️ Entidad Principal**

- **dc_transporte**

**🎯 Objetivo**

**Evitar que se facture un registro de transporte si el campo
dc_facturara no está asociado a un cliente (es decir, si dc_cliente =
false).**

**⚠️ La validación de profit negativo existe en el código pero está
comentada y no se encuentra activa.**

**🧾 Campos Evaluados**

**En dc_transporte:**

  -------------------------------------------------------------------------
  **Campo**          **Descripción**
  ------------------ ------------------------------------------------------
  **dc_facturar**    **Bandera que indica si se desea facturar**

  **dc_facturara**   **Cliente al que se facturará (EntityReference)**
  -------------------------------------------------------------------------

**En dc_asociadodenegocios:**

  -----------------------------------------------------------------------
  **Campo**        **Descripción**
  ---------------- ------------------------------------------------------
  **dc_cliente**   **Indica si está marcado como cliente**

  -----------------------------------------------------------------------

**🔁 Lógica de Validación**

1.  **Validación de Cliente:**

    - **Si dc_facturar = true y el cliente en dc_facturara no tiene
      dc_cliente = true:**

**text**

**CopyEdit**

**Se lanza la siguiente excepción:**

**\"El Facturar a no está marcado como cliente, favor revisar.\"**

2.  **(No activa) Validación de Profit Negativo:**

    - **Comentado: verifica si el profit es negativo y si no ha sido
      aprobado administrativamente.**

**✅ Resultado Esperado**

**El plugin impide la facturación de un transporte si el cliente
relacionado no está marcado como cliente, lo cual previene errores
contables o de facturación.**

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

- **✅ Eliminar el else vacío o agregar comentarios si será usado más
  adelante.**

- **✅ Eliminar el método GetEntityCollection si no se usará.**

- **🔄 Renombrar variable Aduanas a Transporte para mantener
  consistencia con la entidad (dc_transporte).**

- **⚠️ Si se desea validar el profit negativo, se debe descomentar la
  sección correspondiente y asegurar que los campos requeridos
  (dc_profit, dc_facturaradm) estén presentes y bien manejados.**
