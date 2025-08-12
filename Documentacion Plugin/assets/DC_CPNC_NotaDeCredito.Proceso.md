# Nombre del Plugin

DC_CPNC_NotaDeCredito.Proceso

# Acción

Update (Post-Operation)

# Entidad Principal

dc_notadecredito

# Descripción General

Este plugin se ejecuta automáticamente al actualizar una Nota de Crédito
(dc_notadecredito). Su objetivo es asignar automáticamente el monto de
nota de crédito (dc_montonc) a los productos relacionados de cada área
(ventas, embarque, aduana, transporte), asegurando que el valor se
traslade y registre correctamente en la transacción original que generó
la Nota de Crédito.

# Lógica Interna Principal

**1. Prevención de Recursividad\**
 - El plugin ejecuta su lógica solo si la profundidad de contexto es 1
(evita loops infinitos).\
**2. Recuperación de Productos de Nota de Crédito\**
 - Busca todos los productos (dc_productosnc) relacionados a la nota de
crédito y que tengan transacción asociada.\
**3. Separación de Productos por Área\**
 - Clasifica los productos según el valor de su campo dc_departamento en
cuatro listas: ventas, embarque, aduana, transporte.\
- Lanza excepción si algún producto no tiene área definida.\
**4. Actualización de Productos en sus Áreas\**
 - Para cada producto de la nota:\
- Determina la entidad destino según el área/departamento.\
- Busca el producto original en la entidad correspondiente usando el id
de transacción.\
- Actualiza el campo dc_montonc con el monto de la nota de crédito
(dc_total).

# Validaciones Incluidas

\- Verifica que cada producto tenga departamento/área definida.\
- Lanza excepción clara si un área o departamento no es reconocido.\
- Si la transacción no está definida, omite la actualización para ese
producto.

# Entidades Involucradas

\- dc_notadecredito (nota de crédito principal)\
- dc_productosnc (productos de la nota de crédito)\
- dc_productosdeventas (productos de ventas originales)\
- dc_productosdeembarque (productos de embarque originales)\
- dc_productosaduana (productos de aduana originales)\
- dc_productostransporte (productos de transporte originales)

# Métodos Auxiliares Importantes

\- ObtenerProductosNC: Recupera todos los productos de la nota de
crédito con transacción asociada.\
- SepararProductos: Clasifica productos en listas por
área/departamento.\
- ObtenerProductosEPE: Busca el producto original en la entidad
correspondiente usando el id o transacción.\
- GetAreaEntityName: Determina el nombre de la entidad destino según el
valor de departamento.

# Manejo de Errores

\- Lanza excepciones informativas si el departamento o área no es
reconocido o si un producto carece de área.\
- Registra los errores en el servicio de trazas (tracingService) para
facilitar el soporte y la depuración.\
- Lanza el error sin modificar para visibilidad en CRM.

# Consideraciones Finales

\- El plugin automatiza la transferencia del monto de nota de crédito a
los productos originales de cada área, minimizando errores manuales y
garantizando integridad de datos.\
- Permite la extensión a nuevas áreas o departamentos en el futuro.\
- Facilita la trazabilidad y gestión eficiente de notas de crédito y
productos afectados.

# Resumen

Este plugin transfiere automáticamente el monto de la nota de crédito a
los productos originales asociados, clasificando por área y asegurando
que cada producto involucrado reciba el valor correcto, manteniendo la
integridad y trazabilidad de la información en el proceso contable y
operativo.
