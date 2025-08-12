**Documentación del Plugin: Creación Automática de Transporte desde
Embarque**

**Nombre del Plugin**

**DC_CRT_Embarque.Proceso**

**Acción**

- **Update (Post-Operation)**

**Entidad Principal**

- **dc_embarques**

**Descripción General**

**Este plugin se ejecuta automáticamente cuando se actualiza un registro
de embarque (dc_embarques) y el campo booleano dc_creartransporte está
marcado como verdadero.**

**Su función principal es crear un nuevo registro de transporte
(dc_transporte) asociado a ese embarque, copiando y replicando la
información relevante del embarque y sus entidades relacionadas como
aduana, documentos de operaciones, entre otros.**

**Lógica Interna Principal**

1.  **Prevención de Recursividad**

    - **El plugin solo ejecuta su lógica si la profundidad de contexto
      es 1 para evitar ejecuciones múltiples.**

2.  **Validación de Condición**

    - **Solo se ejecuta si el campo dc_creartransporte está activo en el
      embarque.**

3.  **Recuperación del Registro de Embarque**

    - **Recupera el registro completo del embarque y sus campos
      relacionados.**

4.  **Obtención de Entidades Relacionadas**

    - **Recupera el registro de aduana asociado para obtener información
      adicional.**

5.  **Creación del Registro de Transporte**

    - **Crea un nuevo registro dc_transporte copiando campos generales,
      origen y destino, datos del embarque, gestores, medidas de peso y
      volumen, detalles sobre la carga, y datos adicionales.**

6.  **Relación con Documentos de Operaciones**

    - **Busca los documentos de operaciones relacionados al embarque y
      actualiza su referencia al nuevo transporte creado.**

7.  **Actualización del Embarque y Aduana**

    - **Actualiza el registro de embarque y el de aduana (si existe)
      para relacionarlos con el nuevo transporte.**

**Validaciones Incluidas**

- **Verifica la existencia del campo dc_creartransporte y su valor.**

- **Asegura que el embarque y aduana relacionados existan antes de hacer
  las copias.**

- **Control de errores con trazabilidad para facilitar debugging.**

**Entidades Involucradas**

- **dc_embarques (registro principal que activa el plugin)**

- **dc_transporte (registro que se crea)**

- **dc_aduanas (entidad relacionada para datos adicionales)**

- **dc_documentosoperaciones (registros relacionados que se
  actualizan)**

**Métodos y Cálculos Importantes**

- **Uso de RetrieveRequest para obtener el registro completo del
  embarque.**

- **Creación dinámica de un nuevo registro con copiado de campos
  clave.**

- **Consulta y actualización masiva de documentos relacionados con
  QueryExpression.**

**Manejo de Errores**

- **Registra mensajes de error en ITracingService.**

- **Propaga excepciones para asegurar visibilidad en el sistema CRM.**

**Consideraciones Finales**

- **Automatiza la generación y vinculación de transporte desde un
  embarque, agilizando el proceso operativo.**

- **Reduce errores manuales y mantiene integridad en las relaciones
  entre embarque, transporte, aduana y documentos.**

- **Fácilmente ampliable para agregar más campos o lógicas adicionales
  según negocio.**

**✅ Resumen**

**El plugin DC_CRT_Embarque.Proceso crea automáticamente un registro de
transporte basado en la información del embarque cuando el campo
dc_creartransporte está activo, y mantiene la relación con documentos y
entidades relacionadas para una gestión integral y automatizada.**
