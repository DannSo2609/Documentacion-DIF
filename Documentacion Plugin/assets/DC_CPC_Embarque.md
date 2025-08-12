# Documentación del Plugin DC_CPC_Embarque

## Sistema: Dynacorp

---

## Descripción General

El plugin `DC_CPC_Embarque` automatiza la carga y asociación de productos del cliente a un embarque. Su objetivo es identificar y asociar automáticamente los productos relevantes al embarque, utilizando listas de precios del cliente y considerando parámetros logísticos clave como puertos, suplidor, cantidades, tipo de embarque y tipo de transporte.

---

## Propósito

- Automatizar la carga de productos de embarque, basada en configuraciones y condiciones logísticas/comerciales del cliente.
- Garantizar que la selección de productos se base en listas de precios activas y parámetros clave de la operación.
- Evitar la duplicidad de productos ya existentes en el embarque.
- Mejorar la eficiencia y precisión en la preparación de expedientes y servicios logísticos.

---

## Funcionamiento Detallado

### 1. Condición de Activación

- El plugin se ejecuta únicamente si el campo `dc_cargarprodcutosdelcliente` del embarque está activado.
- Antes de proseguir, valida la existencia de datos críticos: cliente, puertos de origen y destino, cantidad y suplidor. Si falta algún dato, lanza un error claro y detiene el proceso.

### 2. Obtención y Validación de Datos

- Recupera los datos clave del embarque: consignatario (cliente), puertos, suplidor de embarque, tipo de embarque, incoterms, tipo de operación, tipo de transporte y cantidad a cargar.
- Verifica que no existan productos de embarque duplicados asociados al registro.

### 3. Expansión de Puertos

- Busca no solo los puertos seleccionados directamente, sino también los grupos de puertos relacionados para origen y destino, ampliando las combinaciones posibles para la búsqueda de listas de precios.

### 4. Combinación de Orígenes y Destinos

- Genera todas las combinaciones posibles entre puertos de origen y destino, permitiendo un filtrado exhaustivo en las listas de precios.

### 5. Búsqueda y Filtrado de Listas de Precios

- Recupera todas las listas de precios asociadas al cliente, filtrando por unidad de negocio, suplidor, puertos y vigencia temporal.
- Solo considera listas de precios activas para la fecha actual y los parámetros logísticos seleccionados.

### 6. Selección y Creación de Productos

- Para cada lista de precios válida, busca los productos que cumplen los criterios de operación, embarque y transporte.
- Verifica que cada producto no exista ya en el embarque.
- Crea los productos de embarque en el registro, incluyendo todos los datos relevantes: producto, suplidor, lista de precios, puertos, unidad de medida, costos, precios y cantidad (según reglas de negocio y unidad de medida).

### 7. Inclusión de Productos Hijos

- Para cada producto principal, busca productos hijos (subproductos) y los crea también en el embarque si no existen, aplicando la misma lógica de validación y carga.

### 8. Manejo de Errores

- Todos los errores ocurridos en la creación de productos son almacenados internamente, permitiendo que el proceso continúe con los demás productos y reportando solo los problemas críticos.

---

## Beneficios

- **Automatización:** Elimina la carga manual y repetitiva de productos de embarque.
- **Precisión:** Solo se crean productos válidos y no duplicados, considerando todos los parámetros y reglas del negocio.
- **Cobertura Completa:** Considera grupos de puertos y productos hijos, asegurando una gestión logística integral.
- **Eficiencia Operativa:** Agiliza la preparación de expedientes de embarque y la gestión de servicios logísticos.

---

## Casos de Uso

- **Preparación de expedientes de embarque:** Cuando un usuario de Dynacorp necesita asociar rápidamente todos los productos aplicables a un embarque.
- **Automatización comercial y logística:** Facilita la generación de presupuestos y la gestión de servicios, asegurando que todos los conceptos estén contemplados.
- **Validación logística:** Permite analizar la viabilidad y costos de una operación según configuración y condiciones comerciales del cliente.

---

## Consideraciones

- El plugin depende de la correcta parametrización de listas de precios, productos, suplidores y relaciones de puertos en Dynacorp.
- Si falta algún dato clave, el proceso no se ejecuta y proporciona un mensaje explicativo al usuario.
- El manejo de errores internos permite robustez y continuidad del proceso, informando únicamente los problemas críticos.
- La lógica de inclusión de productos hijos asegura que se contemplen todos los servicios y conceptos relacionados.

---

## Resumen

El plugin `DC_CPC_Embarque` es fundamental para la eficiencia y control en la gestión de embarques en Dynacorp, asegurando que todos los productos relevantes sean considerados automáticamente, optimizando la operación logística y el servicio al cliente.