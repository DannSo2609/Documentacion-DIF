# Documentación del Plugin DC_CPC_CotizacionTransporte
## Sistema: Dynacorp

---

## Descripción General

El plugin `DC_CPC_CotizacionTransporte` automatiza la carga y asociación de productos de transporte a una cotización de transporte. Su función principal es identificar y asociar automáticamente los productos relevantes a la cotización, tomando como base las listas de precios del cliente y los parámetros logísticos clave como zonas, suplidor, tipo de vehículo y cantidad.

---

## Propósito

- Automatizar la carga de productos de transporte a cotizaciones, basada en las configuraciones y condiciones logísticas del cliente.
- Garantizar que la selección de productos se base en listas de precios activas y parámetros clave de la operación.
- Evitar la duplicidad de productos ya existentes en la cotización.
- Mejorar la eficiencia y precisión en la preparación de propuestas comerciales y logísticas.

---

## Funcionamiento Detallado

### 1. Condición de Activación

- El plugin se ejecuta solo si el campo `dc_cargarprodcutosdelclientetra` de la cotización está activado.
- Antes de proseguir, valida la existencia de datos críticos: cliente, cantidad, zonas de origen y destino, y suplidor de transporte. Si falta algún dato, lanza un error claro y detiene el proceso.

### 2. Obtención y Validación de Datos

- Recupera los datos clave del registro de cotización: cliente, suplidor, incoterms, tipo de vehículo, zonas de origen y destino, y cantidad a cargar.
- Verifica la existencia de productos de transporte ya asociados a la cotización, para evitar duplicados.

### 3. Expansión de Zonas

- Busca no solo las zonas seleccionadas, sino también localidades relacionadas tanto para origen como para destino, generando una lista ampliada de posibilidades.

### 4. Combinación de Orígenes y Destinos

- Genera todas las combinaciones posibles entre zonas/localidades de origen y destino, para una búsqueda más exhaustiva de listas de precios.

### 5. Búsqueda y Filtrado de Listas de Precios

- Recupera todas las listas de precios asociadas al cliente, filtrando por unidad de negocio, suplidor y validez temporal.
- Solo considera listas de precios activas para la fecha actual, el suplidor y la configuración logística seleccionada.

### 6. Selección y Creación de Productos

- Para cada combinación válida de zonas y listas de precios, busca los productos que correspondan al tipo de vehículo y zonas seleccionadas.
- Evita crear productos que ya existan en la cotización.
- Crea los productos de transporte en la cotización con todos los datos relevantes: producto, suplidor, lista de precios, tipo de vehículo, zonas, unidad de medida, precios y cantidad (aplicando reglas de negocio según la unidad de medida).

### 7. Inclusión de Productos Hijos

- Para cada producto principal, busca productos hijos (subproductos) y los crea también en la cotización si no existen, aplicando la misma lógica de validación y carga.

### 8. Manejo de Errores

- Todos los errores que ocurren durante la creación de productos se almacenan internamente, permitiendo que el proceso continúe con los demás productos y reportando solo los problemas críticos.

---

## Beneficios

- **Automatización:** Elimina la carga manual y repetitiva de productos de transporte en cotizaciones.
- **Precisión:** Solo se crean productos válidos y no duplicados, considerando todos los parámetros y reglas de negocio.
- **Cobertura Completa:** Considera zonas, localidades y productos hijos para una propuesta logística integral.
- **Claridad Operativa:** Proporciona retroalimentación clara si algún dato esencial falta, evitando procesos incompletos o incorrectos.
- **Eficiencia Comercial y Operativa:** Agiliza la generación de propuestas y la validación logística para clientes.

---

## Casos de Uso

- **Preparación de cotizaciones de transporte:** Cuando un usuario de Dynacorp necesita asociar rápidamente todos los productos de transporte aplicables en una cotización.
- **Automatización comercial:** Facilita la generación de presupuestos y propuestas comerciales para clientes, asegurando que todos los servicios estén contemplados.
- **Validación logística:** Permite analizar la viabilidad y costos de una operación de transporte según zonas y configuraciones reales.

---

## Consideraciones

- El plugin depende de la correcta parametrización de listas de precios, productos, suplidores, tipos de vehículo, zonas y localidades en Dynacorp.
- Si falta algún dato clave, el proceso no se ejecuta y proporciona un mensaje explicativo.
- El manejo de errores internos permite robustez y continuidad del proceso, informando únicamente los problemas críticos.
- La lógica de inclusión de productos hijos asegura que se contemplen todos los servicios y conceptos relacionados.

---

## Resumen

El plugin `DC_CPC_CotizacionTransporte` es clave para la eficiencia y control en la generación de cotizaciones de transporte en Dynacorp, asegurando que todos los productos relevantes sean considerados de forma automática y precisa, optimizando el servicio al cliente y los procesos logísticos.