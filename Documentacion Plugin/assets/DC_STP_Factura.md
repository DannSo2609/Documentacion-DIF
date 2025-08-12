**Documentación del Plugin: Cálculo de Totales en Facturas**

**🧩 Nombre del Plugin**

**DC_STP_Factura.Proceso**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation)**

**🗂️ Entidad Principal**

- **dc_facturas**

**🎯 Objetivo**

**Este plugin se encarga de calcular automáticamente el precio total de
la factura, desglosado por departamento (Embarques, Aduanas,
Transportes, Ventas), y los convierte a dólares utilizando la tasa del
sistema y el tipo de cambio correspondiente.**

**🧾 Campos Calculados en dc_facturas**

  -----------------------------------------------------------------------------
  **Campo**                    **Descripción**
  ---------------------------- ------------------------------------------------
  **dc_preciototal**           **Total sumado de todos los productos de la
                               factura**

  **dc_preciototal_dolares**   **Total en dólares usando tipo de cambio**

  **dc_totalembarque**         **Total en dólares del departamento Embarque**

  **dc_totaladuanas**          **Total en dólares del departamento Aduanas**

  **dc_totaltransportes**      **Total en dólares del departamento
                               Transportes**

  **dc_totalventas**           **Total en dólares del departamento Ventas**

  **dc_calculartotales**       **Se marca como false al finalizar el proceso**
  -----------------------------------------------------------------------------

**🔁 Lógica del Plugin**

1.  **Control de Profundidad:\
    Solo se ejecuta si Depth \<= 1 para evitar recursividad.**

2.  **Validación de Cálculo:\
    Si dc_calculartotales = true, inicia el cálculo.**

3.  **Obtención de Productos de la Factura:\
    Se consultan todos los dc_productosfactura relacionados.**

4.  **Clasificación por Departamento:\
    Se suman totales individuales por:**

    - **Embarques (departamento 1)**

    - **Aduanas (departamento 2 con traducción = \"CUSTOMS
      CLEARANCE\")**

    - **Transportes (departamento 3)**

    - **Ventas (departamento 0)**

5.  **Conversión de Montos a Dólares:**

    - **Se obtiene la moneda de la factura (dc_moneda)**

    - **Se busca la tasa del sistema**

    - **Se consulta el tipo de cambio en dc_tipodecambio**

    - **Se realiza la conversión: total \* tipoCambio**

6.  **Actualización de Factura:\
    Los montos calculados son asignados a los campos correspondientes.**

**🧮 Fórmulas Clave**

  -----------------------------------------------------------------------
  **Fórmula**                  **Descripción**
  ---------------------------- ------------------------------------------
  **PrecioEnDolares = total \* **Conversión a USD**
  tipoCambio**                 

  **TotalDepartamento =        **Suma por departamento con filtro según
  suma(dc_total)**             dc_departamento**

  **dc_calculartotales =       **Se desactiva al finalizar**
  false**                      
  -----------------------------------------------------------------------

**⚠️ Validaciones y Excepciones**

- **Si no tiene moneda definida, lanza excepción:**

***\"Parece que la Factura no posee una moneda definida\...\"***

- **Si no hay tasa del sistema, lanza excepción:**

***\"No hemos podido encontrar una Tasa del Sistema\...\"***

- **Si no encuentra tipo de cambio entre moneda y dólar, lanza:**

***\"No hemos podido encontrar un Tipo de Cambio\...\"***

**✅ Resultado Esperado**

**Al ejecutar el plugin con dc_calculartotales = true, se completa lo
siguiente:**

- **Se suman los productos de la factura.**

- **Se desglosan por área.**

- **Se convierte todo a dólares.**

- **Se actualiza la entidad dc_facturas con los valores.**

- **Se marca dc_calculartotales = false.**
