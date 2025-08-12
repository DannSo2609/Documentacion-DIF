# Documentación del Plugin DC_CPC_Transporte

## Sistema: Dynacorp

---

## Descripción General

El plugin `DC_CPC_Transporte` automatiza la carga y asociación de productos de transporte a un registro de transporte. Su función principal es identificar y asociar automáticamente los productos relevantes al transporte, tomando como base las listas de precios del cliente y considerando parámetros logísticos clave como zonas, suplidor, tipo de vehículo y cantidad.

---

## Propósito

- Automatizar la carga de productos de transporte en registros de transporte, basada en parámetros logísticos y comerciales del cliente.
- Garantizar que la selección de productos se base en listas de precios activas y condiciones específicas de la operación.
- Evitar la duplicidad de productos ya existentes en el transporte.
- Mejorar la eficiencia y precisión en la preparación de servicios logísticos y cotizaciones.

---

## Funcionamiento Detallado

### 1. Condición de Activación

- El plugin se ejecuta únicamente si el campo `dc_cargarprodcutosdelcliente` del transporte está activado.
- Antes de continuar, valida la existencia de datos críticos: cliente, cantidad, zonas de origen y destino y suplidor. Si falta alguno, el proceso se detiene y muestra un mensaje de error claro.

### 2. Obtención y Validación de Datos

- Recupera los datos clave del registro de transporte: consignatario (cliente), incoterms, tipo de vehículo, zonas de origen y destino, cantidad a cargar y suplidor.
- Verifica si ya existen productos de transporte asociados al registro, para evitar duplicados.

### 3. Expansión de Zonas

- Busca no solo las zonas seleccionadas, sino también localidades relacionadas tanto para origen como para destino, generando una lista ampliada de posibilidades logísticas.

### 4. Combinación de Orígenes y Destinos

- Genera todas las combinaciones posibles entre zonas/localidades de origen y destino, permitiendo una búsqueda exhaustiva en las listas de precios del cliente.

### 5. Búsqueda y Filtrado de Listas de Precios

- Recupera todas las listas de precios asociadas al cliente, filtrando por unidad de negocio, suplidor y vigencia temporal.
- Solo considera listas de precios activas para la fecha actual, el suplidor y la configuración logística seleccionada.

### 6. Selección y Creación de Productos

- Para cada combinación válida de zonas y listas de precios, busca los productos que correspondan al tipo de vehículo y zonas seleccionadas.
- Evita crear productos que ya existan en el transporte.
- Crea los productos de transporte en el registro con todos los datos relevantes: producto, suplidor, lista de precios, tipo de vehículo, zonas, unidad de medida, precios y cantidad (aplicando reglas de negocio según la unidad de medida).

### 7. Inclusión de Productos Hijos

- Para cada producto principal, busca productos hijos (subproductos) y los crea también en el transporte si no existen, aplicando la misma lógica de validación y carga.

### 8. Manejo de Errores

- Todos los errores ocurridos durante la creación de productos se almacenan internamente, permitiendo que el proceso continúe con los demás productos y reportando solo los problemas críticos.

---

## Beneficios

- **Automatización:** Elimina la carga manual y repetitiva de productos de transporte en los registros.
- **Precisión:** Solo se crean productos válidos y no duplicados, considerando todos los parámetros y reglas de negocio.
- **Cobertura Completa:** Considera zonas, localidades y productos hijos para una gestión logística integral.
- **Claridad Operativa:** Proporciona retroalimentación clara si falta algún dato esencial, evitando procesos incompletos o incorrectos.
- **Eficiencia Operativa y Comercial:** Agiliza la generación de propuestas, expedientes y validaciones logísticas para clientes.

---

## Casos de Uso

- **Preparación de servicios de transporte:** Cuando un usuario de Dynacorp necesita asociar rápidamente todos los productos de transporte aplicables en un registro.
- **Automatización de expedientes logísticos:** Facilita la generación de presupuestos y gestión de servicios, asegurando que todos los conceptos estén contemplados.
- **Validación logística:** Permite analizar la viabilidad y costos de una operación de transporte según zonas y configuraciones reales.

---

## Consideraciones

- El plugin depende de la correcta parametrización de listas de precios, productos, suplidores, tipos de vehículo, zonas y localidades en Dynacorp.
- Si falta algún dato clave, el proceso no se ejecuta y proporciona un mensaje explicativo.
- El manejo de errores internos permite robustez y continuidad del proceso, informando únicamente los problemas críticos.
- La lógica de inclusión de productos hijos asegura que se contemplen todos los servicios y conceptos relacionados.

---

## Resumen

El plugin `DC_CPC_Transporte` es esencial para la eficiencia y control en la gestión de servicios de transporte en Dynacorp, asegurando que todos los productos relevantes sean considerados automáticamente, optimizando la operación logística y el servicio al cliente.