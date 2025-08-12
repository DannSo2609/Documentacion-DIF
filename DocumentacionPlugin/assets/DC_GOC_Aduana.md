# Documentación funcional del plugin DC_GOC_Aduana

## Descripción general

El plugin DC_GOC_Aduana automatiza la gestión de órdenes de compra a partir de los productos de aduana registrados en el sistema. Su función es crear o actualizar órdenes de compra para cada combinación de suplidor y moneda encontrada en los productos de aduana de un expediente, garantizando que los datos estén completos y sean consistentes.

## Propósito

El propósito principal de este plugin es optimizar y asegurar la correcta creación y actualización de órdenes de compra vinculadas a los productos de aduana, facilitando la operación logística y administrativa en el módulo de aduana del sistema.

## Flujo general

1. **Condición de ejecución:**  
   El plugin se activa sólo cuando el campo que indica la existencia de orden de compra está marcado en el registro de aduana.

2. **Recuperación de datos esenciales:**  
   Se recuperan los datos clave del registro de aduana: expediente, consignatario, cotización, consolidado, tipo de registro, tipo de operación, HBL, HBL no ruteado y el identificador único de la transacción. Si falta el expediente, el proceso se detiene con un mensaje claro.

3. **Validación y agrupamiento de productos:**  
   Se recuperan todos los productos de aduana asociados al expediente, filtrando sólo aquellos que:
   - No han sido asignados aún a una orden de compra,
   - Tienen un costo unitario mayor a cero,
   - No están desactivados.
   
   Se valida que cada producto tenga los campos obligatorios: producto, moneda de compra y suplidor. Si falta alguno, el proceso se detiene con un mensaje descriptivo.  
   Los productos se agrupan por suplidor y por moneda, generando dos mapas clave para el procesamiento posterior.

4. **Búsqueda de órdenes de compra existentes:**  
   Se consultan las órdenes de compra asociadas al cliente y expediente, que aún no han sido integradas a Dynamics y pertenecen a la unidad de negocio correspondiente.

5. **Actualización de órdenes existentes:**  
   Para cada combinación de suplidor y moneda existente en las órdenes de compra, se agregan los productos correspondientes.  
   Cada producto añadido también actualiza su estado para indicar que ya está asignado a una orden de compra.

6. **Creación de nuevas órdenes de compra:**  
   Para todas las combinaciones de suplidor y moneda que no existen en órdenes de compra previas, se crea una nueva orden con los productos asociados.  
   Se asignan todos los campos relevantes y, si corresponde, se asocian los datos de HBL.  
   Los productos añadidos también actualizan su estado.

7. **Cálculo de totales:**  
   Tras cada creación o actualización de orden de compra, se marca el campo correspondiente para que el sistema recalcule los totales de la orden.

8. **Gestión de errores:**  
   Todos los errores no críticos (como problemas al crear o actualizar órdenes o productos) se almacenan en una lista interna de errores, permitiendo que el proceso continúe para los demás registros.  
   Los errores críticos (como falta de datos obligatorios) detienen el flujo y muestran mensajes claros al usuario.

9. **Notificaciones (opcional, comentado):**  
   El código incluye, pero mantiene comentada, la posibilidad de generar notificaciones visuales para el usuario cuando se crean o actualizan órdenes de compra exitosamente.

## Consideraciones adicionales

- El plugin es robusto ante errores y asegura la integridad de la información antes de realizar cualquier acción sobre las órdenes de compra.
- Permite la gestión eficiente de órdenes de compra con múltiples suplidores y monedas, ajustándose automáticamente a los datos reales de los productos de aduana.
- Facilita la trazabilidad y el control sobre el estado de asignación de cada producto en el flujo de aduana.

## Resumen

El plugin DC_GOC_Aduana automatiza la creación y actualización de órdenes de compra en base a los productos de aduana registrados, agrupando por suplidor y moneda, validando la integridad de los datos, y asegurando que todos los productos queden correctamente relacionados y su estado refleje la realidad administrativa de la operación. La gestión de errores es robusta y no afecta el flujo global, garantizando una experiencia fluida y segura para el usuario.