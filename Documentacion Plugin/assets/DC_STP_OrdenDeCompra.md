**Documentación del Plugin: Cálculo de Totales - Orden de Compra / Nota
de Crédito**

**🧩 Nombre del Plugin**

**DC_STP_OrdenDeCompra.Proceso**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation)**

**🗂️ Entidades Soportadas**

- **dc_ordenesdecompra**

- **dc_notasdecredito**

**🎯 Objetivo**

**Este plugin tiene como finalidad calcular automáticamente el precio
total de una orden de compra o nota de crédito, y convertir dicho total
a dólares estadounidenses (USD) utilizando la tasa del sistema y el tipo
de cambio correspondiente.**

**🧾 Campos Calculados**

  --------------------------------------------------------------------------------
  **Campo**                    **Descripción**
  ---------------------------- ---------------------------------------------------
  **dc_preciototal**           **Suma total de los productos relacionados en la
                               entidad origen**

  **dc_preciototal_dolares**   **Total convertido a dólares usando tipo de
                               cambio**

  **dc_calculartotales**       **Campo booleano, se marca como false luego del
                               cálculo**
  --------------------------------------------------------------------------------

**🔁 Lógica General del Plugin**

1.  **Profundidad:\
    Solo se ejecuta si Depth \<= 1.**

2.  **Verificación de Cálculo:\
    Verifica que el campo dc_calculartotales esté en true.**

3.  **Obtención de Productos:**

    - **Usa la relación entre dc_ordenesdecompra → dc_productosoc**

    - **O dc_notasdecredito → dc_productosnc**

4.  **Cálculo del Precio Total:\
    Suma de todos los valores del campo dc_total en los productos
    relacionados.**

5.  **Conversión a Dólares:**

    - **Se busca la moneda (dc_moneda) asociada al registro.**

    - **Se obtiene la última tasa del sistema (dc_tasa).**

    - **Luego se busca el tipo de cambio (dc_tipodecambio) de la moneda
      origen a USD.**

    - **Se multiplica el precio total por el tipo de cambio.**

6.  **Actualización del Registro:**

    - **Se actualiza la entidad (dc_ordenesdecompra o dc_notasdecredito)
      con:**

      - **dc_preciototal**

      - **dc_preciototal_dolares**

      - **dc_calculartotales = false**

**🧮 Fórmulas Clave**

  -----------------------------------------------------------------------
  **Fórmula**                           **Descripción**
  ------------------------------------- ---------------------------------
  **dc_preciototal = suma(dc_total)**   **Suma de productos
                                        relacionados**

  **PrecioEnDolares = total \*          **Conversión del monto a USD**
  tipoCambio**                          
  -----------------------------------------------------------------------

**🗂️ Métodos Auxiliares**

  -------------------------------------------------------------------------------
  **Método**                    **Función**
  ----------------------------- -------------------------------------------------
  **ObtenerTablasPorEntidad**   **Determina el nombre de las entidades hijas
                                (productos)**

  **ObtenerProductos**          **Retorna los productos de una OC o Nota de
                                Crédito**

  **ObtenerTasa**               **Obtiene la última tasa del sistema (dc_tasa)**

  **ObtenerTipoDeCambio**       **Consulta dc_tipodecambio entre la moneda del
                                documento y USD**

  **ObtenerMontoEnDolares**     **Multiplica el total por el tipo de cambio**

  **FieldDecimalValue**         **Extrae valor decimal de un campo con
                                validación**
  -------------------------------------------------------------------------------

**⚠️ Validaciones y Excepciones**

  -----------------------------------------------------------------------
  **Escenario**                  **Acción del Plugin**
  ------------------------------ ----------------------------------------
  **Si dc_moneda no está         **Lanza InvalidPluginExecutionException
  definida**                     con mensaje detallado**

  **Si no existe ninguna         **Lanza InvalidPluginExecutionException
  dc_tasa**                      solicitando generar una**

  **Si no existe dc_tipodecambio **Lanza InvalidPluginExecutionException
  para moneda origen a USD**     con instrucciones al usuario**
  -----------------------------------------------------------------------

**✅ Resultado Esperado**

**Después de ejecutar el plugin con dc_calculartotales = true, el
sistema debe:**

- **Calcular el total de los productos.**

- **Obtener el tipo de cambio vigente hacia dólares.**

- **Guardar el total en la moneda original y su equivalente en USD.**

- **Cambiar dc_calculartotales a false.**
