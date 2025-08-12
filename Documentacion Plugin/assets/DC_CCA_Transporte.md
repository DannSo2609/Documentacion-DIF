# Documentación del Plugin DC_CCA_Transporte

## Descripción General

Este plugin, ubicado en el namespace `DC_CCA_Transporte`, automatiza el proceso de asociación de contenedores a un registro de Transporte. Su objetivo es asegurar que todos los contenedores relevantes, provenientes de entidades relacionadas como Consolidado, Embarque y Aduana, estén correctamente vinculados al Transporte actual, evitando duplicidades y manteniendo la integridad de los datos.

---

## Propósito

El plugin garantiza que, al crear o actualizar un registro de Transporte, se realice automáticamente la asociación de todos los contenedores relacionados a dicho Transporte, tomando en cuenta las siguientes entidades asociadas:
- **Consolidado**
- **Embarque**
- **Aduana**

De esta forma, el usuario no tiene que asociar manualmente los contenedores, optimizando la operación y reduciendo errores humanos.

---

## Funcionamiento Detallado

### 1. Control de Ejecución

- El plugin solo se ejecuta en el primer nivel de profundidad del pipeline (`Depth <= 1`), evitando ejecuciones recursivas o múltiples en la misma transacción.

### 2. Identificación de Entidades Asociadas

- Recupera las referencias asociadas al Transporte actual:
  - Consolidado (`dc_mawbmbl`)
  - Embarque (`dc_hbl`)
  - Aduana (`dc_aduanas`)

### 3. Obtención de Contenedores Existentes

- Obtiene la lista de contenedores que ya están asociados al Transporte actual, consultando la tabla de relación pertinente.

### 4. Búsqueda de Contenedores en Entidades Relacionadas

- Busca contenedores asociados en cada una de las entidades relacionadas:
  - **Consolidado:** Busca todos los contenedores asociados al consolidado, añadiendo aquellos no presentes aún en la lista por crear.
  - **Embarque:** Busca contenedores conectados al embarque, añadiendo solo los nuevos.
  - **Aduana:** Busca contenedores relacionados a la aduana, evitando duplicados.

### 5. Prevención de Duplicados

- Antes de asociar nuevos contenedores al Transporte, el plugin revisa que estos no estén ya asociados, asegurando que cada contenedor solo esté una vez en la relación.

### 6. Asociación de Contenedores Nuevos

- Si se identifican contenedores que deben ser asociados (y no están actualmente vinculados), el plugin realiza la operación de asociación entre el Transporte y los nuevos contenedores mediante la relación correspondiente.

### 7. Notificación (Opcional)

- Existe una sección comentada en el código para crear una notificación al usuario responsable, informando cuántos contenedores se encontraron y cuántos se insertaron, mejorando la trazabilidad del proceso.

### 8. Manejo de Errores

- Todos los errores no críticos capturados durante el procesamiento se almacenan en una lista privada para su análisis posterior, evitando que el proceso falle abruptamente.

---

## Beneficios

- **Automatización:** Elimina tareas manuales y repetitivas para el usuario.
- **Precisión:** Evita asociaciones duplicadas y asegura que ningún contenedor relevante quede sin asociar.
- **Trazabilidad:** Permite (opcionalmente) notificar al usuario sobre las acciones realizadas.
- **Integridad de Datos:** Refuerza la correcta relación entre entidades críticas del proceso logístico.

---

## Casos de Uso

- **Creación de un nuevo Transporte:** Al registrar un nuevo transporte, el plugin asocia automáticamente todos los contenedores que correspondan según consolidado, embarque y aduana.
- **Actualización de un Transporte existente:** Si se modifica un transporte y se agregan nuevas referencias, el plugin asegura que los contenedores vinculados a esas nuevas referencias sean añadidos correctamente.

---

## Consideraciones

- El plugin depende de la correcta configuración de las relaciones entre entidades en Dynamics 365.
- La lógica de notificación puede ser activada/desactivada según las necesidades del negocio, actualmente está comentada.
- El manejo de errores internos permite robustez sin interrumpir el proceso principal.

---

## Resumen

El plugin `DC_CCA_Transporte` es una herramienta esencial para la gestión logística automatizada en Dynacorp, asegurando que la asociación de contenedores a transportes sea completa, precisa y eficiente.