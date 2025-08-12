# DC_ECD_Aduana - Documentación del Plugin

## Descripción General

**DC_ECD_Aduana** Automatiza la actualización de información clave en las declaraciones asociadas a un registro de aduana. Su función principal es mantener sincronizados los campos de VUCE y Flete entre una aduana y todas sus declaraciones relacionadas, garantizando la coherencia de datos y facilitando la operación de los usuarios.

---

## Objetivo del Plugin

El objetivo principal de este plugin es asegurar que, cuando se actualicen ciertos datos relevantes en un registro de aduana (como el identificador VUCE o el valor del flete), estos cambios se reflejen de manera inmediata y automática en todas las declaraciones asociadas. Esto elimina la necesidad de actualizaciones manuales repetitivas y reduce el riesgo de inconsistencias en la información.

---

## Funcionamiento General

### ¿Cuándo se ejecuta?

El plugin se activa ante la modificación de un registro de aduana, siempre que la profundidad de ejecución sea la adecuada (para evitar llamadas recursivas). Así, cada vez que se actualiza una aduana, se propagan los cambios a las declaraciones relacionadas.

### Proceso de Ejecución

1. **Verificación Inicial**
   - El plugin comprueba que la ejecución no sea recursiva, permitiendo su operación solo una vez por acción.

2. **Obtención de Datos de Aduana**
   - Recupera el registro de aduana modificado, extrayendo los valores de los campos clave: ID, VUCE y Flete.

3. **Búsqueda de Declaraciones Asociadas**
   - Realiza una consulta para obtener todas las declaraciones relacionadas con la aduana actual, identificando aquellas cuyo campo de referencia coincide.

4. **Actualización Masiva de Declaraciones**
   - Para cada declaración encontrada:
     - Actualiza los campos de VUCE y Flete con los valores actuales de la aduana.
     - Cuenta cuántas declaraciones han sido actualizadas.

5. **(Opcional) Notificación**
   - El plugin contiene lógica comentada para notificar al usuario sobre la cantidad de declaraciones actualizadas, permitiendo una fácil activación si se requiere informar al usuario final directamente en la interfaz.

---

## Entidades y Elementos Involucrados

- **Aduana (`dc_aduanas`)**:
  - Registro principal desde el cual se propagan los cambios.
  - Campos clave: `dc_id` (identificador), `dc_vuce` (código VUCE), `dc_flete` (valor del flete).

- **Declaraciones (`dc_declaraciones`)**:
  - Registros secundarios que dependen de la información de la aduana.
  - Campos actualizados: `dc_vuce`, `dc_flete`.
  - Relacionados mediante el campo de referencia a la aduana.

---

## Reglas de Negocio y Validaciones

- **Sincronización Automática**: Cualquier cambio en los campos VUCE o Flete de una aduana se reflejará automáticamente en todas las declaraciones asociadas.
- **Ejecución Única**: El plugin previene la ejecución recursiva, asegurando que la actualización solo ocurra una vez por evento.
- **Notificación Opcional**: Existe la posibilidad de notificar al usuario sobre la actualización masiva de declaraciones.

---

## Experiencia del Usuario

- **Usuario de Negocio**: No requiere realizar acciones manuales para mantener actualizadas las declaraciones; toda la información se sincroniza automáticamente al modificar la aduana.
- **Transparencia**: El usuario puede confiar en que los datos estarán siempre alineados entre aduana y declaraciones.
- **Notificaciones**: (Opcional) Puede ser informado sobre la cantidad de declaraciones actualizadas tras cada modificación.

---

## Consideraciones Técnicas

- **Mantenimiento de Consistencia**: El plugin garantiza que los datos críticos estén sincronizados a través de todas las entidades relacionadas.
- **Escalabilidad**: Es adecuado para escenarios donde una aduana pueda tener múltiples declaraciones asociadas.
- **Extensibilidad**: Puede ampliarse para incluir otros campos, entidades o lógica de negocio según lo requiera la operación.

---

## Escenarios de Uso

- **Actualización de VUCE o Flete en una Aduana**: Al modificar estos datos en una aduana, todas las declaraciones relacionadas se actualizan automáticamente, asegurando que la información sea consistente y actual.
- **Auditoría y Control de Cambios**: Facilita la trazabilidad y reduce el riesgo de errores humanos en la gestión de datos aduaneros.

---

## Requisitos Previos

- El plugin debe estar correctamente instalado y registrado en el entorno de Dynamics 365.
- Los usuarios deben contar con los permisos adecuados para modificar aduanas y declaraciones.
- Los campos personalizados deben estar correctamente configurados y asociados en el modelo de datos.

---

## Soporte y Mantenimiento

- Revisar periódicamente la lógica del plugin ante cambios en el modelo de datos o procesos de negocio.
- Activar o personalizar la notificación al usuario según las necesidades de la operación.
- Contactar al administrador del sistema para soporte o ajustes adicionales.

---

## Resumen

El plugin **DC_ECD_Aduana** asegura la sincronización automática de información clave entre registros de aduana y sus declaraciones asociadas, optimizando la operación y garantizando la integridad de los datos en el sistema Dynacorp.