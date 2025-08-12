# Documentación funcional del plugin DC_GOC_Transporte

## Sistema: Dynacorp

## Descripción general

El plugin **DC_GOC_Transporte** está diseñado para el sistema Dynacorp y automatiza la creación y actualización de órdenes de compra a partir de los productos asociados a un registro de transporte. Gestiona la asignación de productos a órdenes, agrupando por suplidor y moneda, garantizando la validación de datos críticos y la adecuada actualización de estados y totales.

---

## Propósito

Optimizar y controlar la generación de órdenes de compra derivadas de los productos registrados en un transporte, manteniendo la trazabilidad, la integridad y el estado actualizado de cada producto y orden. El plugin reduce errores manuales y mejora la eficiencia en la gestión logística y de compras.

---

## Flujo general

1. **Condición de ejecución:**  
   El plugin se ejecuta únicamente si el campo de control para órdenes de compra (`dc_tieneordendecompra`) está activado en el transporte.

2. **Recuperación y validación del transporte:**  
   Se recuperan todos los datos clave del transporte: expediente, consignatario, cotización, consolidado, tipo de registro, tipo de operación, HBL, HBL no ruteado y el identificador único.  
   Si falta el expediente, el proceso se detiene y muestra un mensaje informativo.

3. **Consulta y validación de productos:**  
   Se buscan todos los productos asociados al transporte que:
   - No han sido aún asignados a una orden de compra,
   - Tienen costo unitario mayor a cero y están activos.
   Cada producto es validado para asegurar que tenga todos los campos obligatorios: producto, moneda de compra y suplidor. Si falta alguno, se detiene y se informa el motivo.

4. **Agrupamiento por suplidor y moneda:**  
   Los productos se agrupan en estructuras internas por suplidor y moneda, preparando todas las combinaciones necesarias para la generación o actualización de órdenes de compra.

5. **Actualización de órdenes existentes:**  
   Se buscan las órdenes de compra ya existentes que correspondan al cliente, expediente y unidad de negocio.  
   Si una combinación de suplidor y moneda ya tiene orden, se actualiza añadiendo los productos correspondientes y se marca el cálculo de totales.

6. **Creación de nuevas órdenes de compra:**  
   Para las combinaciones de suplidor y moneda que no tienen orden existente, se crea una nueva orden, se agregan los productos, y se actualiza el estado de los productos y la orden para reflejar la correcta asignación.

7. **Gestión de errores:**  
   Todos los errores no críticos (como fallos en la actualización de una orden o producto) se almacenan en una lista interna, permitiendo que el proceso siga con los demás productos. Los errores críticos (ausencia de datos obligatorios) detienen el proceso y muestran mensajes concretos.

8. **Notificaciones (comentado):**  
   El código incluye bloques comentados para notificar al usuario sobre la creación o actualización de órdenes de compra mediante notificaciones visuales en Dynacorp.

---

## Consideraciones adicionales

- El plugin valida que todos los productos tengan los datos necesarios antes de procesar, evitando inconsistencias en las órdenes de compra.
- Gestiona el estado de los productos de transporte para indicar su correcta asignación a una orden.
- Marca siempre el cálculo de totales pendiente tras cada operación para mantener la integridad financiera y logística.
- El diseño es robusto, preparado para la extensión y adaptaciones futuras.

---

## Resumen

El plugin **DC_GOC_Transporte** es clave para la automatización y control de la generación de órdenes de compra a partir de productos de transporte en Dynacorp. Su lógica robusta y validaciones aseguran eficiencia, trazabilidad y calidad de datos en los procesos de logística y compras de la plataforma.