**📄 Documentación del Plugin: Carga Automática de Productos desde Lista
de Precio en Embarque**

**🧩 Nombre del Plugin**

**DC_EmbarqueListaPrecio.Proceso**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation)**

**🗂️ Entidad Principal**

**dc_embarques**

**🎯 Objetivo**

**Este plugin automatiza la carga de productos al embarque basándose en
una lista de precios previamente definida por el usuario. Evalúa
condiciones como tipo de embarque, operación, incoterms y tipo de
contenedor para seleccionar los productos correctos de la lista y crear
registros en dc_productosdeembarque.**

**🧾 Campos Involucrados**

**En la entidad dc_embarques:**

  ------------------------------------------------------------------------
  **Campo técnico**            **Descripción**
  ---------------------------- -------------------------------------------
  **dc_cargarlistadeprecio**   **Referencia a la lista de precios a
                               cargar**

  **dc_piescubico**            **Volumen en pies cúbicos**

  **dc_metrocubico**           **Volumen en metros cúbicos**

  **dc_centimetroscubicos**    **Volumen en centímetros cúbicos**

  **dc_toneladas**             **Peso en toneladas**

  **dc_kilogramos**            **Peso en kilogramos**

  **dc_libras**                **Peso en libras**

  **dc_puertoorigen**          **Puerto de origen**

  **dc_puertodestino**         **Puerto de destino**

  **dc_incoterms**             **Condiciones de entrega**

  **dc_tipodeembarque**        **Tipo de embarque (FCL, LCL, etc.)**

  **dc_tipodeoperacion**       **Tipo de operación**

  **dc_tipodetransporte**      **Tipo de transporte**
  ------------------------------------------------------------------------

**En la entidad dc_productoslistadeprecios:**

  -----------------------------------------------------------------------
  **Campo técnico**            **Descripción**
  ---------------------------- ------------------------------------------
  **dc_tipodeembarque**        **Tipo de embarque del producto**

  **dc_tipodeoperacion**       **Tipo de operación del producto**

  **dc_incoterms**             **Incoterms aplicables**

  **dc_tipodecontenedor**      **Tipo de contenedor (opcional)**

  **dc_ratebases**             **Base de tarifa (si aplica)**

  **dc_unidaddemedida**        **Unidad de medida (kg, m³, etc.)**

  **dc_preciodeventa**         **Precio de venta del producto**

  **dc_producto**              **Referencia al producto maestro**
  -----------------------------------------------------------------------

**🔁 Lógica del Plugin**

1.  **Recuperación de datos del embarque: Se obtiene el embarque y la
    lista de precios asociada.**

2.  **Consulta de productos activos de la lista de precios.**

3.  **Verificación de condiciones:**

    - **Coincidencia entre tipo de embarque y tipo de operación.**

    - **Coincidencia de incoterms.**

    - **Presencia (o no) de tipo de contenedor.**

4.  **Cálculo de cantidad:**

    - **Si la unidad de medida es por contenedor (valor 8), se toma la
      cantidad basada en el número de contenedores del tipo
      correspondiente.**

    - **Si se utiliza peso o volumen (kg, toneladas, m³, etc.), se usa
      la unidad relacionada.**

    - **Si hay múltiples unidades (ej. kg y m³), se escoge la que genere
      mayor valor de venta.**

5.  **Creación del registro en dc_productosdeembarque con la cantidad y
    datos comerciales del producto seleccionado.**

**🧮 Tipos de Contenedores Manejados**

  -----------------------------------------------------------------------
  **Tipo Contenedor (valor)**          **Descripción**
  ------------------------------------ ----------------------------------
  **0**                                **20\'**

  **1**                                **40\'**

  **2**                                **40HC**

  **3 - 14**                           **Otros tipos especiales**
  -----------------------------------------------------------------------

**📦 Cálculo y Agrupación por Ratebase**

- **Los productos que tienen una dc_ratebases asignada se agrupan por
  producto y base tarifaria.**

- **Dentro de cada grupo, se selecciona el producto cuya combinación
  unidad \* precio genere el mayor valor.**

**🧪 Validaciones**

- **Se omiten productos sin coincidencia en tipo de embarque, operación
  o incoterms.**

- **Se evita la creación de productos si no hay cantidad relevante
  asociada.**

- **Si hay múltiples candidatos con diferentes unidades, se prioriza el
  que represente mayor valor monetario.**

- **Se verifica que cada contenedor tenga un tipo válido antes de ser
  considerado.**

**🧾 Resultado Esperado**

- **Se crean uno o más registros de dc_productosdeembarque asociados al
  embarque actual.**

- **Cada producto se crea con la cantidad, precio, unidad de medida y
  referencias correspondientes.**

- **El proceso es automático al actualizar el embarque si tiene una
  lista de precio vinculada.**
