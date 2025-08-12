**Documentación del Plugin: Finalización de Facturas por Expediente**

**🧩 Nombre del Plugin**

**DC_FEU_Transporte.Proceso**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation)**

**🗂️ Entidad Principal**

**dc_transporte**

**🎯 Objetivo**

**Este plugin tiene como finalidad verificar si todas las operaciones
asociadas a un expediente están listas para facturar (aduanas,
embarques, transporte y ventas), y de ser así, marcar todas las facturas
relacionadas como completadas (dc_completada = true).**

**🧾 Campos Involucrados**

**En la entidad dc_transporte:**

  -------------------------------------------------------------------------
  **Campo técnico**          **Descripción**
  -------------------------- ----------------------------------------------
  **dc_expediente**          **Expediente relacionado**

  **dc_facturara**           **Cliente al que se facturará**

  **dc_facturar**            **Indicador si debe facturarse**

  **dc_cotizacion**          **Cotización relacionada**

  **statecode**              **Estado del registro (activo/inactivo)**

  **dc_estadodelregistro**   **Estado del proceso (seguimiento interno)**
  -------------------------------------------------------------------------

**En la entidad dc_asociadodenegocios:**

  --------------------------------------------------------------------------------
  **Campo técnico**               **Descripción**
  ------------------------------- ------------------------------------------------
  **dc_opcionesenviodefactura**   **Conjunto de opciones que indican qué módulos
                                  deben facturarse (0 = Embarques, 1 = Aduanas, 2
                                  = Transporte, 3 = Ventas)**

  --------------------------------------------------------------------------------

**🔁 Lógica del Plugin**

1.  **Validación de Datos Iniciales:**

    - **Se asegura que dc_expediente y dc_facturara no estén vacíos.**

    - **Recupera las opciones de facturación del cliente
      (dc_opcionesenviodefactura).**

2.  **Determinación de Validaciones a Ejecutar:**

    - **Si el cliente no tiene configuraciones, se asume que se deben
      validar todos los módulos.**

    - **Si tiene configuraciones, se activa la validación sólo de los
      módulos seleccionados (Embarques, Aduanas, Transporte, Ventas).**

3.  **Condición para Completar Facturas:**

    - **El registro actual debe tener dc_estadodelregistro = 7 o estar
      inactivo (statecode = 1) o ya facturado (dc_facturar = true).**

    - **Para cada módulo habilitado, se verifica si las entidades
      correspondientes (dc_aduanas, dc_transporte, dc_cotizacion)
      cumplen con al menos una de estas condiciones:**

      - **dc_facturar = true**

      - **statecode = 1**

      - **dc_estadodelregistro = 7 (ó dc_estadodelregistrojh = 5 para
        cotizaciones)**

4.  **Acción Final:**

    - **Si todas las condiciones anteriores se cumplen, el plugin
      actualiza los registros en dc_facturas relacionadas al expediente,
      marcándolos como completados (dc_completada = true).**

**🧪 Validaciones por Módulo**

  -------------------------------------------------------------------------------
  **Módulo**       **Entidad**              **Estado requerido**
  ---------------- ------------------------ -------------------------------------
  **Aduanas**      **dc_aduanas**           **dc_estadodelregistro = 7**

  **Transporte**   **dc_transporte**        **Evaluado desde entidad principal**

  **Embarques**    **dc_embarques**         **dc_estadodelregistro = 7**

  **Ventas**       **Dc_cotizacion /        **Debe haber al menos un producto con
                   dc_productosdeventas**   dc_ventaunitario \> 0 y statecode =
                                            0**
  -------------------------------------------------------------------------------

**✅ Resultado Esperado**

- **Todas las facturas del expediente actual (dc_facturas.dc_expediente)
  serán actualizadas a dc_completada = true, si se cumple lo
  siguiente:**

  - **El estado del proceso lo permite (estado = 7, inactivo o
    facturado)**

  - **Todas las validaciones por módulo habilitado fueron exitosas.**
