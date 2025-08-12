# Documentación funcional del plugin DC_SR_EmbarquesHijos

## Sistema: Dynacorp

## Descripción general

El plugin **DC_SR_EmbarquesHijos** para Dynacorp automatiza la actualización de los totales de peso y volumen del embarque padre, sumando los valores de todos los embarques hijos asociados a través del campo `dc_nietos`. Los campos actualizados incluyen toneladas, kilogramos, libras, metros cúbicos, pies cúbicos, centímetros cúbicos y cantidad de piezas.

---

## Propósito

Facilitar y asegurar la consolidación automática de los datos de peso y volumen en el embarque padre, evitando errores manuales y manteniendo la información centralizada y actualizada para análisis logístico y operativo.

---

## Flujo general

1. **Condición de ejecución:**  
   El plugin se ejecuta únicamente si la profundidad del pipeline es igual o menor a 1.

2. **Recuperación y validación del embarque:**  
   Se recupera el registro de embarque y se valida que tenga asignado un padre (campo `dc_nietos`).  
   Si no tiene padre, el proceso finaliza.

3. **Obtención de embarques hijos relacionados:**  
   Se consultan todos los embarques cuyo campo `dc_nietos` coincida con el padre.  
   Para cada embarque hijo, se suman los valores de toneladas, kilogramos, libras, metros cúbicos, pies cúbicos, centímetros cúbicos y cantidad de piezas.

4. **Actualización del embarque padre:**  
   Se actualiza el registro del embarque padre con los nuevos totales acumulados.  
   Si ocurre un error durante la actualización, se almacena para diagnóstico pero el proceso continúa para el resto de los embarques.

5. **Gestión de errores:**  
   Todos los errores no críticos (errores al procesar individualmente un embarque hijo o al actualizar el padre) se almacenan en una lista interna, permitiendo la continuidad del proceso.

6. **Notificaciones (comentado):**  
   El código incluye un bloque comentado para generar una notificación visual al usuario en Dynacorp cuando el embarque padre es actualizado exitosamente, aunque esta funcionalidad está deshabilitada por defecto.

---

## Consideraciones adicionales

- El plugin asegura que únicamente se sumen los valores de los embarques hijos directos del padre, evitando duplicidad en las sumas.
- Los campos actualizados cubren todas las métricas de peso y volumen relevantes para la operación logística.
- Es robusto ante errores y permite la trazabilidad de fallos a través de la lista interna de excepciones.
- El diseño es extensible para incorporar nuevas métricas o reglas de negocio en el futuro.

---

## Resumen

El plugin **DC_SR_EmbarquesHijos** automatiza la actualización de totales de peso y volumen en el embarque padre de Dynacorp, sumando los datos de los embarques hijos y manteniendo la información centralizada, precisa y actualizada para la operación logística y administrativa.