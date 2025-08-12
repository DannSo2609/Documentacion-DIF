**Documentación del Plugin: Cálculo de Totales de Cotización**

**🧩 Nombre del Plugin**

**DC_STP_Cotizacion.Proceso**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation)**

**🗂️ Entidad Principal**

**dc_cotizacion**

**🎯 Objetivo**

**Este plugin recalcula y actualiza los totales de costo, venta y profit
para una cotización (dc_cotizacion) cuando el campo dc_calculartotales
se establece en true. Una vez finalizado el cálculo, el campo se marca
como false para evitar recálculos innecesarios.**

**🧾 Campos Involucrados**

**En dc_cotizacion:**

  --------------------------------------------------------------------------
  **Campo**                     **Descripción**
  ----------------------------- --------------------------------------------
  **dc_calculartotales**        **Bandera para activar el cálculo de
                                totales**

  **dc_totalcostoventas**       **Total de costos en ventas**

  **dc_totalventaventas**       **Total de venta en ventas**

  **dc_profitdeventas**         **Profit en ventas (venta - costo)**

  **dc_totalcostoembarque**     **Total de costos de embarque**

  **dc_totalventaembarque**     **Total de venta de embarque**

  **dc_profitdeembarque**       **Profit en embarque (venta - costo)**

  **dc_totalcostoaduana**       **Total de costos en aduanas**

  **dc_totalventaaduana**       **Total de venta en aduanas**

  **dc_profitaduanas**          **Profit en aduanas (venta - costo)**

  **dc_totalcostotransporte**   **Total de costos en transporte**

  **dc_totalventatransporte**   **Total de venta en transporte**

  **dc_profittransporte**       **Profit en transporte (venta - costo)**
  --------------------------------------------------------------------------

**🔁 Lógica del Plugin**

1.  **Verifica Depth \<= 1**

    - **Previene ejecuciones recursivas.**

2.  **Verifica si dc_calculartotales = true**

    - **Si no, el plugin no hace nada.**

    - **Si es true, continúa el proceso de cálculo.**

3.  **Llama los siguientes métodos de cálculo:**

    - **ActualizarVentas**

    - **ActualizarEmbarques**

    - **ActualizarAduanas**

    - **ActualizarTransporte**

4.  **Cada método:**

    - **Llama a ObtenerTotales(context, nombre_entidad_producto).**

    - **Realiza un RetrieveMultiple con la cotización como filtro.**

    - **Suma los valores de dc_montodolarescosto y
      dc_montodolaresventa.**

    - **Calcula el profit = venta - costo.**

    - **Asigna los resultados a los campos respectivos de la cotización
      y hace Update.**

5.  **Al final del proceso:**

    - **Actualiza la cotización y pone dc_calculartotales = false.**

**🧪 Validaciones / Seguridad**

- **El plugin solo trabaja con registros que tengan dc_calculartotales =
  true.**

- **Los valores nulos en los montos se consideran 0.**

- **Se usa Math.Round(\..., 2) para redondear los totales.**

- **Si la entidad de productos no es válida, lanza una excepción.**

**✅ Resultado Esperado**

  --------------------------------------------------------------------------
  **Acción**             **Resultado**
  ---------------------- ---------------------------------------------------
  **dc_calculartotales = **Se actualizan los totales de venta, costo y
  true**                 profit por componente**

  **Final del plugin**   **dc_calculartotales se marca en false**
  --------------------------------------------------------------------------
