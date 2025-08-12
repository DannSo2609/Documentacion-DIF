# Documentación del Plugin DC_Notificacion_TareasCompletadas

## Sistema: Dynacorp

---

## Descripción General

El plugin `DC_Notificacion_TareasCompletadas` automatiza el envío de una notificación al creador de una Tarea cuando esta es marcada como "Completada" en Dynacorp (Dynamics 365). Su objetivo es asegurar que el usuario que creó la tarea sea notificado oportunamente cuando otro usuario complete la tarea, facilitando el seguimiento y cierre de actividades.

---

## Propósito

- Notificar automáticamente al usuario creador de la Tarea cuando ésta es marcada como completada.
- Mejorar la trazabilidad y el control de los procesos internos y administrativos.
- Mantener informados a los responsables sobre el avance y cierre de tareas bajo su supervisión.

---

## Funcionamiento Detallado

### 1. Detección de Cambio de Estado

- El plugin se activa al modificar el registro de una Tarea.
- Lee el campo `statecode` del registro, y si el valor es `1` (Completada), continúa con el proceso.

### 2. Identificación de Usuarios

- Recupera el registro de la Tarea y extrae el usuario que la creó (`createdby`).
- Recupera el usuario que realizó la acción de completar la Tarea (`InitiatingUserId`).

### 3. Envío de Notificación

- Genera una notificación para el usuario creador, informando que la Tarea fue completada y especificando quién la completó.
- El mensaje incluye un enlace directo al registro de la Tarea y un ícono personalizado para la notificación.
- La lógica para crear la entidad de notificación está preparada pero actualmente comentada, permitiendo personalización posterior.

### 4. Manejo de Errores

- Cualquier error no crítico durante la ejecución se almacena en una lista interna, permitiendo la continuidad del proceso y facilitando el diagnóstico posterior.

---

## Beneficios

- **Automatización:** Elimina la necesidad de notificaciones manuales, asegurando la comunicación oportuna al usuario creador.
- **Trazabilidad:** Permite un mejor seguimiento de las tareas, informando al creador cuando se completa una actividad bajo su responsabilidad.
- **Transparencia:** Facilita el control y cierre de procesos administrativos y operativos.

---

## Casos de Uso

- **Notificación al usuario:** Cuando una Tarea es completada, el creador es notificado automáticamente para su seguimiento.
- **Control interno:** Permite al responsable confirmar que la Tarea bajo su supervisión ha sido atendida y cerrada.
- **Mejora de procesos administrativos:** Asegura que los involucrados reciban información relevante sin intervención manual.

---

## Consideraciones

- El plugin depende de la correcta configuración de usuarios y del flujo de trabajo de tareas en Dynacorp.
- La funcionalidad de creación de notificaciones puede personalizarse y activarse según las necesidades del negocio.
- El correcto funcionamiento requiere que el campo `statecode` utilice el valor `1` para indicar el estado "Completada".

---

## Resumen

El plugin `DC_Notificacion_TareasCompletadas` es fundamental para la gestión eficiente y transparente de tareas en Dynacorp, asegurando que los creadores de las tareas sean notificados de manera oportuna cuando una actividad es completada por otro usuario.