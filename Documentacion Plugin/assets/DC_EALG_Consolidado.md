# DC_EALG_Consolidado - Documentación del Plugin

## Descripción General

**DC_EALG_Consolidado** Su propósito es automatizar y gestionar el proceso de envío de avisos de llegada unificados para embarques consolidados, asegurando una comunicación eficiente y estandarizada con los consignatarios. El plugin genera un correo electrónico con toda la información relevante, adjunta los documentos necesarios y actualiza automáticamente el estado de los registros implicados.

---

## Objetivo del Plugin

El objetivo principal de este plugin es facilitar el envío grupal de avisos de llegada de embarques que comparten un mismo consignatario, unificando la información en una sola notificación, adjuntando los documentos PDF correspondientes y desmarcando los registros consolidados una vez enviado el aviso. Esto optimiza el flujo de información y reduce los errores o duplicidades en la comunicación operativa.

---

## Funcionamiento General

### ¿Cuándo se ejecuta?

El plugin se activa al marcar el campo de envío unificado (`dc_enviarunificado`) en cualquier registro consolidado. Solo se ejecuta una vez por acción para evitar notificaciones duplicadas.

### Proceso de Ejecución

1. **Verificación Inicial**  
   - El plugin verifica que la ejecución no sea recursiva y que el campo de envío unificado esté activo.

2. **Obtención de Consolidados**  
   - Se recuperan todos los registros de tipo "consolidado" marcados para ser unificados en el envío.

3. **Validación del Consignatario**  
   - Se asegura que todos los embarques asociados a los consolidados tengan el mismo consignatario. Si no es así, se detiene el proceso y se lanza un error.

4. **Construcción de la Tabla de Embarques**  
   - Para cada embarque relacionado, se recopila la información relevante: MBL/HBL, buque, valor de flete, puertos, fechas, etc.
   - Se genera una tabla HTML que resume toda la información de los embarques incluidos en el aviso unificado.

5. **Generación del Correo Electrónico**  
   - Se compone un correo con formato HTML, incluyendo la tabla de embarques, instrucciones y advertencias para el consignatario.

6. **Adjunto de Documentos**  
   - Para cada embarque, se adjunta el archivo PDF correspondiente (si está disponible y es válido) al correo generado.

7. **Envío del Correo**  
   - El correo se envía automáticamente al consignatario a través del sistema, utilizando las funcionalidades nativas de Dynamics 365.

8. **Actualización de Consolidados**  
   - Los registros consolidados procesados se actualizan para desmarcar los campos de unificación y envío, evitando que vuelvan a ser procesados en futuros avisos.

9. **Gestión de Errores**  
   - Todos los errores internos no críticos se almacenan para su posterior revisión. Solo errores críticos detienen la ejecución.

---

## Reglas de Negocio y Validaciones

- **Consignatario Único**: Es imprescindible que todos los embarques relacionados tengan el mismo consignatario, de lo contrario el proceso se detiene y se notifica el problema.
- **Archivos Adjuntos**: Solo se adjuntan archivos en formato PDF. Si un embarque no tiene archivo o el formato es diferente, se registra el error pero el proceso continúa.
- **Actualización de Estado**: Tras el envío, los consolidados se actualizan para evitar reprocesamientos accidentales.

---

## Elementos y Entidades Involucradas

- **Consolidados**: Registros que agrupan embarques para procesar envíos unificados.
- **Embarques**: Entidades vinculadas a consolidados, contienen datos específicos de cada movimiento.
- **Consignatario**: Persona o entidad que recibe el embarque, destinatario del aviso.
- **Correo Electrónico**: Entidad generada y enviada automáticamente, incluye la tabla de embarques y los archivos adjuntos.
- **Archivos Adjuntos**: Documentos PDF de cada embarque, anexados al correo si están disponibles y cumplen los requisitos.

---

## Experiencia del Usuario

El usuario solo debe marcar el campo de envío unificado. El sistema se encarga de:

- Validar la agrupación.
- Generar y enviar el aviso al consignatario.
- Adjuntar los documentos PDF de cada embarque relacionado.
- Restablecer los estados de los consolidados procesados.

El consignatario recibirá un correo estructurado, con todos los embarques relacionados y las instrucciones necesarias.

---

## Consideraciones Técnicas

- **Tolerancia a Errores**: Los errores no críticos no detienen el proceso, pero se almacenan para su revisión.
- **Integridad de Datos**: Se asegura que ningún consolidado sea procesado más de una vez.
- **Formato de Archivos**: Solo se aceptan y adjuntan archivos PDF.
- **Personalización**: El formato del correo y la tabla pueden ser adaptados según los requerimientos del negocio.

---

## Escenarios de Uso

- **Envío de avisos de llegada para embarques agrupados a un mismo consignatario.**
- **Gestión eficiente y automatizada de la comunicación operativa con clientes.**
- **Automatización del adjunto de documentos y control de reprocesamiento de consolidados.**

---

## Requisitos Previos

- El plugin debe estar instalado y correctamente registrado en el entorno de Dynamics 365.
- Los usuarios deben tener los permisos necesarios para modificar consolidados y enviar correos.
- Los campos y entidades personalizadas deben estar correctamente configurados y disponibles.

---

## Soporte y Mantenimiento

- Revisar periódicamente los errores internos registrados por el plugin.
- Ajustar la lógica si se agregan nuevos campos, entidades o requisitos al proceso.
- Contactar al administrador del sistema o equipo de desarrollo ante incidencias o dudas recurrentes.

---

## Resumen

**DC_EALG_Consolidado** automatiza y estandariza el envío de avisos de llegada unificados en el sistema Dynacorp, optimizando la comunicación con consignatarios y asegurando la correcta gestión de embarques consolidados.