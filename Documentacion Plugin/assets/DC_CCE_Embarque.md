# Documentación del Plugin DC_CCE_Embarque

## Sistema: Dynacorp

---

## Descripción General

El plugin `DC_CCE_Embarque` automatiza el proceso de creación y asociación de consolidado a partir de un embarque dentro del sistema Dynacorp. Su función principal es detectar si se debe generar un consolidado basado en un embarque, crear dicho consolidado con toda la información relevante y vincular los contenedores asociados, optimizando la operación logística y reduciendo la intervención manual.

---

## Propósito

- Automatizar la creación de un consolidado cuando un embarque lo requiere.
- Garantizar que todos los detalles y relaciones relevantes del embarque se transfieran adecuadamente al consolidado.
- Asociar todos los contenedores del embarque al consolidado recién creado.
- Actualizar el embarque para reflejar la referencia al nuevo consolidado.

---

## Funcionamiento Detallado

### 1. Control de Ejecución

- El plugin solo ejecuta su lógica principal cuando la profundidad (`Depth`) del pipeline es 1, evitando ejecuciones repetidas o recursivas.

### 2. Validación de Condición

- Verifica si el campo `dc_crearconsolidado` está activado en el embarque. Solo en ese caso procede el proceso automatizado de consolidación.

### 3. Obtención de Información del Embarque

- Recupera todos los datos relevantes del embarque que servirán para crear el consolidado, incluyendo información general, origen, destino, tipo de operación, gestor, fechas, volúmenes, entre otros.

### 4. Creación del Consolidado

- Genera un nuevo registro de consolidado replicando la información clave del embarque.
- Incluye datos de identificación, referencias, volúmenes, fechas, agentes, consignatarios, transportistas y otros campos relevantes para la trazabilidad logística.
- Utiliza funciones auxiliares para copiar correctamente valores numéricos y de fechas, asegurando que solo se traspasen datos válidos.

### 5. Asociación de Contenedores al Consolidado

- Busca todos los contenedores actualmente asociados al embarque.
- Actualiza cada contenedor para que apunte al nuevo consolidado creado, asegurando que la relación contenedor-consolidado quede actualizada y consistente.

### 6. Actualización del Embarque

- Modifica el embarque original para que su campo de referencia al consolidado (`dc_mawbmbl`) apunte al consolidado recién creado.
- Esto permite mantener la trazabilidad y relación directa entre embarque y consolidado.

### 7. Manejo de Errores

- Todos los errores durante el proceso se almacenan en una lista interna, permitiendo identificar problemas sin detener la ejecución principal del plugin.

### 8. Notificación al Usuario (Opcional)

- El plugin incluye una sección para notificar al usuario sobre la creación exitosa del consolidado (actualmente comentada). Esto puede ser habilitado para mejorar la visibilidad y seguimiento de procesos automáticos.

---

## Beneficios

- **Automatización:** Elimina la necesidad de que el usuario cree manualmente el consolidado y realice las asociaciones necesarias.
- **Consistencia:** Garantiza que toda la información relevante del embarque se transfiera correctamente al consolidado.
- **Trazabilidad:** Mantiene la relación clara entre embarque, consolidado y contenedores.
- **Eficiencia:** Reduce tiempos y errores asociados a la gestión manual de consolidaciones.

---

## Casos de Uso

- **Cierre de embarque grupal:** Cuando varios embarques se consolidan en uno mayor, el plugin automatiza la creación y vinculación del consolidado y sus contenedores.
- **Optimización de operaciones logísticas:** Permite a los operadores logísticos centrarse en tareas estratégicas, dejando la gestión de relaciones y datos repetitivos al sistema.
- **Trazabilidad documental:** Asegura que todas las relaciones y referencias estén siempre actualizadas, facilitando inspecciones y auditorías.

---

## Consideraciones

- El plugin depende de que los campos clave estén correctamente configurados y que los datos de embarque sean válidos.
- El correcto funcionamiento requiere que no existan restricciones personalizadas fuera de Dynamics que impidan la creación o actualización de consolidado/relaciones.
- La habilitación de la notificación al usuario es opcional y puede personalizarse según las políticas internas de Dynacorp.

---

## Resumen

El plugin `DC_CCE_Embarque` es fundamental para la eficiencia y trazabilidad en la gestión de embarques consolidados dentro de Dynacorp, asegurando procesos más ágiles, seguros y transparentes en el manejo de operaciones logísticas complejas.