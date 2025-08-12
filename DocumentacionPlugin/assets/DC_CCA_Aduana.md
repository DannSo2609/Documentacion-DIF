# Documentación del Plugin `DC_CCA_Aduana`

## Descripción General

El plugin DC_CCA_Aduana forma parte de la solución Dynacorp y está diseñado para ejecutarse en el contexto de la entidad de Aduanas. Su función principal es asociar automáticamente los contenedores correspondientes a un registro de Aduana, a partir de entidades relacionadas como Consolidado, Embarque y Transporte. Este proceso asegura que la información de los contenedores esté actualizada y sin duplicados, facilitando la gestión logística y aduanera.

---

## Funcionamiento del Plugin

### 1. Control de Ejecución

El plugin está programado para ejecutarse solo una vez por pipeline, verificando la profundidad de ejecución (`Depth <= 1`). Esto previene ejecuciones múltiples innecesarias que podrían causar inconsistencias o recursividad.

### 2. Identificación de Entidades Asociadas

Al inicio de la ejecución, el plugin identifica si el registro de Aduana tiene relaciones con las siguientes entidades:
- Consolidado
- Embarque
- Transporte

Estas entidades son claves porque pueden tener asociados contenedores relevantes para la Aduana en cuestión.

### 3. Gestión y Clasificación de Contenedores

El plugin utiliza diferentes estructuras para organizar los contenedores durante la ejecución:
- **Contenedores Existentes:** Lista de identificadores (IDs) de contenedores que ya están asociados a la Aduana actual.
- **Contenedores por Crear:** Lista de IDs de contenedores que deben asociarse a la Aduana, provenientes de Consolidado, Embarque y Transporte.
- **Referencias de Contenedores:** Colección de referencias a los contenedores nuevos que serán asociados.

### 4. Búsqueda de Contenedores

El plugin realiza búsquedas específicas según las relaciones encontradas:
- **Contenedores ya asociados a la Aduana:** Consulta la relación actual y extrae los IDs.
- **Contenedores asociados al Consolidado:** Extrae los IDs de los contenedores relacionados al Consolidado, si existe.
- **Contenedores asociados al Embarque:** Extrae los IDs de los contenedores relacionados al Embarque, si existe.
- **Contenedores asociados al Transporte:** Extrae los IDs de los contenedores relacionados al Transporte, si existe.

Cada búsqueda verifica que los contenedores identificados no estén ya en la lista de "por crear", para evitar duplicados desde distintas fuentes.

### 5. Prevención de Duplicados

Antes de asociar los contenedores a la Aduana, el plugin valida que estos no estén ya registrados como asociados, asegurando la integridad de la información y evitando asociaciones repetidas.

### 6. Asociación de Contenedores

Una vez identificados los contenedores nuevos, el plugin realiza la asociación utilizando la relación definida entre Aduana y Contenedores en Dynacorp. Así, los nuevos contenedores quedan vinculados automáticamente al registro de Aduana.

### 7. Manejo de Errores

El plugin implementa manejo de errores no críticos, capturando cualquier excepción que ocurra durante las búsquedas o asociaciones. Estos errores se almacenan en una lista interna, permitiendo su revisión posterior sin interrumpir el proceso principal.

### 8. Notificaciones (Opcional)

Existe una sección comentada en el código para la generación de notificaciones en la aplicación. Puede ser utilizada para informar al usuario sobre la cantidad de contenedores encontrados y asociados, facilitando la trazabilidad de los cambios.

---

## Consideraciones y Buenas Prácticas

- **Optimización de Consultas:** El plugin realiza consultas focalizadas para minimizar el consumo de recursos y el tiempo de respuesta.
- **Evita Duplicados:** Garantiza que no se asocien contenedores repetidos a la misma Aduana.
- **Gestión de Excepciones:** Captura y almacena errores para mantener la robustez del proceso.
- **Escalabilidad:** Su arquitectura permite agregar nuevas relaciones o criterios de asociación según evolucionen los requerimientos de Dynacorp.

---

## Uso y Configuración

- El plugin debe ser registrado en la entidad de Aduanas, preferiblemente en los eventos de creación o actualización.
- Es indispensable que las relaciones y atributos utilizados existan y estén correctamente configurados en el modelo de datos de Dynacorp.
- Para activar notificaciones, se debe adaptar y descomentar la sección correspondiente en el código.

---

## Personalización

- **Notificaciones:** Se puede personalizar la lógica de notificación para alertar a los usuarios sobre eventos importantes.
- **Nuevas Entidades:** Se pueden agregar búsquedas o asociaciones adicionales siguiendo la lógica ya implementada.
- **Auditoría de Errores:** La lista de errores internos puede ser utilizada para implementar sistemas de monitoreo o alertas.

---

## Resumen

El plugin `DC_CCA_Aduana` de Dynacorp automatiza la actualización y asociación de contenedores relacionados a un registro de Aduana, asegurando información precisa y evitando duplicidades. Su diseño modular y robusto lo hace adaptable a futuras necesidades operativas y funcionales dentro de la solución Dynacorp.