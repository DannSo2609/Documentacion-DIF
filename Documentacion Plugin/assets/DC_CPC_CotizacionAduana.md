# Documentación del Plugin DC_CPC_CotizacionAduana

## Sistema: Dynacorp

---

## Descripción General

El plugin `DC_CPC_CotizacionAduana` automatiza la carga y asociación de productos del cliente a una cotización de aduanas dentro del sistema Dynacorp (Dynamics 365). Su función principal es identificar y asociar automáticamente los productos relevantes a la cotización de aduana, tomando como base las listas de precios del cliente y los parámetros logísticos y comerciales clave, como puertos, suplidor, cantidades y tipos de operación.

---

## Propósito

- Automatizar la carga de productos del cliente en cotizaciones de aduana.
- Asegurar que la selección de productos se base en listas de precios activas y parámetros logísticos y comerciales específicos.
- Evitar la duplicidad de productos ya existentes en la cotización.
- Facilitar la preparación de propuestas comerciales y la gestión aduanera.

---

## Funcionamiento Detallado

### 1. Condición de Activación

- El plugin se activa únicamente si el campo `dc_cargarprodcutosdelclienteadu` está habilitado en la cotización.
- Antes de proceder, valida que todos los datos críticos estén presentes: cliente, puertos de origen y destino, cantidad, tipo de transporte y suplidor de aduana. Si falta alguno, el proceso se detiene y muestra un mensaje de error claro.

### 2. Obtención y Validación de Datos

- Recupera la información clave de la cotización: cliente, puertos, suplidor de aduana, tipo de embarque, incoterms, tipo de operación, tipo de transporte y cantidad a cargar.
- Verifica que los datos sean válidos y suficientes para una búsqueda precisa de productos.

### 3. Búsqueda de Productos Existentes

- Identifica los productos de aduana ya asociados a la cotización, lo que permite evitar crear duplicados.

### 4. Expansión de Puertos

- Busca no solo los puertos seleccionados directamente, sino también grupos de puertos relacionados, ampliando las posibilidades de emparejamiento con listas de precios.

### 5. Combinación de Orígenes y Destinos

- Genera todas las combinaciones posibles entre los puertos de origen y destino, permitiendo un filtrado exhaustivo en las listas de precios.

### 6. Búsqueda de Listas de Precios

- Busca todas las listas de precios asociadas al cliente, filtrando solo aquellas que cumplen criterios de unidad de negocio, suplidor, puertos, fechas vigentes y coincidencia con los parámetros logísticos.
- Si no hay suplidor seleccionado, el proceso se detiene indicando el requerimiento.

### 7. Selección y Creación de Productos

- Para cada lista de precios válida, busca los productos que correspondan a los criterios de operación, embarque y transporte.
- Verifica que cada producto no exista ya en la cotización.
- Crea los productos en la cotización de aduana incluyendo todos los datos relevantes: producto, suplidor, lista de precios, puertos, unidad de medida, costos y precios, y cantidad (aplicando reglas de negocio según la unidad de medida).

### 8. Inclusión de Productos Hijos

- Para cada producto de lista de precios, busca productos hijos (subproductos) y los crea en la cotización si no existen, aplicando la misma lógica de validación y carga.

### 9. Manejo de Errores

- Todos los errores que ocurran durante la creación de productos son capturados y almacenados en una lista interna, permitiendo que el proceso continúe con los demás productos y reportando solo los fallos críticos.

---

## Beneficios

- **Automatización:** Elimina la carga manual y repetitiva de productos del cliente en cotizaciones de aduana.
- **Precisión:** Solo se crean productos que cumplen todos los filtros y no están previamente asociados a la cotización.
- **Cobertura Completa:** Considera grupos de puertos y productos hijos para incluir todos los conceptos logísticos y comerciales relevantes.
- **Mensajes Claros:** Proporciona retroalimentación al usuario en caso de datos faltantes o inconsistentes.
- **Eficiencia Comercial:** Agiliza la generación de propuestas y el análisis logístico para clientes.

---

## Casos de Uso

- **Preparación de cotizaciones aduaneras:** Cuando un usuario de Dynacorp necesita cargar rápidamente todos los productos y servicios aplicables en una cotización de aduana para un cliente.
- **Automatización de propuestas comerciales:** Al generar presupuestos y propuestas para clientes, el sistema asocia automáticamente los productos y servicios relevantes.
- **Validación logística:** Permite evaluar rápidamente la viabilidad y costos de una operación aduanera según parámetros actuales.

---

## Consideraciones

- El plugin depende de la correcta parametrización de listas de precios, productos, suplidores y relaciones de puertos.
- Si falta algún dato clave, el proceso no se ejecuta y proporciona un mensaje explicativo.
- La inclusión de productos hijos garantiza que se contemplen todos los conceptos asociados a la operación.
- El registro de errores permite diagnóstico posterior sin interrumpir el flujo principal.

---

## Resumen

El plugin `DC_CPC_CotizacionAduana` es esencial para la eficiencia comercial en Dynacorp, automatizando la carga de productos del cliente en cotizaciones de aduana y garantizando rapidez, precisión e integridad en las propuestas y procesos logísticos.