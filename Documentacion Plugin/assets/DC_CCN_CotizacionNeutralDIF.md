# 1. Descripción General

Este plugin de Dynamics CRM está diseñado para ejecutarse cuando se
marca el campo \'dc_crearct\' en una entidad del tipo
\'dc_cotizacionesnl\'. Su propósito es duplicar la información de una
cotización neutral (NL) y crear una nueva cotización estándar, junto con
sus productos asociados (dc_productosembarques). El proceso también
establece referencias a los países y puertos de origen/destino, además
de enlazar productos desde una lista de precios predefinida.

# 2. Flujo General del Proceso

El proceso general sigue los siguientes pasos:

1.  1\. Verifica si el campo \'dc_crearct\' es verdadero.

2.  2\. Recupera la cotización neutral original.

3.  3\. Obtiene referencias a país/puerto de origen y destino.

4.  4\. Crea una nueva cotización estándar \'dc_cotizacion\'.

5.  5\. Recupera los productos asociados a la cotización DIF.

6.  6\. Asocia productos desde la lista de precios y los copia como
    nuevos productos de embarque.

# 3. Detalle Técnico

## 3.1 Validación Inicial

Se asegura que el plugin no se ejecute de forma recursiva más de una vez
verificando Depth \<= 1.

## 3.2 Recuperación de Datos Base

Utiliza RetrieveRequest para obtener todos los atributos de la
cotización neutral origen. De ahí extrae los campos clave como puertos y
países.

## 3.3 Resolución de Entidades Relacionadas

Se ejecutan QueryExpressions para obtener los EntityReference de los
países y puertos de origen y destino utilizando su \'dc_codigoiso\'.

## 3.4 Creación de Nueva Cotización

Se crea una nueva entidad \'dc_cotizacion\' con los valores copiados de
la cotización NL original. Incluye atributos como fecha de cotización,
tipo de transporte, operación, etc.

## 3.5 Copiado de Productos

Se consultan los productos \'dc_productosembarques\' relacionados a la
cotización original. Cada producto se compara contra la lista de precios
predefinida y, si existe coincidencia, se crea un nuevo producto de
embarque con los datos encontrados.

## 3.6 Manejo de Errores

Cualquier excepción que se produzca durante la ejecución es capturada y
registrada mediante el servicio de trazado (ITracingService).

# 4. Consideraciones Especiales

\- La lista de precios utilizada está hardcodeada con el ID
\'4780392f-199e-ef11-8a69-6045bdfec0f2\'.

\- El suplidor neutral tiene un ID fijo
\'327da440-6c48-ee11-be6d-000d3a1f642a\'.

\- Se espera encontrar exactamente un país y un puerto para cada código
ISO.

# 5. Fin del Documento

Este documento describe de forma estructurada el propósito, lógica y
flujo del plugin DC_CCN_CotizacionNeutralDIF implementado en Dynamics
CRM.
