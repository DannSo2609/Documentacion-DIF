# Documentación funcional del plugin DC_GOC_ProductosAColectar

## Sistema: Dynacorp

## Descripción general

El plugin **DC_GOC_ProductosAColectar** automatiza en Dynacorp la creación y actualización de órdenes de compra a partir de los productos a colectar asociados a registros de aduana, embarque o transporte. Su lógica central permite agrupar productos según suplidor y moneda, validar datos críticos y mantener la trazabilidad y el control del estado de cada producto y orden generada.

---

## Propósito

Optimizar y controlar la generación de órdenes de compra derivadas de los productos a colectar, indistintamente si provienen de una aduana, un embarque o un transporte. El plugin permite que todos los productos válidos se asignen correctamente a sus órdenes, dejando trazabilidad y actualizando los estados necesarios para la operación logística y administrativa.

---

## Flujo general

1. **Identificación del registro padre:**  
   El plugin identifica si el producto a colectar está relacionado con una aduana, un embarque o un transporte. Según el caso, recupera los datos clave del registro padre, como expediente, consignatario, cotización, consolidado y tipo de operación.

2. **Validación de expediente:**  
   Si el registro padre no tiene expediente, el proceso se detiene y se lanza un mensaje claro al usuario.

3. **Consulta y validación de productos a colectar:**  
   Se buscan todos los productos a colectar asociados al registro padre (aduana, embarque o transporte) que:
   - No han sido asignados aún a una orden de compra,
   - Tienen un costo mayor a cero y están activos.
   Para cada producto, se valida la existencia obligatoria de los campos: producto, moneda de compra y suplidor. Si falta alguno, se detiene el proceso con un mensaje descriptivo.

4. **Agrupamiento por suplidor y moneda:**  
   Los productos se agrupan en estructuras internas por suplidor y moneda, preparando todas las combinaciones necesarias para la generación o actualización de órdenes de compra.

5. **Actualización de órdenes existentes:**  
   Se buscan las órdenes de compra ya existentes que correspondan al expediente, cliente y unidad de negocio.  
   Si una combinación de suplidor y moneda ya tiene una orden, se actualiza añadiendo los productos correspondientes y se marca el cálculo de totales.

6. **Creación de nuevas órdenes de compra:**  
   Para las combinaciones de suplidor y moneda que no tienen orden existente, se crea una nueva orden, se agregan los productos, y se actualiza el estado de los productos y la orden para reflejar la correcta asignación y el cálculo de totales.

7. **Gestión de errores:**  
   Todos los errores no críticos (fallos de creación o actualización) se capturan internamente, permitiendo que el proceso continúe con los demás productos. Los errores críticos (ausencia de datos obligatorios) detienen el proceso y muestran mensajes explicativos.

8. **Notificaciones (comentado):**  
   El código incluye (comentada) la posibilidad de notificar al usuario sobre la creación o actualización de órdenes de compra mediante notificaciones visuales en Dynacorp.

---

## Consideraciones adicionales

- El plugin asegura que todos los productos a colectar tengan los datos necesarios antes de procesar, evitando inconsistencias en las órdenes de compra.
- Gestiona el estado de los productos para indicar su correcta asignación a una orden.
- Siempre marca el cálculo de totales pendiente tras cada operación para mantener la integridad financiera y logística.
- Permite la extensión y adaptación a nuevos requerimientos de negocio o integraciones adicionales.

---

## Resumen

El plugin **DC_GOC_ProductosAColectar** es clave para la automatización y control de la generación de órdenes de compra a partir de productos a colectar, sin importar el origen (aduana, embarque o transporte) en Dynacorp. Su lógica robusta y validaciones aseguran eficiencia, trazabilidad y calidad de datos, facilitando los procesos logísticos y administrativos de la plataforma.