**Documentación del Plugin: Cálculo de Peso Volumétrico en Productos**

**Nombre del Plugin**

**DC_CPV_Entidades.Proceso**

**Acción**

- **Update (Post-Operation)**

**Entidad Principal**

- **dc_productos (u otra entidad que invoque el plugin; depende del
  modelo exacto)**

**Descripción General**

**Este plugin se ejecuta automáticamente cuando se actualiza un producto
o entidad relacionada con cotización, siempre que el campo
dc_calcularpesovolumetrico esté marcado como verdadero.**

**El objetivo es sumar todos los valores de peso volumétrico
(dc_pesovolumetrico) relacionados al producto actual y actualizar dicho
total directamente en el campo dc_pesovolumetrico del mismo registro.**

**Lógica Interna Principal**

1.  **Prevención de Recursividad**

    - **Solo se ejecuta si la profundidad (context.Depth) es 1.**

2.  **Validación de Condición**

    - **Verifica si el campo booleano dc_calcularpesovolumetrico está
      marcado como verdadero.**

3.  **Recuperación del Registro**

    - **Recupera el producto con el campo dc_medidapeso.**

4.  **Consulta de Pesos Relacionados**

    - **Construye y ejecuta una QueryExpression para buscar todos los
      registros en dc_pesovolumetrico relacionados al producto actual.**

5.  **Cálculo del Total**

    - **Suma todos los valores del campo dc_pesovolumetrico.**

6.  **Actualización del Registro**

    - **Actualiza el campo dc_pesovolumetrico del producto con el total
      calculado.**

**Validaciones Incluidas**

- **Verifica que el campo dc_calcularpesovolumetrico esté activo.**

- **Valida que existan registros relacionados antes de calcular.**

- **Maneja la ausencia del campo dc_pesovolumetrico en algunos
  registros.**

**Entidades Involucradas**

- **Entidad Principal: (probablemente dc_productos o
  dc_productoscotizacion)**

- **dc_pesovolumetrico (entidad relacionada para el cálculo)**

**Métodos y Cálculos Importantes**

- **QueryExpression personalizada: para buscar registros relacionados
  según el ID del producto.**

- **Sumatoria de valores: acumulación segura de decimal con validación
  de existencia de campo.**

**Manejo de Errores**

- **Se utiliza ITracingService para registrar trazas durante el
  proceso.**

- **Lanza excepciones con mensaje claro si no se encuentran registros
  válidos.**

- **Los errores no son suprimidos, se propagan al sistema para que el
  usuario o soporte los vea.**

**Consideraciones Finales**

- **Este plugin automatiza la suma de pesos volumétricos y su registro
  en el producto.**

- **Evita la necesidad de cálculos manuales y reduce errores.**

- **Puede extenderse fácilmente para incluir otras métricas o
  condiciones según reglas de negocio.**

**✅ Resumen**

**El plugin DC_CPV_Cotizacion.Proceso automatiza el cálculo del peso
volumétrico total de un producto al sumar los valores relacionados en la
entidad dc_pesovolumetrico, mejorando la precisión del dato y la
eficiencia del proceso en CRM.**
