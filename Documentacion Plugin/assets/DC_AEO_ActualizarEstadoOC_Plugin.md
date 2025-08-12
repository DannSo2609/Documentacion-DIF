# Nombre del Plugin:

DC_AEO_ActualizarEstadoOC.Proceso

# Objetivo del Plugin:

Actualizar el estado de los productos asociados a una Orden de Compra
(OC), según el área correspondiente (ventas, embarque, aduana,
transporte), cuando la OC se guarda o modifica.

# 1. ¿Cuándo se ejecuta el plugin?

El plugin se activa automáticamente al modificar una Orden de Compra en
el sistema.

# 2. Requisitos previos para que el plugin actúe

El plugin solo actúa si existen productos asociados a la Orden de Compra
y cada producto tiene un valor en el campo \"Transacción Producto\".

# 3. ¿Qué hace el sistema?

1\. Recupera los productos relacionados a la OC.\
2. Verifica si la OC tiene activado el campo \'Integrar a Dynamics\'.\
3. Clasifica los productos por departamento (ventas, embarque, aduana,
transporte).\
4. Por cada producto con transacción válida, busca sus registros
relacionados en la entidad del área.\
5. Actualiza el estado del producto según si está integrado o no:\
- 2 = Integrado\
- 1 = No integrado

# 4. Estados posibles de los productos

  -----------------------------------------------------------------------
  Valor                               Descripción
  ----------------------------------- -----------------------------------
  1                                   No integrado

  2                                   Integrado
  -----------------------------------------------------------------------

# 5. Entidades y campos involucrados

  -------------------------------------------------------------------------
  Entidad                  Campo                    Descripción
  ------------------------ ------------------------ -----------------------
  dc_ordenesdecompra       dc_integraradynamics     Indica si se debe
                                                    integrar a Dynamics

  dc_productosoc           dc_transaccionproducto   Identificador de
                                                    transacción del
                                                    producto

  dc_productosoc           dc_departamento          Departamento del
                                                    producto (ventas,
                                                    embarque, aduana,
                                                    transporte)

  dc_productosdeembarque   dc_estadosdeoc           Campo a actualizar con
                                                    estado OC

  dc_productosaduana       dc_estadodeoc            Campo a actualizar con
                                                    estado OC

  dc_productostransporte   dc_estadodeoc            Campo a actualizar con
                                                    estado OC
  -------------------------------------------------------------------------

# 6. Clasificación por departamentos

  -----------------------------------------------------------------------
  Valor OptionSet                     Departamento
  ----------------------------------- -----------------------------------
  0                                   Ventas

  1                                   Embarque

  2                                   Aduana

  3                                   Transporte
  -----------------------------------------------------------------------

# 7. Mensajes de error comunes

\- Departamento no reconocido: el valor del campo \'dc_departamento\' no
está dentro del rango permitido.\
- Área no reconocida: nombre de entidad no válido para el área
determinada.\
- Producto sin departamento definido: si se desea validar este caso, se
puede activar la validación comentada en el código fuente.

# 8. Recomendaciones

\- Asegúrese de que todos los productos tengan un departamento y
transacción definidos.\
- Verifique que las entidades relacionadas tengan los campos de estado
correctamente configurados.

# 9. Contacto y soporte

Para soporte técnico o ajustes en el proceso, contactar al área de
desarrollo o soporte Dynamics CRM.
