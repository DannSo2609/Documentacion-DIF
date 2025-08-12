# Documentación del Plugin DC_Notificacion_Consolidado

## Sistema: Dynacorp

---

## Descripción General

El plugin `DC_Notificacion_Consolidado` automatiza el envío de notificaciones a usuarios o equipos responsables cuando se les asigna un registro de Consolidado en Dynacorp (Dynamics 365). Además, asegura que todos los Embarques e "hijos" (nietos) relacionados con el Consolidado sean reasignados al nuevo propietario y notifica sobre la cantidad de registros implicados, facilitando el seguimiento y gestión eficiente de consolidaciones logísticas.

---

## Propósito

- Notificar automáticamente a los responsables (usuarios o equipos) cuando se les asigna un registro de Consolidado.
- Propagar la asignación a todos los Embarques asociados y, a su vez, a los "nietos" relacionados.
- Mejorar la trazabilidad y el seguimiento de asignaciones en procesos logísticos complejos.
- Facilitar la colaboración y la responsabilidad al alertar a todos los miembros de un equipo si la asignación es grupal.

---

## Funcionamiento Detallado

### 1. Control de Ejecución

- El plugin se ejecuta solo en el primer nivel de profundidad del pipeline para evitar ejecuciones redundantes.

### 2. Identificación del Responsable

- Obtiene el usuario que realiza la asignación (usuario que inicia la acción).
- Extrae el campo `ownerid` del registro de Consolidado para identificar si el responsable es un usuario individual o un equipo.
- Determina el nombre y tipo de responsable (usuario o equipo).

### 3. Reasignación Recursiva

- Recupera todos los Embarques asociados al Consolidado y reasigna su propietario al nuevo responsable.
- Para cada Embarque, recupera los "nietos" (otros embarques relacionados) y también los reasigna.
- Lleva la cuenta de cuántos Embarques y Nietos fueron reasignados.

### 4. Notificación Personalizada

- **Si el responsable es un usuario individual:**
  - Envía una notificación directamente a ese usuario, personalizada con su nombre, el nombre del asignador y la cantidad de Embarques y Nietos reasignados.
- **Si el responsable es un equipo:**
  - Recupera todos los miembros del equipo.
  - Envía una notificación individual a cada miembro, indicando que reciben la tarea como parte del equipo, mencionando al asignador y la cantidad de registros implicados.

- Si no hay Embarques asociados, la notificación se limita a la asignación principal.

### 5. Estructura de la Notificación

- El título y cuerpo de la notificación incluyen el propósito de la asignación, el nombre del usuario asignador y el detalle de la cantidad de registros asociados.
- Incluye un enlace directo al registro de Consolidado asignado y un ícono personalizado para la notificación.
- La lógica para crear la entidad de notificación está preparada pero actualmente comentada, permitiendo personalización posterior.

### 6. Manejo de Errores

- Todos los errores no críticos que se produzcan durante la ejecución (por ejemplo, problemas al recuperar usuarios, equipos o actualizar propietarios) se almacenan internamente, permitiendo continuar el proceso y facilitar el diagnóstico posterior.

---

## Beneficios

- **Automatización Propagada:** No solo notifica la asignación principal, sino que asegura la reasignación de todos los registros relacionados.
- **Trazabilidad:** Asegura que cada responsable sea informado y que todos los registros dependientes sean correctamente transferidos.
- **Colaboración:** Si la tarea es de equipo, todos los miembros reciben la alerta y pueden colaborar en el seguimiento.
- **Personalización:** Los mensajes incluyen detalles de la operación y el contexto de la asignación.

---

## Casos de Uso

- **Asignación de registros de Consolidado:** Cuando se asigna un Consolidado a un usuario o equipo, este recibe una notificación automatizada y todos los registros dependientes se reasignan.
- **Gestión de tareas logísticas complejas:** Al asignar registros a equipos, todos los miembros están al tanto y pueden colaborar en el seguimiento de los múltiples embarques implicados.
- **Mejora de la eficiencia operativa:** Asegura que nadie quede sin ser notificado y que toda la estructura dependiente esté bajo control del nuevo responsable.

---

## Consideraciones

- El plugin depende de la correcta configuración de usuarios, equipos, relaciones de Embarques y Consolidado en Dynacorp.
- La funcionalidad de creación de notificaciones puede personalizarse y activarse según las necesidades del negocio.
- El correcto funcionamiento requiere que las relaciones de Consolidado -> Embarques -> Nietos estén bien mapeadas.

---

## Resumen

El plugin `DC_Notificacion_Consolidado` es fundamental para la gestión eficiente de asignaciones y la comunicación interna en Dynacorp, asegurando que usuarios y equipos sean notificados oportunamente y que todas las entidades asociadas sean correctamente transferidas ante nuevas responsabilidades logísticas.