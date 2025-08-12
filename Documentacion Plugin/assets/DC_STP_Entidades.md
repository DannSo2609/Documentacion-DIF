**Documentación del Plugin: Cálculo de Totales en Operaciones (Embarque,
Aduana, Transporte)**

**🧩 Nombre del Plugin**

**DC_STP_Entidades.Proceso**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation)**

**🗂️ Entidades Soportadas**

- **dc_embarques**

- **dc_aduanas**

- **dc_transporte**

**🎯 Objetivo**

**Este plugin calcula automáticamente los totales de costo, venta, notas
de crédito, profit y comisión de cada operación cuando se establece el
campo dc_calculartotales = true. Una vez calculado, se desactiva el flag
para evitar recálculos innecesarios.**

**🧾 Campos Actualizados en la Entidad Principal**

  ---------------------------------------------------------------------------------
  **Campo**                  **Descripción**
  -------------------------- ------------------------------------------------------
  **dc_totalcosto**          **Suma total de los costos en dólares
                             (dc_montodolarescosto)**

  **dc_totalventa**          **Suma total de las ventas en dólares
                             (dc_montodolaresventa)**

  **dc_totalnotascredito**   **Total de notas de crédito incluyendo NC + monto
                             crédito productos activos**

  **dc_profit**              **Ganancia neta: (venta - costo - notas crédito)**

  **dc_totalcomision**       **Comisión calculada según el tipo de operación**

  **dc_calculartotales**     **Se marca como false al finalizar el proceso**
  ---------------------------------------------------------------------------------

**🔁 Lógica del Plugin**

1.  **Profundidad de Ejecución (Depth \<= 1):\
    Controla la recursividad.**

2.  **Validación del Campo dc_calculartotales:\
    Solo se ejecuta si el campo es true.**

3.  **Obtiene Tablas Relacionadas a la Entidad:**

    - **Para cada tipo (embarque, aduanas, transporte) se asignan:**

      - **Entidad de relación**

      - **Tabla de productos**

      - **Tabla de notas de crédito**

4.  **Se Calculan los Totales:**

    - **dc_montodolarescosto, dc_montodolaresventa, dc_montoncdolares y
      dc_total (de la tabla de NC)**

    - **Solo productos con statecode = 0 (Activos)**

5.  **Se Calcula la Comisión:**

    - **Para Aduanas: comisión = montoNC \* 10%**

    - **Para Transporte y Embarque: comisión = (profit - notas crédito -
      total NC) \* %**

      - **Transporte: 10%**

      - **Embarque: 15%**

6.  **Se Actualiza la Entidad Principal con todos los totales.**

**🧮 Fórmulas Clave**

  -----------------------------------------------------------------------
  **Fórmula**                              **Descripción**
  ---------------------------------------- ------------------------------
  **Profit = Venta - Costo**               **Si venta ≠ 0, caso
                                           contrario: 0 - Costo**

  **TotalNotasCredito = totalNC +          **Se suman ambas fuentes**
  notasCredito**                           

  **Comisión Aduana = NC \* 10%**          

  **Comisión Transporte = (Profit - NC -   
  notas) \* 10%**                          

  **Comisión Embarque = (Profit - NC -     
  notas) \* 15%**                          
  -----------------------------------------------------------------------

**⚠️ Validaciones y Manejo de Errores**

- **Si la entidad no es una de las 3 soportadas, lanza excepción.**

- **Redondeos con Math.Round(\..., 2).**

- **Revisión de existencia del campo antes de sumar.**

- **Solo productos activos (statecode = 0) se incluyen en el cálculo.**

**✅ Resultado Esperado**

  -----------------------------------------------------------------------
  **Acción**                  **Resultado**
  --------------------------- -------------------------------------------
  **dc_calculartotales =      **Se actualizan los totales de la
  true**                      operación**

  **Final del plugin**        **dc_calculartotales se marca en false**
  -----------------------------------------------------------------------
