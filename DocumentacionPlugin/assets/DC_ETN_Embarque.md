# Documentación funcional del plugin DC_ETN_Embarque

## Descripción general

El plugin DC_ETN_Embarque tiene como objetivo mantener actualizado el estado del campo “dc_tienenietos” en un registro padre de embarque, según la existencia de hijos (nietos) asociados. Este proceso se realiza de manera automática al ejecutarse el plugin, garantizando la integridad de las relaciones jerárquicas entre embarques.

## Propósito

El propósito de este plugin es reflejar en el registro padre si tiene o no hijos registrados. De esta forma, el sistema puede utilizar este campo para procesos de negocio, validaciones o visualizaciones que dependan de la estructura jerárquica de los embarques.

## Flujo general

1. **Recuperación del embarque actual:**  
   El plugin inicia recuperando el registro de embarque sobre el cual se ejecuta, asegurándose de obtener el campo que vincula este registro con su padre (campo “dc_nietos”).

2. **Gestión de errores en la recuperación:**  
   Si ocurre algún error al recuperar el embarque, el error se almacena y el proceso finaliza inmediatamente, evitando impactos en el flujo global.

3. **Obtención del registro padre:**  
   Se extrae la referencia al registro padre desde el campo “dc_nietos” del embarque actual. Si no existe referencia al padre, el proceso termina.

4. **Recuperación de los hijos del padre:**  
   Si hay padre, el plugin busca todos los registros de embarque que tienen como padre al registro identificado. Si ocurre un error en esta consulta, el error se almacena y el proceso se detiene.

5. **Actualización del campo “dc_tienenietos” en el padre:**  
   - Si el padre tiene uno o más hijos, se actualiza el campo “dc_tienenietos” a verdadero.
   - Si el padre no tiene hijos, se actualiza el campo a falso.
   Si ocurre un error durante la actualización, el error se almacena pero el proceso no se detiene.

6. **Manejo robusto de errores:**  
   Todos los errores encontrados durante cualquiera de los pasos anteriores se almacenan en una lista interna, lo cual permite el diagnóstico y análisis posterior sin interrumpir la operación principal del sistema.

## Consideraciones adicionales

- El plugin es seguro ante errores: cualquier excepción se registra internamente y, dependiendo del punto del flujo, puede detener el proceso o simplemente dejar constancia sin interrumpir la ejecución.
- El campo “dc_tienenietos” es fundamental para identificar la existencia de jerarquía en los registros de embarques, facilitando el desarrollo de reglas de negocio dependientes de la estructura familiar de los embarques.
- La lógica es fácilmente adaptable a otros escenarios similares donde se requiera mantener actualizado un campo booleano en función de la existencia de registros relacionados.

## Resumen

El plugin DC_ETN_Embarque automatiza la actualización del campo “dc_tienenietos” en registros padre de embarque, asegurando que este campo refleje correctamente la existencia de hijos (nietos) asociados. La gestión de errores es robusta y no afecta la operación principal, garantizando la integridad y confiabilidad de la información jerárquica en Dyncorp.