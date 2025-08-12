# Documentación del Plugin DC_Notificacion_Oportunidad

## Sistema: Dynacorp

---

## Descripción General

El plugin `DC_Notificacion_Oportunidad` automatiza el envío de notificaciones a usuarios o equipos responsables cuando se les asigna un registro de Oportunidad en Dynacorp (Dynamics 365). Su objetivo es asegurar que los responsables reciban una alerta inmediata y personalizada, facilitando el seguimiento y la gestión eficiente de oportunidades comerciales dentro del sistema.

---

## Propósito

- Notificar automáticamente a los responsables (usuarios o equipos) cuando se les asigna un registro de Oportunidad.
- Mejorar la trazabilidad y el seguimiento de asignaciones en procesos comerciales.
- Facilitar la colaboración y la responsabilidad al alertar a todos los miembros de un equipo si la asignación es grupal.

---

## Funcionamiento Detallado

### 1. Control de Ejecución

- El plugin se ejecuta solo en el primer nivel de profundidad del pipeline para evitar ejecuciones redundantes.

### 2. Identificación del Responsable

- Obtiene el usuario que realiza la asignación (usuario que inicia la acción).
- Extrae el campo `ownerid` del registro de Oportunidad para identificar si el responsable es un usuario individual o un equipo.
- Determina el nombre y tipo de responsable (usuario o equipo).

### 3. Notificación Personalizada

- **Si el responsable es un usuario individual:**
  - Envía una notificación directamente a ese usuario, personalizada con su nombre y el nombre del asignador.
- **Si el responsable es un equipo:**
  - Recupera todos los miembros del equipo.
  - Envía una notificación individual a cada miembro, indicando que reciben la tarea como parte del equipo y mencionando al asignador.

### 4. Estructura de la Notificación

- El título y cuerpo de la notificación incluyen el propósito de la asignación y el nombre del usuario asignador.
- Incluye un enlace directo al registro de Oportunidad asignado y un ícono personalizado para la notificación.
- La lógica para crear la entidad de notificación está preparada pero actualmente comentada, permitiendo personalización posterior.

### 5. Manejo de Errores

- Todos los errores no críticos que se produzcan durante la ejecución (por ejemplo, problemas al recuperar usuarios o equipos) se almacenan internamente, permitiendo continuar el proceso y facilitar el diagnóstico posterior.

---

## Beneficios

- **Automatización:** Elimina la necesidad de notificaciones manuales al asignar registros de Oportunidad.
- **Trazabilidad:** Asegura que cada responsable sea informado oportunamente y pueda dar seguimiento inmediato.
- **Colaboración:** Si la tarea es de equipo, todos los miembros reciben la alerta, evitando omisiones.
- **Personalización:** Los mensajes pueden adecuarse según el usuario o equipo y el contexto de la asignación.

---

## Casos de Uso

- **Asignación de registros de Oportunidad:** Cuando se asigna un registro a un usuario o equipo, estos reciben una notificación automatizada para iniciar el seguimiento.
- **Gestión de tareas compartidas:** Al asignar registros a equipos, todos los miembros están al tanto y pueden colaborar en el seguimiento.
- **Mejora de la eficiencia operativa:** Asegura que nadie quede sin ser notificado ante la asignación de tareas críticas en el área comercial.

---

## Consideraciones

- El plugin depende de la correcta configuración de usuarios y equipos en Dynacorp.
- La funcionalidad de creación de notificaciones puede personalizarse y activarse según las necesidades del negocio.
- La lógica está preparada para adaptarse tanto a usuarios individuales como a equipos, escalando el proceso de comunicación.

---

## Resumen

El plugin `DC_Notificacion_Oportunidad` es esencial para la gestión eficiente de asignaciones y la comunicación interna en Dynacorp, asegurando que usuarios y equipos sean notificados de manera oportuna y personalizada ante nuevas responsabilidades en registros de oportunidad.