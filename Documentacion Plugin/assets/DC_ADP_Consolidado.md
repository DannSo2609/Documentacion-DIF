# Nombre del Plugin:

DC_ADP_Consolidado.Proceso

# Objetivo del Plugin:

Automatizar el proceso de envío de un consolidado (MAWB/MBL) al equipo
de tráfico, validando que los embarques relacionados cumplan ciertas
condiciones.

# 1. ¿Cuándo se ejecuta el plugin?

El plugin se activa automáticamente cuando se marca la opción "Enviar a
Tráfico" (dc_enviaratrafico) en un consolidado (registro MAWB o MBL).

# 2. Requisitos previos antes de enviar a tráfico

Antes de marcar \"Enviar a Tráfico\", asegúrese de que los embarques
relacionados con ese consolidado cumplan con las siguientes condiciones:

## 2.1 Fecha ATD obligatoria

Cada embarque debe tener la fecha ATD (Actual Time of Departure)
ingresada.

## 2.2 Embarque Facturado o con Permiso para Tráfico

El embarque debe estar facturado o tener el campo \"Permitir pasar a
tráfico\" activado.

# 3. ¿Qué hace el sistema si todos los embarques están correctos?

Si todos los embarques tienen fecha ATD y están facturados o permitidos
para tráfico, el plugin asigna automáticamente el consolidado al equipo
de tráfico.

# 4. ¿Cómo se configura el equipo de tráfico?

El equipo responsable de recibir los consolidado se configura en la
entidad Configuración del sistema (dc_configuracion), en el campo
\"Equipo de Tráfico\" (dc_equipotrafico).

# 5. Casos comunes de error

  ---------------------------------------------------------------------------
  Error                   Causa                       Solución
  ----------------------- --------------------------- -----------------------
  El embarque no tiene    El campo dc_atd está vacío  Ingresar la fecha ATD
  fecha ATD                                           en el embarque

  El embarque no está     Ambos campos (dc_facturar y Marcar uno de los dos
  facturado ni permitido  dc_permitirpasaratrafico)   campos como \'Sí\'
  para tráfico            están en \'No\'             según el procedimiento
                                                      del área

  No se encuentra la      No existe un registro en    Crear o actualizar la
  configuración del       dc_configuracion            configuración en la
  equipo de tráfico                                   entidad correspondiente
  ---------------------------------------------------------------------------

# 6. Campos involucrados

  --------------------------------------------------------------------------
  Campo                      Entidad                 Descripción
  -------------------------- ----------------------- -----------------------
  dc_enviaratrafico          Consolidado (MAWB/MBL)  Campo de activación del
                                                     plugin

  dc_atd                     Embarques               Fecha de salida real
                                                     del embarque

  dc_facturar                Embarques               Indica si el embarque
                                                     ya fue facturado

  dc_permitirpasaratrafico   Embarques               Permite pasar a tráfico
                                                     sin estar facturado

  dc_hbl                     Embarques               Número de conocimiento
                                                     de embarque hijo

  dc_equipotrafico           Configuración           Equipo responsable del
                                                     tráfico (destinatario
                                                     del consolidado)
  --------------------------------------------------------------------------

# 7. Flujo del proceso automatizado

1\. Usuario marca "Enviar a Tráfico" en un consolidado.\
2. El sistema valida todos los embarques relacionados.\
3. Si hay algún error, el sistema muestra un mensaje y no continúa.\
4. Si todos cumplen, se asigna automáticamente el consolidado al equipo
de tráfico.

# 8. Recomendaciones

\- Verifique la información de cada embarque antes de enviar un
consolidado a tráfico.\
- Asegure que la configuración del equipo de tráfico esté actualizada.\
- Si recibe errores, revise el número de HBL indicado en el mensaje para
corregir directamente ese embarque.

# 9. Contacto y soporte

Para soporte técnico o ajustes en el proceso, contactar al área de
desarrollo o soporte Dynamics CRM.
