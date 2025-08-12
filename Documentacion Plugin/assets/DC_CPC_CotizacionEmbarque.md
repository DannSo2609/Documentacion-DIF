# Documentación del Plugin DC_CPC_CotizacionEmbarque

## Sistema: Dynacorp

---

## Descripción General

El plugin `DC_CPC_CotizacionEmbarque` automatiza la carga y asociación de productos del cliente a una cotización de embarque en Dynacorp (Dynamics 365). Su objetivo es identificar, calcular y asociar automáticamente los productos procedentes de listas de precios del cliente, considerando parámetros logísticos como puertos, suplidor, cantidades, tipo de embarque y tipo de transporte, así como reglas comerciales y de competencia.

---

## Propósito

- Automatizar la carga de productos del cliente en cotizaciones de embarque.
- Garantizar que la selección y cantidad de productos estén basadas en listas de precios válidas y parámetros logísticos/comerciales reales.
- Evitar la duplicidad de productos en la cotización y asegurar la inclusión de productos hijos y conceptos relacionados.
- Mejorar la eficiencia y precisión en la preparación de cotizaciones comerciales y operativas.

---

## Funcionamiento Detallado

### 1. Condición de Activación

- El plugin se ejecuta solo si el campo `dc_cargarprodcutosdelcliente` de la cotización está activado.
- Antes de continuar, valida la existencia de datos críticos: cliente, puertos de origen y destino, cantidad a cargar y suplidor de embarque. Si falta alguno, lanza un mensaje de error claro y detiene el proceso.

### 2. Obtención y Validación de Datos

- Recupera todos los datos relevantes de la cotización: cliente, puertos, suplidor de embarque, tipo de embarque, incoterms, tipo de operación, tipo de transporte y cantidades (peso, volumen, unidades).
- Verifica que existan productos de embarque ya asociados a la cotización para evitar duplicados.

### 3. Expansión de Puertos

- Busca no solo los puertos seleccionados, sino también grupos de puertos relacionados, generando todas las combinaciones posibles entre origen y destino para una búsqueda más exhaustiva de listas de precios.

### 4. Búsqueda y Filtrado de Listas de Precios

- Recupera todas las listas de precios asociadas al cliente, filtrando por unidad de negocio, suplidor, puertos, fechas de vigencia y otros parámetros logísticos.
- Solo se consideran listas de precios activas y válidas para la fecha actual y los parámetros seleccionados.

### 5. Selección y Creación de Productos

- Para cada lista de precios válida, busca los productos que cumplen con los criterios de operación, embarque y transporte.
- Verifica que el producto no exista ya en la cotización.
- Los productos pueden ser:
  - **No competitivos:** (ejemplo: B/L, LS) Se crean directamente con la cantidad y precio correspondiente.
  - **Competitivos:** Se agrupan por producto padre y se selecciona la mejor opción en base al mayor valor calculado (cantidad x precio de venta).

### 6. Cálculo Automático de Cantidad para Productos Competitivos

- Calcula la cantidad de cada producto competitivo en función de la unidad de medida y los valores de la cotización (kg, toneladas, libras, metros cúbicos, pies cúbicos), utilizando las bases de tarifa configuradas (rate base) para peso y dimensiones.

### 7. Inclusión de Productos Hijos

- Para cada producto principal, busca productos hijos (subproductos) y los crea también en la cotización si no existen, aplicando la misma lógica de validación y carga.

### 8. Manejo de Errores

- Todos los errores que ocurren al crear productos (por ejemplo, problemas de datos o reglas de negocio) se almacenan internamente y no detienen el proceso general, permitiendo continuar con los demás productos.

---

## Beneficios

- **Automatización Integral:** Elimina la carga manual y asegura que todos los productos válidos estén en la cotización.
- **Precisión Comercial y Operativa:** Solo se incluyen productos y cantidades que cumplen criterios comerciales y logísticos reales.
- **Cobertura Completa:** Incluye productos hijos y conceptos relacionados, asegurando una cotización integral.
- **Flexibilidad y Escalabilidad:** Considera grupos de puertos y reglas avanzadas de competencia y tarifas.
- **Eficiencia:** Ahorra tiempo operativo y reduce errores de omisión o duplicidad.

---

## Casos de Uso

- **Generación de cotizaciones de embarque:** Cuando se requiere preparar rápidamente una cotización para un cliente incluyendo todos los servicios y productos aplicables.
- **Simulación y análisis logístico:** Permite analizar qué productos y costos aplicarían según distintas combinaciones de puertos, cantidades y suplidores.
- **Optimización comercial:** Facilita la propuesta de la mejor combinación de productos competitivos para el cliente.

---

## Consideraciones

- El plugin depende de una correcta configuración de listas de precios, productos, suplidores y bases de tarifas (rate base) en Dynacorp.
- Si faltan datos clave, el proceso no se ejecuta y reporta el error al usuario.
- El manejo de errores internos permite robustez y continuidad, informando solo los problemas críticos.
- La lógica de selección de productos competitivos prioriza el mayor valor para el cliente y la empresa.

---

## Resumen

El plugin `DC_CPC_CotizacionEmbarque` es esencial para la eficiencia y precisión en la generación de cotizaciones de embarque en Dynacorp, permitiendo la automatización, el control de calidad y la optimización comercial del proceso.