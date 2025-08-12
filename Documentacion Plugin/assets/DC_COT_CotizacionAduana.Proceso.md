# 📄 Documentación del Plugin

## Creación Automática de Aduana desde Cotización

### 🔹 Nombre del Plugin

DC_COT_CotizacionAduana.Proceso

### 🔹 Acción Disparadora

\- Evento: Update (Post-Operation)\
- Entidad Principal: dc_cotizacion

### 🧩 Descripción General

Este plugin se ejecuta automáticamente al actualizar una cotización
(dc_cotizacion). Su función principal es crear un registro de aduana
(dc_aduanas), y asociar tanto productos como documentos relacionados,
siempre que la cotización cumpla ciertas condiciones.\
\
El proceso automatiza la creación del expediente de aduana, la
vinculación de registros relacionados y actualiza entidades asociadas,
garantizando la integridad de datos y eliminando la intervención manual.

## 🔧 Lógica Interna

### 1. Prevención de Recursividad

El plugin verifica que la profundidad del contexto (Depth) sea 1,
evitando ejecuciones recursivas.

### 2. Validación de Condiciones

\- La cotización debe estar aprobada (dc_aprobarcotizacion = true).\
- Debe haberse solicitado la creación de órdenes de trabajo de aduana
(dc_crearordenesdetrabajoaduanas = true).

### 3. Creación del Registro de Aduana

Se validan y recopilan todos los campos obligatorios y se asignan al
nuevo registro de aduana según las reglas de negocio.

### 4. Actualización de Entidades Relacionadas

Cotización, documentos, productos, embarques y transporte se actualizan
para referenciar la nueva aduana.

### 5. Validación de Productos

Antes de actualizar los productos, se valida que todos tengan moneda de
compra y de venta.

### 6. Asignación del Responsable

El ejecutivo de aduana, si existe, se asigna como dueño. Si no, se
conserva el dueño original.

## ✅ Validaciones Incluidas

\- Cotización debe estar aprobada.\
- Todos los campos obligatorios deben estar presentes.\
- Todos los productos deben tener moneda de compra y venta.\
- Si falta algún dato, el proceso se cancela con mensaje claro.

## 📘 Entidades Involucradas

\- dc_cotizacion\
- dc_aduanas\
- dc_productosaduana\
- dc_documentosoperaciones\
- dc_embarques (si aplica)\
- dc_transporte (si aplica)

## 🛠️ Métodos Auxiliares Clave

\- CrearAduanaPorCotizacion: Crea la aduana validando campos
requeridos.\
- ComprobarProductos: Valida que cada producto tenga moneda definida.\
- ObtenerProductos / ObtenerDocumentos: Recupera los registros
relacionados.\
- ActualizarProductos / ActualizarDocumentos: Relaciona entidades a la
aduana.\
- FieldDecimalValue: Copia valores decimales si el campo existe.\
- UpdateEntity\<T\>: Actualiza cualquier campo en cualquier entidad.

## ⚠️ Manejo de Errores

\- Lanza InvalidPluginExecutionException con mensajes claros.\
- Usa tracingService para registrar cada paso y errores.\
- Relanza la excepción para asegurar visibilidad en CRM.

## 📝 Consideraciones Finales

Este plugin mejora la eficiencia operativa al automatizar la creación de
aduanas desde cotizaciones, eliminando errores manuales y garantizando
cumplimiento de reglas de negocio.\
Facilita la colaboración entre áreas comerciales y operativas y es
escalable a futuro.

## 🧾 Resumen

Este plugin permite que, al aprobar una cotización y marcar la opción de
generar órdenes de trabajo aduaneras, el sistema cree automáticamente la
aduana, valide los datos, relacione los productos y documentos, y
actualice las entidades relacionadas, todo garantizando la integridad y
trazabilidad.
