**Documentación del Plugin: Cálculo Automático de Peso Neto en
Declaraciones**

**Nombre del Plugin**

**DC_CPN_Declaraciones.Proceso**

**Acción**

- **Update (Post-Operation)**

**Entidad Principal**

- **dc_declaraciones**

**Descripción General**

**Este plugin se ejecuta automáticamente al actualizar una declaración
(dc_declaraciones). Su objetivo es calcular y actualizar automáticamente
los totales de cantidad, valor FOB y pesos (neto y bruto) en la
declaración, sumando los valores de todos los productos relacionados
cuando el campo booleano dc_calcularpesoneto está marcado en la
declaración.**

**Este proceso centraliza y automatiza el cálculo, evitando errores
manuales y asegurando integridad y coherencia en los datos de cada
declaración.**

**Lógica Interna Principal**

1.  **Prevención de Recursividad**

    - **El plugin ejecuta su lógica solo si la profundidad de contexto
      es 1 (evita loops infinitos).**

2.  **Validación de Condición**

    - **Solo procede si el campo booleano dc_calcularpesoneto está
      marcado como verdadero en la declaración.**

3.  **Recuperación de Productos Relacionados**

    - **Busca todos los productos asociados a la declaración
      (dc_declaraciondga) y recupera los campos clave: cantidad, FOB
      total, peso neto autorizado, unidad de medida y estado.**

4.  **Cálculo de Totales**

    - **Suma las cantidades, valores FOB y pesos netos de todos los
      productos activos (statecode = 0).**

    - **Si existen productos en kilogramos (unidad = 1), calcula también
      los totales específicos para kilogramos.**

    - **Calcula el peso y valor FOB restante restando los valores de
      kilogramos (si aplica) al bruto de la declaración.**

5.  **Actualización de la Declaración**

    - **Actualiza los campos de totales en la declaración:**

      - **dc_cantidadtotaldeproductos**

      - **dc_valorfobtotaldeproductos**

      - **dc_totalpesokilogramos**

      - **dc_totalfobkilogramos**

      - **dc_pesoneto**

    - **Marca el campo dc_calcularpesoneto como falso al finalizar el
      cálculo.**

6.  **Manejo de Casos Sin Productos**

    - **Si no existen productos relacionados, limpia los totales de la
      declaración y marca el cálculo como realizado.**

**Validaciones Incluidas**

- **Valida existencia de productos activos antes de calcular.**

- **Realiza sumas diferenciadas según unidad de medida (en particular,
  para kilogramos).**

- **Asegura que los campos decimales sean tratados correctamente incluso
  si algún producto carece de valor.**

**Entidades Involucradas**

- **dc_declaraciones (declaración principal)**

- **dc_productosdeclaraciones (productos relacionados a la
  declaración)**

**Métodos y Cálculos Importantes**

- **Sumatoria condicional:\
  Suma valores solo de productos activos (statecode = 0) y, para
  kilogramos, con unidad de medida específica.**

- **Cálculo de peso y FOB restantes:\
  Si existen valores en kilogramos, el peso y valor FOB restantes se
  calculan restando los kilogramos del bruto/FOB totales.**

**Manejo de Errores**

- **Registra todos los errores en el servicio de trazas (tracingService)
  para facilitar soporte y debugging.**

- **Lanza el error sin modificar para asegurar visibilidad en CRM.**

**Consideraciones Finales**

- **El plugin automatiza el cálculo y actualización de totales en
  declaraciones, eliminando pasos manuales y asegurando datos
  consistentes.**

- **Puede ser extendido fácilmente para nuevas unidades de medida o
  reglas de negocio adicionales.**

- **Facilita la trazabilidad y control en el proceso de gestión
  aduanera.**

**✅ Resumen**

**Este plugin suma automáticamente las cantidades, valores FOB y pesos
netos de todos los productos relacionados a una declaración, dejando los
totales correctamente reflejados y asegurando la consistencia de la
información para procesos aduaneros y contables.**
