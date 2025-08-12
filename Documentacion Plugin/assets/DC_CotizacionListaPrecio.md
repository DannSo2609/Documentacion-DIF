# 📄 Documentación del Plugin: Creación y Asignación Automática de Productos desde Lista de Precios en Cotización

## Nombre del Plugin

DC_CotizacionListaPrecio.Proceso

## Acción

Update (Post-Operation)

## Entidad Principal

dc_cotizacion

## Descripción General

Este plugin se ejecuta automáticamente al actualizar una cotización
(dc_cotizacion). Su objetivo es traer y crear automáticamente productos
asociados a la cotización basándose en la lista de precios seleccionada.
Además, actualiza campos clave en la cotización según la información de
la lista de precios y determina cantidades de productos en base a los
datos de la cotización (peso/volumen). La lógica automatiza la selección
de productos relevantes, agrupando por ratebase cuando aplica, y crea
productos en las entidades específicas: ventas, embarque, aduana y
transporte, dependiendo del tipo de negocio.

## Lógica Interna Principal

- \- Prevención de Recursividad: El plugin ejecuta su lógica solo si la
  profundidad de contexto es 1 (evita loops infinitos).

- \- Recuperación de Cotización y Lista de Precios: Recupera los datos
  de la cotización y la lista de precios asociada. Valida fechas de
  vigencia de la lista de precios (no vencida). Recupera puertos de
  origen/destino y sus países relacionados.

- \- Actualización de la Cotización: Actualiza la cotización con los
  datos de puertos, países, tipo de embarque, tipo de transporte,
  suplidor, estado, etc., obtenidos desde la lista de precios. Ajusta la
  fecha de cotización usando la zona horaria de Caracas.

- \- Procesamiento de Productos de la Lista de Precios: Consulta y
  filtra los productos de la lista de precios según el departamento
  (ventas, embarque, aduana, transporte). Para productos de embarque con
  ratebase, agrupa por producto y ratebase y selecciona el producto
  correcto según los valores de peso/volumen en la cotización. Para
  productos de aduana, transporte y ventas, crea los productos según la
  unidad de medida y los datos presentes en la cotización.

- \- Creación de Productos Asociados: Crea registros de productos en las
  entidades correspondientes (dc_productosdeembarque,
  dc_productosaduana, dc_productostransporte, dc_productosdeventas),
  estableciendo cantidades y precios según las reglas de negocio.

- \- Método Auxiliar Importante: CreateShipmentProduct: Método dedicado
  a la creación de productos de embarque, realiza los cálculos y
  asignación de campos según unidad de medida.

## Validaciones Incluidas

- \- Verifica la existencia y vigencia de la lista de precios.

- \- Valida la existencia de puertos y países asociados.

- \- Lanza excepciones informativas si algún campo clave no está
  presente o si la lista está vencida.

- \- Selecciona correctamente los productos por unidad de medida según
  los valores de la cotización.

## Entidades Involucradas

- \- dc_cotizacion (cotización principal)

- \- dc_listasdeprecios (lista de precios asociada)

- \- dc_productoslistadeprecios (productos fuente a crear)

- \- dc_productosdeembarque (productos creados de embarque)

- \- dc_productosaduana (productos creados de aduana)

- \- dc_productostransporte (productos creados de transporte)

- \- dc_productosdeventas (productos creados de ventas)

- \- dc_puertos, dc_pais (información geográfica complementaria)

## Métodos Auxiliares Importantes

CreateShipmentProduct: Crea un producto de embarque calculando la
cantidad según la unidad de medida y los valores de la cotización.

## Manejo de Errores

- \- Utiliza InvalidPluginExecutionException para informar de manera
  clara al usuario sobre listas vencidas o datos clave faltantes.

- \- Registra los errores en el servicio de trazas (tracingService) para
  facilitar el soporte y la depuración.

- \- Lanza el error sin modificar para visibilidad en CRM.

## Consideraciones Finales

- \- El plugin automatiza la creación y asignación de productos a la
  cotización según la lista de precios seleccionada, minimizando errores
  manuales y garantizando integridad de datos.

- \- Permite la extensión a más departamentos o reglas de negocio
  futuras.

- \- Facilita la trazabilidad y gestión eficiente de cotizaciones y
  productos asociados.

## Resumen

Este plugin permite la creación automática y confiable de productos en
cotizaciones, basándose en una lista de precios y la información de la
cotización, asegurando que siempre se creen los productos y cantidades
correctos según los datos ingresados y la vigencia de la lista.
