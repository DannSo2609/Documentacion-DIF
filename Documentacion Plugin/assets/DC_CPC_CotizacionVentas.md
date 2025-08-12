# Documentación del Plugin DC_CPC_CotizacionVentas

## Sistema: Dynacorp

---

## Descripción General

El plugin `DC_CPC_CotizacionVentas` automatiza la carga y asociación de productos de ventas a una cotización de ventas. Su objetivo es identificar y asociar automáticamente los productos relevantes a la cotización, tomando como base las listas de precios del cliente y los parámetros comerciales clave, como suplidor y cantidad, asegurando así que la cotización refleje todos los productos y servicios aplicables de manera precisa y eficiente.

---

## Propósito

- Automatizar la carga de productos de ventas a cotizaciones, basada en las configuraciones y condiciones comerciales del cliente.
- Garantizar que la selección de productos se base en listas de precios activas y parámetros clave de la operación.
- Evitar la duplicidad de productos ya existentes en la cotización.
- Mejorar la eficiencia y precisión en la preparación de propuestas y presentaciones comerciales.

---

## Funcionamiento Detallado

### 1. Condición de Activación

- El plugin se ejecuta solo si el campo `dc_cargarprodcutosdelclienteven` de la cotización está activado.
- Antes de continuar, valida la existencia de datos críticos: cliente, cantidad y suplidor de ventas. Si falta algún dato, muestra un error claro y detiene el proceso.

### 2. Obtención y Validación de Datos

- Recupera los datos clave del registro de cotización: cliente, suplidor de ventas, incoterms y cantidad a cargar.
- Verifica si existen productos de ventas ya asociados a la cotización, para evitar duplicados.

### 3. Búsqueda y Filtrado de Listas de Precios

- Recupera todas las listas de precios asociadas al cliente, filtrando por unidad de negocio y suplidor de ventas.
- Solo considera listas de precios activas para la fecha actual y los parámetros seleccionados.

### 4. Selección y Creación de Productos

- Para cada lista de precios válida, busca los productos que correspondan a los criterios establecidos.
- Evita crear productos que ya existan en la cotización.
- Crea los productos de ventas en la cotización con todos los datos relevantes: producto, suplidor, lista de precios, unidad de medida, precios y cantidad (aplicando reglas de negocio según la unidad de medida).

### 5. Inclusión de Productos Hijos

- Para cada producto principal, busca productos hijos (subproductos) y los crea también en la cotización si no existen, aplicando la misma lógica de validación y carga.

### 6. Manejo de Errores

- Todos los errores que ocurren durante la creación de productos se almacenan internamente, permitiendo que el proceso continúe con los demás productos y reportando solo los problemas críticos.

---

## Beneficios

- **Automatización:** Elimina la carga manual y repetitiva de productos de ventas en cotizaciones.
- **Precisión:** Solo se crean productos válidos y no duplicados, considerando todos los parámetros y reglas de negocio.
- **Cobertura Completa:** Considera productos hijos para una propuesta comercial integral.
- **Claridad Operativa:** Proporciona retroalimentación clara si algún dato esencial falta, evitando procesos incompletos o incorrectos.
- **Eficiencia Comercial:** Agiliza la generación de propuestas y la validación comercial para clientes.

---

## Casos de Uso

- **Preparación de cotizaciones de ventas:** Cuando un usuario de Dynacorp necesita asociar rápidamente todos los productos y servicios de ventas aplicables en una cotización.
- **Automatización comercial:** Facilita la generación de presupuestos y propuestas para clientes, asegurando que todos los servicios estén contemplados.
- **Validación de disponibilidad comercial:** Permite analizar la viabilidad de una operación comercial según la configuración real del cliente.

---

## Consideraciones

- El plugin depende de la correcta parametrización de listas de precios, productos y suplidores en Dynacorp.
- Si falta algún dato clave, el proceso no se ejecuta y proporciona un mensaje explicativo.
- El manejo de errores internos permite robustez y continuidad del proceso, informando únicamente los problemas críticos.
- La lógica de inclusión de productos hijos asegura que se contemplen todos los servicios y conceptos relacionados.

---

## Resumen

El plugin `DC_CPC_CotizacionVentas` es clave para la eficiencia y control en la generación de cotizaciones de ventas en Dynacorp, asegurando que todos los productos relevantes sean considerados de forma automática y precisa, optimizando la propuesta comercial y el servicio al cliente.