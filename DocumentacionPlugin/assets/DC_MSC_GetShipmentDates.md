**Documentación del Plugin: GethblMSC - Actualización de Fechas de
Embarque desde API MSC**

**🧩 Nombre del Plugin**

**DC_MSC_GetShipmentDates.Services.GethblMSC**

**⚙️ Evento que lo Dispara**

**Update (Post-Operation) sobre la entidad dc_embarques**

**🗂️ Entidad Principal**

- **dc_embarques**

**🎯 Objetivo del Plugin**

**Consultar una API externa de MSC para obtener eventos relacionados al
número de booking (dc_booking) y actualizar campos del embarque como:**

- **dc_etd (Estimated Time of Departure)**

- **dc_eta (Estimated Time of Arrival)**

- **dc_atd (Actual Time of Departure)**

- **dc_ata (Actual Time of Arrival)**

- **dc_barcooaeronave (Barco asignado)**

- **dc_viaje (Viaje asignado)**

**📦 Campos Utilizados (Entrada / Salida)**

**En la entidad dc_embarques:**

  -----------------------------------------------------------------------
  **Campo**               **Descripción**
  ----------------------- -----------------------------------------------
  **dc_booking**          **Booking MSC usado para consultar la API**

  **dc_etd**              **Fecha estimada de salida**

  **dc_eta**              **Fecha estimada de llegada**

  **dc_atd**              **Fecha real de salida**

  **dc_ata**              **Fecha real de llegada**

  **dc_barcooaeronave**   **Barco asignado al embarque**

  **dc_viaje**            **Número de viaje**

  **dc_puertoorigen**     **Puerto de origen del embarque**

  **dc_puertodestino**    **Puerto de destino del embarque**

  **dc_hbl**              **HBL asociado al embarque**
  -----------------------------------------------------------------------

**🌐 API Externa Consultada**

- **Se realiza un llamado a
  connectToServiceApi.GetHBLByBookingNumber(bookingNumber)**

- **La respuesta debe contener eventos en formato JSON relacionados al
  booking.**

**📆 Eventos Relevantes Procesados**

  -----------------------------------------------------------------------
  **Evento en API**                            **Campo Actualizado**
  -------------------------------------------- --------------------------
  **\"Vessel Departure\"**                     **dc_etd**

  **\"Export Loaded on Vessel\"**              **dc_atd**

  **\"Estimated Time of Arrival\"**            **dc_eta**

  **\"Import Discharged from Vessel\"**        **dc_ata**
  -----------------------------------------------------------------------

**🛠️ Lógica de Procesamiento**

1.  **Se recupera el embarque desde CRM usando el ID de la entidad.**

2.  **Si tiene número de booking (dc_booking), se consulta la API de
    MSC.**

3.  **Se deserializa la respuesta y se filtran eventos relevantes.**

4.  **Se comparan fechas nuevas vs actuales, y si hay diferencias:**

    - **Se actualizan los campos dc_etd, dc_eta, dc_atd, dc_ata.**

5.  **Si se detecta un nuevo barco y/o viaje, se actualiza
    dc_barcooaeronave y dc_viaje.**

6.  **Si no existe el barco en CRM, se crea automáticamente.**

7.  **Si hay actualizaciones:**

    - **Se guarda el embarque.**

    - **Se envía correo con resumen de los cambios detectados al equipo
      configurado (por teamid).**

**📧 Envío de Correo de Resumen**

- **Se envía a los miembros del equipo con ID:**

**CopyEdit**

**7f06d364-1c2c-f011-8c4e-00224824bbf2**

- **El correo contiene:**

  - **HBL y Booking**

  - **Cambios detectados (fechas, barco, viaje)**

  - **Link al embarque en Dynamics**

**🔐 Métodos Auxiliares**

  -------------------------------------------------------------------------------
  **Método**                    **Función**
  ----------------------------- -------------------------------------------------
  **GetPortName(Guid)**         **Devuelve el código ISO del puerto dado su ID.**

  **ObtenerOCrearBarco()**      **Retorna un EntityReference de barco; si no
                                existe, lo crea.**

  **ObtenerBarco()**            **Busca barcos por nombre (IMO).**

  **CrearBarco()**              **Crea un nuevo barco en dc_barcosyaeronaves.**

  **EnviarCorreoResumen()**     **Envía un correo HTML con los cambios
                                detectados.**

  **IsDateBeignUpdated()**      **Verifica si la nueva fecha es distinta a la
                                registrada.**

  **ConvertirFormatoFecha()**   **Convierte cadena ISO 8601 a DateTime.**
  -------------------------------------------------------------------------------

**🧪 Casos de Prueba Recomendados**

  -----------------------------------------------------------------------
  **Escenario**                   **Resultado Esperado**
  ------------------------------- ---------------------------------------
  **Booking válido con eventos    **Campos actualizados y correo
  nuevos**                        enviado**

  **Booking válido sin cambios**  **No hay actualización ni correo**

  **Puerto origen/destino         **No se actualiza ni se llama a la
  faltante**                      API**

  **Fecha nueva es igual a la     **No se actualiza**
  registrada**                    

  **Nuevo barco no existe en      **Se crea barco nuevo y se asigna al
  CRM**                           embarque**

  **API devuelve \"NotFound\"**   **Plugin no hace nada y continúa**

  **Error al parsear JSON**       **Lanza excepción con detalle del
                                  error**
  -----------------------------------------------------------------------

**⚠️ Observaciones Técnicas**

**✅ Validaciones consistentes para evitar duplicar fechas o barcos
innecesariamente.\
✅ Uso de clases auxiliares bien organizadas.\
❗ Importante: el campo dc_nombres del barco se usa como código IMO.
Asegúrate que esto es correcto; muchas veces se usa dc_imoiata.\
❗ El método ObtenerBarco puede traer más de un registro si hay
duplicados. Se toma el primero.\
❗ No hay validación para múltiples eventos con la misma descripción; se
aplica en orden de llegada.**
