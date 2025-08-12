# Dynacorp - Plugin DC_CPIA_General

## Descripción General

El plugin **DC_CPIA_General** es una solución parametrizable para la automatización de la creación de productos asociados a procesos de Aduana, Embarque y Transporte. Su objetivo es centralizar y estandarizar la lógica de asignación de productos basada en listas de precios, orígenes, destinos y reglas de negocio, reemplazando así la necesidad de múltiples plugins individuales para cada entidad.

## Objetivo

- Automatizar la generación de productos en oportunidades, cotizaciones y entidades de operación.
- Permitir la configuración dinámica de campos y reglas por cada tipo de proceso (aduana, embarque, transporte) y entidad.
- Mantener la lógica centralizada, escalable y fácil de mantener.

## Funcionamiento General

El plugin detecta cuándo debe ejecutarse a partir de un flag (campo booleano) configurado en la entidad. Cuando este flag es activado (`true`), el plugin:

1. Recupera la configuración específica para la entidad y proceso (aduana, embarque, transporte).
2. Valida que los campos requeridos estén completos (origen, destino, suplidor, cantidad, tipo de transporte, etc.).
3. Busca las listas de precios válidas para el cliente, origen, destino y suplidor.
4. Verifica si ya existen productos asociados para evitar duplicados.
5. Crea los productos asociados (y sus productos hijos, si corresponde) en la entidad de productos correspondiente.
6. Utiliza equivalencias de origen y destino cuando existen relaciones alternativas.
7. Lanza mensajes de error claros cuando faltan datos obligatorios.

## Estructura de Configuración

El plugin utiliza una estructura de configuración por entidad y proceso, que define:

- **Nombre de la entidad principal** (ej: oportunidad, cotización, embarque)
- **Campos de referencia** (campos de origen, destino, suplidor, cantidad, flag IA, etc.)
- **Tabla y campos de relación para listas de precios del cliente**
- **Campos y tablas de equivalencia de origen/destino**
- **Entidad donde se crean los productos**
- **Unidad de negocio y otros parámetros de filtrado**
- **Mensajes de error personalizados por entidad**

Esta configuración se almacena en un diccionario interno y es seleccionada dinámicamente según la entidad y el proceso activo.

## Lógica de Ejecución

1. **Detección del proceso y entidad:**  
   El plugin detecta el tipo de operación a partir del nombre lógico de la entidad y los flags de proceso (por ejemplo, Transporte, Aduana o Embarque).

2. **Validaciones:**  
   Se verifica que todos los campos necesarios estén completos antes de procesar (origen, destino, suplidor, cantidad, etc.). Si falta alguno, el plugin detiene la ejecución y muestra un mensaje específico con instrucciones para el usuario.

3. **Obtención de listas de precios:**  
   Se consultan las listas de precios asociadas al cliente y se filtran por origen, destino, suplidor y unidad de negocio, utilizando equivalencias si están configuradas.

4. **Creación de productos:**  
   Se crean productos relacionados con la entidad principal, evitando duplicados, y se asocian los datos de lista de precios, suplidor, cantidad, unidad de medida, costos y demás atributos relevantes.

5. **Manejo de productos hijos:**  
   Si una lista de precios tiene productos hijos (productos secundarios asociados), también se crean y asocian automáticamente, replicando la lógica del producto principal.

6. **Errores y trazabilidad:**  
   En caso de error (por ejemplo, falta de datos, problemas con la creación de registros, etc.), el plugin lanza mensajes claros y detallados para facilitar la corrección y el soporte.

## Cobertura de Procesos

El plugin cubre automáticamente los siguientes procesos y entidades, mediante configuración:

- **Aduana:** Cotización, Oportunidad, Operación de Aduana
- **Embarque:** Cotización, Oportunidad, Embarque
- **Transporte:** Cotización, Oportunidad, Transporte

Cada uno tiene su propia configuración de campos y lógica adaptada.

## Ventajas

- **Centralización:** Toda la lógica está centralizada en un solo plugin, facilitando el mantenimiento y la evolución.
- **Configurabilidad:** Permite modificar la lógica simplemente ajustando la configuración interna, sin necesidad de modificar código fuente.
- **Escalabilidad:** Añadir nuevos procesos, entidades o variaciones solo requiere agregar entradas de configuración.
- **Reducción de Errores:** Minimiza la duplicidad y el riesgo de inconsistencias entre entidades.
- **Mensajes Claros:** Los errores y advertencias son específicos del proceso, facilitando el soporte y la formación de usuarios.

## Consideraciones para la Configuración

- Los campos y nombres lógicos de cada entidad deben estar correctamente configurados en la estructura de configuración.
- Es importante validar que todos los campos requeridos estén incluidos y que los valores en las entidades de Dataverse/Dynamics coincidan exactamente (mayúsculas/minúsculas y nombres lógicos).
- Los cambios en la lógica de negocio suelen poder implementarse modificando únicamente la configuración, sin necesidad de recompilar el plugin.

## Escenarios de Uso

- Generación automática de productos de cotización en oportunidades de transporte, aduana o embarque.
- Creación de productos secundarios asociados a una lista de precios principal.
- Validación automática de datos antes de ejecutar procesos comerciales.
- Facilitar la gestión de tarifas y servicios complejos en operaciones logísticas.

## Mensajes de Error

El plugin proporciona mensajes personalizados y detallados cuando faltan datos, hay errores de configuración o problemas en la creación de productos. Esto permite al usuario identificar rápidamente la causa y proceder a la corrección.

## Recomendaciones de Mantenimiento

- Revisar y actualizar la configuración ante cambios en los nombres de campos o procesos del sistema.
- Validar que todas las entidades y relaciones configuradas existan en el entorno Dynacorp.
- Probar cada proceso tras actualizaciones importantes para asegurar la continuidad operativa.
