# Documentación funcional del plugin DC_GOC_Embarque

## Sistema: Dynacorp

## Descripción general

El plugin **DC_GOC_Embarque** automatiza en Dynacorp la creación y actualización de órdenes de compra a partir de los productos de embarque de un registro de embarque específico. Gestiona la asignación de productos a órdenes, agrupando por suplidor y moneda, y asegura que se validen todos los datos críticos antes de procesar.

---

## Propósito

Optimizar y controlar la generación de órdenes de compra derivadas de los productos registrados en un embarque, manteniendo la trazabilidad, la integridad y el estado actualizado de cada producto y orden. El plugin ayuda a reducir errores manuales y mejorar la eficiencia operativa en la gestión de embarques y compras.

---

## Flujo general

1. **Condición de ejecución:**  
   El plugin se ejecuta únicamente si el campo de control para órdenes de compra (`dc_tieneordendecompra`) está activado en el embarque.

2. **Recuperación y validación del embarque:**  
   Se recuperan todos los datos clave del embarque: expediente, consignatario, cotización, consolidado, tipo de operación y HBL.  
   Si falta el expediente, el proceso se detiene con un mensaje claro.

3. **Consulta y validación de productos:**  
   Se buscan los productos del embarque asociados al registro actual que:
   - No han sido aún asignados a una orden de compra,
   - No son cargos consolidados,
   - Tienen costo unitario mayor a cero y están activos.
   Cada producto es validado para asegurar que tenga todos los campos obligatorios: producto, moneda de compra y suplidor. Si falta alguno, se detiene y se informa el motivo.

4. **Agrupamiento por suplidor y moneda:**  
   Los productos se agrupan en mapas internos por suplidor y moneda, preparando las combinaciones necesarias para la generación o actualización de órdenes de compra.

5. **Actualización de órdenes existentes:**  
   Se buscan las órdenes de compra ya existentes que correspondan al cliente, expediente y unidad de negocio.  
   Si una combinación de suplidor y moneda ya tiene una orden, se actualiza añadiendo los productos correspondientes, y se marca el cálculo de totales.

6. **Creación de nuevas órdenes de compra:**  
   Para las combinaciones de suplidor y moneda que no tienen orden existente, se crea una nueva orden, se agregan los productos, y se actualiza el estado de los productos y la orden para reflejar la correcta asignación y cálculo de totales.

7. **Gestión de errores:**  
   Todos los errores no críticos (como fallos en la actualización de una orden o producto) se almacenan en una lista interna, permitiendo que el proceso siga con los demás productos. Errores críticos (ausencia de datos obligatorios) detienen el proceso y muestran mensajes concretos.

8. **Notificaciones (comentado):**  
   El código incluye la posibilidad, actualmente comentada, de notificar al usuario sobre la creación o actualización de órdenes de compra mediante notificaciones visuales en Dynacorp.

---

## Consideraciones adicionales

- El plugin asegura que todos los productos tengan los datos necesarios antes de procesar, evitando inconsistencias en las órdenes de compra.
- Gestiona el estado de los productos de embarque para indicar su correcta asignación a una orden.
- Marca siempre el cálculo de totales pendiente tras cada operación para mantener la integridad financiera y logística.
- Puede adaptarse fácilmente a futuros requerimientos de negocio o integraciones adicionales.

---

## Resumen

El plugin **DC_GOC_Embarque** es clave para la automatización y control de la generación de órdenes de compra a partir de productos de embarque en Dynacorp. Su lógica robusta y validaciones aseguran eficiencia, trazabilidad y calidad de datos en los procesos de logística y compras de la plataforma.