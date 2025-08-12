# 📄 Documentación del Plugin: Creación y Asignación Automática de Productos desde Perfil de Cliente en Cotización

## Nombre del Plugin

DC_CP_CotizacionPerfil.Proceso

## Acción

Update (Post-Operation)

## Entidad Principal

dc_cotizacion

## Descripción General

Este plugin se ejecuta automáticamente al actualizar una cotización
(dc_cotizacion). Su objetivo es traer y crear automáticamente productos
asociados a la cotización basándose en el perfil de cliente seleccionado
(dc_perfilcliente). Además, actualiza campos clave en la cotización
según la información del perfil y determina cantidades de productos en
base a los datos de la cotización (peso/volumen). La lógica automatiza
la selección de productos relevantes, agrupando por ratebase cuando
aplica, y crea productos en las entidades específicas: ventas, embarque,
aduana y transporte, dependiendo del tipo de negocio, usando las reglas
de negocio y los valores del perfil del cliente.

## Lógica Interna Principal

- Prevención de Recursividad: El plugin ejecuta su lógica solo si la
  profundidad de contexto es 1 (evita loops infinitos).

- Recuperación de Cotización y Perfil de Cliente: Recupera los datos de
  la cotización y el perfil de cliente asociado. Valida obligatoriamente
  la presencia de puertos de origen y destino, además de países
  asociados.

- Actualización de la Cotización: Actualiza la cotización con los datos
  de puertos, países, tipo de embarque, incoterms, tipo de operación,
  cliente, estado, etc., obtenidos desde el perfil. Ajusta la fecha de
  cotización usando la hora actual.

- Procesamiento de Productos del Perfil: Consulta y filtra los productos
  del perfil según el estado y el perfil. Para productos con ratebase,
  agrupa por producto y ratebase y selecciona el producto correcto según
  los valores de peso/volumen en la cotización y el mayor valor de
  venta. Para productos sin ratebase, crea el producto directamente
  usando las reglas de unidad de negocio y unidad de medida.

- Creación de Productos Asociados: Crea registros de productos en las
  entidades correspondientes (dc_productosdeembarque,
  dc_productosaduana, dc_productostransporte, dc_productosdeventas),
  estableciendo cantidades y precios según las reglas de negocio y los
  valores de la cotización.

- Método Auxiliar Importante: CreateShipmentProduct: Método dedicado a
  la creación de productos, realiza los cálculos y asignación de campos
  según unidad de medida, tipo de negocio, perfil y valores de la
  cotización.

## Validaciones Incluidas

- Verifica la existencia y validez del perfil de cliente y de los
  puertos y países asociados.

- Lanza excepciones informativas si algún campo clave no está presente.

- Selecciona correctamente los productos por unidad de medida y mayor
  valor de venta según los valores de la cotización y el perfil.

## Entidades Involucradas

- dc_cotizacion (cotización principal)

- dc_perfilescliente (perfil de cliente asociado)

- dc_productosdeperfil (productos fuente a crear)

- dc_productoslistadeprecios (productos fuente complementarios)

- dc_productosdeembarque (productos creados de embarque)

- dc_productosaduana (productos creados de aduana)

- dc_productostransporte (productos creados de transporte)

- dc_productosdeventas (productos creados de ventas)

- dc_puertos, dc_pais (información geográfica complementaria)

## Métodos Auxiliares Importantes

CreateShipmentProduct: Crea un producto en la entidad correspondiente
calculando la cantidad según la unidad de medida y los valores de la
cotización.

## Manejo de Errores

- Utiliza InvalidPluginExecutionException para informar de manera clara
  al usuario sobre datos clave faltantes en el perfil o la cotización.

- Registra los errores en el servicio de trazas (tracingService) para
  facilitar el soporte y la depuración.

- Lanza el error sin modificar para visibilidad en CRM.

## Consideraciones Finales

El plugin automatiza la creación y asignación de productos a la
cotización según el perfil de cliente seleccionado, minimizando errores
manuales y garantizando integridad de datos. Permite la extensión a más
departamentos o reglas de negocio futuras. Facilita la trazabilidad y
gestión eficiente de cotizaciones y productos asociados.

## Resumen

Este plugin permite la creación automática y confiable de productos en
cotizaciones, basándose en el perfil de cliente y la información de la
cotización, asegurando que siempre se creen los productos y cantidades
correctos según los datos ingresados y la lógica de negocio definida.
