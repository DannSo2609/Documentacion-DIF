# Documentación del Plugin DC_CPC_Aduana

## Sistema: Dynacorp

---

## Descripción General

El plugin `DC_CPC_Aduana` automatiza la carga de productos del cliente en un registro de Aduana dentro del sistema Dynacorp (Dynamics 365). Su función principal es identificar y asociar de manera automática todos los productos relevantes a la Aduana, basándose en las listas de precios del cliente y considerando los parámetros logísticos clave, como puertos de origen y destino, tipos de operación, transporte, y cantidades.

---

## Propósito

- Facilitar la creación masiva y automática de productos vinculados a la Aduana.
- Garantizar que la selección de productos se base en las listas de precios y parámetros logísticos específicos del cliente.
- Evitar la duplicidad de productos ya existentes para la Aduana.
- Optimizar la operación logística y la preparación de la documentación aduanera.

---

## Funcionamiento Detallado

### 1. Control de Activación

- El plugin se activa únicamente si el campo `dc_cargarprodcutosdelcliente` está habilitado en el registro de Aduana.
- Antes de proceder, valida que todos los datos críticos estén presentes (cliente, puertos de origen y destino, cantidad, tipo de transporte). Si falta alguno, el proceso se detiene y muestra un mensaje de error claro al usuario.

### 2. Obtención de Datos Clave

- Recupera la información esencial del registro de Aduana: consignatario (cliente), puertos, tipo de embarque, incoterms, tipo de operación, tipo de transporte, cantidad a cargar y suplidor de embarque.

### 3. Búsqueda de Productos Existentes

- Identifica los productos que ya están cargados en la Aduana, para no duplicar información.

### 4. Expansión de Puertos

- Busca no solo los puertos de origen y destino directos, sino también grupos de puertos relacionados, ampliando las combinaciones posibles para encontrar listas de precios relevantes.

### 5. Combinación de Orígenes y Destinos

- Forma todas las combinaciones posibles entre puertos de origen y destino, permitiendo una búsqueda exhaustiva de productos según la logística real de la operación.

### 6. Búsqueda de Listas de Precios

- Busca y filtra todas las listas de precios asociadas al cliente, considerando únicamente aquellas que cumplen criterios de negocio (unidad de negocio, suplidor, puertos, fechas de validez, tipo de embarque y transporte).
- Solo se consideran listas válidas para la fecha actual y para los parámetros logísticos seleccionados.

### 7. Selección y Creación de Productos

- Para cada lista de precios válida, busca los productos correspondientes considerando:
  - Tipo de operación
  - Tipo de embarque
  - Tipo de transporte
- Verifica que el producto no exista ya en la Aduana antes de crearlo.
- Crea los productos en la entidad de productos de Aduana con toda la información relevante (producto, suplidor, lista de precios, puertos, unidad de medida, precio de compra y venta, cantidad).
- Aplica reglas de negocio para determinar la cantidad según la unidad de medida.

### 8. Inclusión de Productos Hijos

- Para cada producto de lista de precios, busca productos hijos (subproductos) relacionados y los crea también en la Aduana si no existen, aplicando la misma lógica de validación y carga.

### 9. Manejo de Errores

- Todos los errores que se produzcan durante la creación de productos son capturados y almacenados internamente, permitiendo que el proceso continúe con los demás productos y evitando la interrupción total del flujo.

---

## Beneficios

- **Automatización:** Ahorra tiempo al eliminar la carga manual de productos por parte del usuario.
- **Precisión:** Solo se crean productos que cumplen todos los filtros y no están previamente asociados a la Aduana.
- **Flexibilidad:** Considera grupos de puertos y productos hijos, cubriendo casos logísticos complejos.
- **Transparencia:** Mensajes de validación claros en caso de datos faltantes o incorrectos.
- **Eficiencia Operativa:** Facilita la preparación de trámites aduaneros y presupuestación para clientes.

---

## Casos de Uso

- **Preparación rápida de expedientes aduaneros:** Cuando un operador de Dynacorp necesita asociar rápidamente todos los productos posibles para una operación de importación/exportación.
- **Cotizaciones automáticas:** Al generar propuestas para clientes, el sistema puede cargar todos los productos y costos asociados de forma automática.
- **Validación de disponibilidad logística:** Permite conocer rápidamente qué productos están disponibles según la logística y contratos actuales del cliente.

---

## Consideraciones

- El plugin depende de la correcta parametrización de listas de precios, productos y relaciones de puertos en Dynacorp.
- Si faltan datos clave en la Aduana, el proceso se detiene con un mensaje explicativo.
- El manejo de productos hijos garantiza que se cubran todos los costes y servicios asociados.
- El manejo de errores internos permite robustez y continuidad, reportando únicamente los problemas críticos.

---

## Resumen

El plugin `DC_CPC_Aduana` es una pieza esencial para la eficiencia de las operaciones aduaneras en Dynacorp. Automatiza la carga de productos del cliente en Aduana, logrando mayor rapidez, precisión y control en los procesos logísticos y administrativos.