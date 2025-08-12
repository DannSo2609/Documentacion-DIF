**Documentación del Plugin: Cálculo de Totales en Contenedor**

**Nombre del Plugin**

**DC_CTE_Contenedores.Proceso**

**Acción**

- **Update (Post-Operation)**

**Entidad Principal**

- **dc_contenedores**

**Descripción General**

**Este plugin se ejecuta automáticamente al actualizar un registro de
contenedor. Su propósito es calcular los totales de peso, piezas y
volumen basándose en los registros de distribución
(dc_distribucioncontenedores) relacionados, siempre y cuando el campo
booleano dc_calculartotaleshbl esté marcado como verdadero.**

**Lógica Interna Principal**

1.  **Prevención de Recursividad**

    - **Se ejecuta solo si la profundidad de ejecución es 1 o menor.**

2.  **Validación del Flag de Ejecución**

    - **Solo realiza cálculos si dc_calculartotaleshbl = true.**

3.  **Consulta de Distribuciones Asociadas**

    - **Busca todas las distribuciones (dc_distribucioncontenedores)
      vinculadas al contenedor actual.**

4.  **Sumatoria de Campos**

    - **Suma los campos dc_peso, dc_piezas y dc_volumen de los registros
      encontrados.**

5.  **Actualización del Contenedor**

    - **Actualiza los campos del contenedor:**

      - **dc_totalpesohbl**

      - **dc_totalpiezashbl**

      - **dc_totalvolumen**

6.  **Validación de Existencia de Distribuciones**

    - **Si no se encuentran distribuciones asociadas, lanza una
      excepción para alertar al usuario.**

**Validaciones Incluidas**

- **Verifica que dc_calculartotaleshbl esté en true.**

- **Comprueba que existan registros relacionados antes de hacer
  cálculos.**

- **Lanza un error si no hay datos para procesar.**

**Entidades Involucradas**

- **dc_contenedores (contenedor principal)**

- **dc_distribucioncontenedores (detalles que alimentan los totales)**

**Métodos y Cálculos Importantes**

- **Suma valores de múltiples registros usando foreach.**

- **Condición Contains para asegurar que los campos existan antes de
  sumarlos.**

- **Actualización en bloque del contenedor padre con los resultados.**

**Manejo de Errores**

- **Registra errores en tracingService.**

- **Lanza InvalidPluginExecutionException si no hay registros para
  procesar.**

**Consideraciones Finales**

- **Este plugin automatiza los totales por contenedor con base en sus
  distribuciones.**

- **Reduce errores manuales y mantiene actualizada la información para
  embarques y logística.**

- **Se puede extender fácilmente para incluir más campos o tipos de
  cálculos.**

**✅ Resumen**

**El plugin DC_CTE_Contenedores.Proceso calcula y actualiza
automáticamente los totales de peso, piezas y volumen de un contenedor
en base a sus distribuciones asociadas, asegurando integridad de datos
en los procesos de embarque y logística.**
