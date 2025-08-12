# Documentación funcional del plugin DC_GOC_Cargosconsolidado

## Descripción general

El plugin DC_GOC_Cargosconsolidado es un componente para Dynacorp que automatiza la actualización y sincronización de cargos (consolidados, individuales y depósitos logísticos) vinculados a un consolidado, manteniendo alineados los registros de productos de embarque y las órdenes de compra asociadas. El plugin asegura que cualquier cambio en los cargos o productos se refleje adecuadamente en las órdenes de compra y recalcula los totales donde corresponde.

## Propósito

El propósito de este plugin es mantener la coherencia de la información entre los diferentes registros de cargos y las órdenes de compra relacionadas en el contexto de la gestión logística y administrativa de un consolidado. Automatiza la actualización de datos como cantidades, precios y totales, y recalcula los valores agregados en embarques y órdenes de compra para evitar inconsistencias.

## Flujo general

1. **Condición de ejecución:**  
   El plugin se ejecuta únicamente en la primera profundidad de ejecución del pipeline (Depth <= 1) para evitar ejecuciones recursivas.

2. **Identificación de la entidad y su consolidado relacionado:**  
   Para las entidades de cargos consolidados, cargos individuales y productos de depósitos, se determina el consolidado asociado consultando el campo correspondiente en el registro actual.

3. **Sincronización de productos de embarque:**  
   Si la entidad es un producto de embarque, se buscan los cargos relacionados y, si existen, se actualizan los valores de cantidad y precio en los registros de cargos correspondientes.

4. **Recuperación y validación del consolidado:**  
   Una vez identificado el consolidado, se recuperan todos los datos principales y, si falta algún dato crítico (como el expediente), el proceso lanza una excepción que detiene la ejecución.

5. **Consulta de cargos asociados:**  
   Se consultan los cargos consolidados, individuales y depósitos logísticos asociados al consolidado, junto con las monedas correspondientes para cada tipo de cargo.

6. **Actualización de órdenes de compra:**  
   Para cada tipo de cargo con registros relacionados:
   - Se busca la orden de compra correspondiente, filtrando por cliente, expediente, suplidor y moneda.
   - Si existe una orden, se actualizan los productos de la orden con los datos de los cargos.
   - Se actualizan campos como producto, cantidad, precio, subtotal y total.
   - Tras actualizar los productos, se marca la orden para que el sistema recalcule los totales.
   - Se recalculan también los totales en los embarques relacionados con los cargos.

7. **Gestión de errores:**  
   Todos los errores no críticos que ocurren durante la actualización o consulta de datos se almacenan en una lista interna, permitiendo que el proceso continúe para los demás registros. Solo los errores críticos (como la ausencia de expediente en el consolidado) detienen la ejecución.

8. **Notificaciones (opcional, comentado en el código):**  
   El plugin incluye bloques comentados de código para notificar visualmente al usuario en Dynamics cuando se actualiza una orden de compra, aunque esta funcionalidad está deshabilitada.

## Consideraciones adicionales

- El plugin es robusto frente a errores y asegura que los datos críticos estén presentes antes de ejecutar cualquier acción.
- Evita la recursividad y sobrecarga del sistema controlando la profundidad de ejecución.
- Asegura la sincronización de productos y cargos entre diferentes entidades, manteniendo la integridad de la información en los procesos logísticos y de compras.
- El recalculo de totales tras cada actualización garantiza que los reportes y análisis posteriores sean precisos.

## Resumen

El plugin DC_GOC_Cargosconsolidado  es una herramienta clave para mantener la integridad, coherencia y actualización automática de los datos de cargos y órdenes de compra en los procesos logísticos de Dynamics 365. Automatiza la sincronización de datos críticos, gestiona los errores de forma no intrusiva y facilita la operación eficiente y confiable del sistema.