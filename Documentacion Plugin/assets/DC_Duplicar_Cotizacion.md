# Dynacorp - Documentación del Plugin DC_Duplicar_Cotizacion

## Descripción General

**DC_Duplicar_Cotizacion** es un sistema de automatización de duplicado de cotizaciones, diseñado para facilitar la gestión y réplica de registros de cotización y todos sus componentes asociados. El plugin permite que, mediante una simple acción en la interfaz de usuario, se pueda crear una copia completa de una cotización existente, incluyendo sus productos, pesos volumétricos, volúmenes marítimos y otros detalles relevantes. Además, notifica al usuario sobre el éxito del proceso e identifica el nuevo registro creado.

---

## Objetivo del Plugin

El objetivo principal es agilizar el proceso de generación de nuevas cotizaciones reutilizando la información ya registrada, minimizando el riesgo de errores manuales y reduciendo el tiempo invertido por los usuarios en la creación de registros similares. Además, garantiza que todos los datos relevantes, incluidos los componentes relacionados, sean correctamente replicados y asociados a la nueva cotización.

---

## Funcionamiento General

### ¿Cuándo se ejecuta?

El plugin se activa automáticamente cuando se marca el campo específico de duplicación (`dc_duplicarcotizacion`) en una cotización existente. Solo se permite la ejecución una vez por cada acción para evitar duplicados no intencionados.

### Proceso de Duplicado

1. **Verificación Inicial**: 
   - Comprueba que la profundidad de ejecución sea adecuada para evitar llamadas recursivas.
   - Valida que el campo de duplicación esté activo.

2. **Creación de la Nueva Cotización**:
   - Extrae todos los campos relevantes del registro original.
   - Crea un nuevo registro de cotización, replicando fielmente los valores de campos como cliente, expediente, correo, teléfonos, origen, destino, incoterms, tipo de embarque, tipo de operación, tipo y cantidad de bultos, valores de peso, dimensiones, fechas, notas, etc.

3. **Duplicación de Componentes Relacionados**:
   - Replica registros de entidades asociadas, tales como:
     - Pesos volumétricos
     - Volúmenes marítimos
     - Productos de ventas
     - Productos de embarque
     - Productos de aduana
     - Productos de transporte
   - Cada uno de estos componentes se duplica individualmente, y los errores en uno no detienen el proceso de los demás, permitiendo un duplicado tolerante a fallos.

4. **Notificación al Usuario**:
   - Una vez completado el proceso, se genera una notificación en el sistema dirigida al usuario que realizó la acción.
   - La notificación incluye detalles del registro original y un enlace directo al nuevo registro de cotización creado.

5. **Gestión de Errores**:
   - El sistema almacena internamente los errores no críticos ocurridos durante el proceso de duplicación de componentes relacionados, para posibles revisiones posteriores.
   - Si ocurre un error crítico en la duplicación principal de la cotización, el proceso se detiene y no se replica ningún componente.

---

## Entidades y Elementos Duplicados

- **Cotización**: Se copian todos los campos relevantes, tanto información general, datos de cliente, fechas, valores de dimensiones y peso, información de embarque, aduanas, transporte, condiciones, notas y responsables.
- **Pesos Volumétricos**: Se duplican todos los registros asociados, manteniendo los valores e indicadores específicos.
- **Volúmenes Marítimos**: Se replica toda la información de volúmenes marítimos relacionados.
- **Productos de Ventas**: Incluye todos los productos relacionados a la cotización con sus valores, monedas, cantidades y precios.
- **Productos de Embarque**: Se duplican los productos y sus detalles como puertos, cantidades, costos y tarifas escalonadas.
- **Productos de Aduana**: Todos los productos y valores relacionados a servicios de aduana.
- **Productos de Transporte**: Incluye detalles de productos asociados al servicio de transporte, incluyendo zonas, vehículos y valores asociados.

---

## Notificaciones y Experiencia de Usuario

Al finalizar el proceso, el usuario recibe una notificación dentro de la plataforma indicando el éxito del duplicado, el identificador de la cotización original y un acceso directo al nuevo registro. Esto permite un seguimiento eficiente y asegura la trazabilidad de la acción.

---

## Consideraciones Técnicas

- **Tolerancia a Errores**: El sistema está diseñado para registrar internamente los errores no críticos y continuar el proceso cuando sea posible.
- **Idempotencia**: El plugin solo se ejecuta una vez por acción, evitando duplicaciones accidentales.
- **Consistencia de Datos**: Se asegura que toda la información relevante y relaciones entre entidades se mantengan entre el registro original y el duplicado.
- **Personalización**: La lógica puede ser extendida o personalizada para incluir otros componentes asociados a la cotización según lo requiera el negocio.

---

## Escenarios de Uso

- Creación rápida de cotizaciones para clientes recurrentes.
- Replicación de cotizaciones con múltiples componentes y servicios asociados.
- Automatización de procesos comerciales donde la similitud entre cotizaciones es frecuente.

---

## Requisitos Previos

- El plugin debe estar instalado y registrado en el entorno de Dynamics 365.
- El usuario debe tener permisos para crear cotizaciones y sus componentes relacionados.
- Los campos y entidades personalizadas deben existir y estar correctamente configurados en la solución Dynamics.

---

## Soporte y Mantenimiento

Para el correcto funcionamiento y resolución de incidencias:

- Revisar periódicamente los errores internos almacenados por el plugin.
- Ajustar la lógica de duplicación si se agregan nuevos campos o entidades a la cotización.
- Contactar al administrador del sistema para dudas o incidencias recurrentes.

---

## Resumen

DC_Duplicar_Cotizacion automatiza y simplifica el proceso de duplicación de cotizaciones en Dynacorp, asegurando una experiencia de usuario eficiente, consistente y alineada a las mejores prácticas de gestión comercial.