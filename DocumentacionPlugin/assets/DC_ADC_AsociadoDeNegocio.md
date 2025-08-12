# Nombre del Plugin:

DC_ADC_AsociadoDeNegocio.Proceso

# Objetivo del Plugin:

Generar automáticamente el campo \"Dirección Compuesta\" en los
registros de Asociado de Negocios combinando los campos de dirección
individuales.

# 1. ¿Cuándo se ejecuta el plugin?

El plugin se activa automáticamente cuando se crea o actualiza un
registro de \"Asociado de Negocios\".

# 2. ¿Qué hace el sistema?

1\. Recupera los campos individuales de dirección del registro de
Asociado de Negocios:\
- Calle\
- Número\
- Ciudad\
- Estado\
- Código Postal\
- País\
2. Combina los campos disponibles en una sola cadena separada por
comas.\
3. Guarda el resultado en el campo \"Dirección Compuesta\"
(dc_direccioncompuesta).

# 3. Campos involucrados

  -----------------------------------------------------------------------
  Campo                   Entidad                 Descripción
  ----------------------- ----------------------- -----------------------
  dc_calle                dc_asociadodenegocios   Nombre de la calle

  dc_numero               dc_asociadodenegocios   Número de calle o local

  dc_ciudad               dc_asociadodenegocios   Ciudad

  dc_estado               dc_asociadodenegocios   Estado o provincia

  dc_codigopostal         dc_asociadodenegocios   Código postal

  dc_pais                 dc_asociadodenegocios   País (referencia a
                                                  entidad relacionada)

  dc_direccioncompuesta   dc_asociadodenegocios   Campo calculado y
                                                  actualizado por el
                                                  plugin
  -----------------------------------------------------------------------

# 4. Reglas de construcción de la dirección compuesta

\- Cada parte de la dirección se agrega solo si tiene un valor.\
- Las partes se concatenan en el siguiente orden: Calle, Número, Ciudad,
Estado, Código Postal, País.\
- Cada parte se separa con una coma y un espacio.\
- Si alguna parte está vacía, se omite sin afectar el resto.

# 5. Validaciones del sistema

\- El plugin no se ejecuta si se activa desde un proceso interno que ya
está en profundidad \> 1.\
- El campo \'País\' es una referencia (lookup), por lo que se usa el
nombre del país.

# 6. Recomendaciones

\- Asegúrese de llenar correctamente los campos de dirección al crear o
actualizar el Asociado de Negocios.\
- Verifique que el campo País tenga un valor válido (nombre visible en
el lookup).\
- El campo Dirección Compuesta será sobrescrito automáticamente por el
sistema.

# 7. Contacto y soporte

Para soporte técnico o ajustes en el proceso, contactar al área de
desarrollo o soporte Dynamics CRM.
