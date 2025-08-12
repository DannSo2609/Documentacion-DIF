# Documentación funcional del plugin DC_PC_Consolidado

## Sistema: Dynacorp

## Descripción general

El plugin **DC_PC_Consolidado** para Dynacorp automatiza el procesamiento de cargos a partir de las liquidaciones asociadas a un consolidado. Se encarga de validar la existencia de homologaciones para los productos de liquidación, asociar correctamente los productos internos y las listas de precios, y crear los cargos consolidados necesarios, manteniendo la integridad y trazabilidad de la información.

---

## Propósito

Facilitar y controlar el proceso de generación de cargos consolidados para cada liquidación registrada en un consolidado, asegurando que todos los datos sean consistentes, estén homologados y se cumplan las reglas de negocio establecidas para la facturación y el control logístico.

---

## Flujo general

1. **Condición de ejecución:**  
   El plugin se ejecuta sólo si el campo de control para procesar cargos (`dc_procesarcargos`) está activado en el consolidado.

2. **Recuperación y validación del consolidado:**  
   Se recupera el registro de consolidado con todos sus campos.  
   Si el consolidado no tiene agente asignado, el proceso se detiene y se muestra un mensaje claro.

3. **Obtención de liquidaciones asociadas:**  
   Se buscan todas las liquidaciones asociadas al consolidado.  
   Si ocurre algún error durante la consulta, el proceso se detiene y se almacena el error para diagnóstico.

4. **Procesamiento de cada liquidación:**  
   Para cada liquidación:
   - Se recupera la homologación correspondiente según la descripción del producto de liquidación.  
     - Si no existe, o hay duplicados, el proceso se detiene para esa liquidación y se informa el error.
   - Se valida el costo de la liquidación (debe ser distinto de cero).
   - Se obtienen las listas de precios que coinciden con los parámetros del consolidado (agente, origen, destino, tipo de embarque/transporte, unidad de negocio).
   - Se valida que exista un producto interno en la lista de precios homologada.
   - Se obtiene el embarque asociado a la liquidación por medio del HBL.
   - Se crea el registro de cargo consolidado con todos los datos relacionados (consolidado, embarque, suplidor, lista de precios, producto, cantidad y precio).

5. **Gestión de errores:**  
   Todos los errores no críticos (como problemas con homologaciones, productos o listas de precios) se almacenan internamente y no detienen el procesamiento de otras liquidaciones.  
   Los errores críticos (como la ausencia de agente o errores al recuperar datos esenciales) detienen el flujo y muestran mensajes informativos.

6. **Notificaciones (comentado):**  
   El código incluye bloques comentados para notificar al usuario acerca del éxito del proceso mediante notificaciones visuales en Dynacorp, aunque esta funcionalidad está deshabilitada por defecto.

---

## Consideraciones adicionales

- El plugin valida la existencia y unicidad de las homologaciones para cada producto de liquidación, asegurando la correcta correspondencia con productos internos.
- Solo se procesan liquidaciones con un balance distinto de cero.
- Gestiona los estados y relaciones de los cargos de manera robusta, permitiendo la extensión y adaptación a futuros requerimientos del negocio.
- La arquitectura del proceso facilita la trazabilidad y el control de errores, permitiendo análisis y mejoras continuas.

---

## Resumen

El plugin **DC_PC_Consolidado** automatiza el procesamiento de cargos a partir de liquidaciones en un consolidado en Dynacorp. Garantiza la correcta homologación de productos, la selección adecuada de listas de precios y la creación fiable de cargos consolidados, mejorando la eficiencia, la calidad de los datos y la trazabilidad operativa en la plataforma.