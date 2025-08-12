**Documentación del Plugin: Cálculo Automático de Volumen Marítimo en
Ventas**

**Nombre del Plugin**

**DC_CVM_Ventas.Proceso**

**Acción**

- **Update (Post-Operation)**

**Entidad Principal**

- **opportunity (Ventas)**

**Descripción General**

**Este plugin se ejecuta automáticamente al actualizar una venta
(opportunity) cuando el campo dc_calcularvolumenmaritimo está activado.
Su función es calcular automáticamente el volumen total en metros
cúbicos, pies cúbicos y centímetros cúbicos a partir de los registros de
volumen marítimo asociados, y actualizar estos valores en la venta
correspondiente.**

**Lógica Interna Principal**

1.  **Verificación del Flag de Cálculo**

    - **Solo ejecuta la lógica si dc_calcularvolumenmaritimo está
      activado.**

2.  **Recuperación de la Venta y Unidad de Medida**

    - **Obtiene la entidad opportunity con el campo
      dc_medidadimensiones.**

3.  **Consulta de Volúmenes Marítimos Asociados**

    - **Recupera registros de dc_volumenmaritimo relacionados con la
      venta.**

4.  **Validación de Existencia de Registros**

    - **Si no se encuentran volúmenes relacionados, se lanza una
      excepción.**

5.  **Suma de Volúmenes y Conversión**

    - **Se totalizan los valores en metros cúbicos.**

    - **Se convierten a pies cúbicos (1 m³ = 35.3147 ft³).**

    - **Se convierten a centímetros cúbicos (1 ft³ = 28,316.8 cm³).**

6.  **Actualización de la Venta**

    - **Se actualizan los campos:**

      - **dc_metrocubico**

      - **dc_piescubico**

      - **dc_centimetroscubicos**

**Entidades Involucradas**

- **opportunity (Ventas)**

- **dc_volumenmaritimo (registros de volumen asociados)**

**Campos Calculados**

  -----------------------------------------------------------------------
  **Campo en Ventas**           **Descripción**
  ----------------------------- -----------------------------------------
  **dc_metrocubico**            **Volumen total en m³**

  **dc_piescubico**             **Conversión a pies cúbicos**

  **dc_centimetroscubicos**     **Conversión a centímetros cúbicos**
  -----------------------------------------------------------------------

**Manejo de Errores**

- **Lanza error si no existen registros de volumen relacionados.**

- **Registra mensajes de traza para facilitar depuración.**

**Beneficios**

- **Automatiza el proceso de cálculo de volumen logístico.**

- **Asegura coherencia de datos en diferentes unidades de medida.**

- **Mejora la calidad del dato para procesos operativos y comerciales.**

**✅ Resumen**

**Este plugin permite calcular automáticamente los volúmenes totales en
distintos formatos (m³, ft³, cm³) a partir de registros relacionados, y
asignarlos directamente a una venta. Es ideal para operaciones marítimas
que requieren control detallado del volumen de carga.**
