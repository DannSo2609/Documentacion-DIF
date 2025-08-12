Documentación del Plugin: Actualización de Embarques desde Consolidado

**Nombre del Plugin**

DC_AFE_Consolidado.Proceso

**Acción**

Update (Post-Operation)

**Entidad Principal**

dc_consolidado

**Descripción General**

Este plugin se ejecuta sobre la entidad dc_consolidado y su propósito es
sincronizar los campos de fecha relevantes con todos los registros de
dc_embarques que estén relacionados mediante el valor del campo
dc_mawbmbl.\
\
Cuando se actualiza un consolidado, el plugin busca todos los embarques
asociados (donde dc_mawbmbl del embarque coincide con el Id del
consolidado) y copia en cada uno de ellos las siguientes fechas, solo si
estas tienen valores válidos:

\- dc_proforma\
- dc_manifiesto\
- dc_prealerta\
- dc_documentobl\
- dc_fechafactura\
- dc_enviadoalmacen\
- dc_llegadaalmacen\
- dc_descargaalmacen

**Lógica Interna**

1\. Recupera el registro de dc_consolidado desde el contexto.\
2. Extrae todas las fechas relevantes.\
3. Busca los embarques relacionados por el campo dc_mawbmbl.\
4. Compara cada campo de fecha. Si el valor en el consolidado es válido
(!= 0001-01-01) y diferente del valor en el embarque, se actualiza.\
5. Se realiza Update para cada embarque modificado.

**Validaciones Incluidas**

\- Se asegura que cada campo de fecha no esté vacío (InvalidDate).\
- Solo se actualiza si el valor en el embarque es distinto al del
consolidado, para evitar actualizaciones innecesarias.

**Entidades Involucradas**

\- dc_consolidado (origen de datos)\
- dc_embarques (destino de actualización)

**Método Auxiliar**

ObtenerEmbarques(ContextBase context):\
Filtra los registros de dc_embarques donde dc_mawbmbl coincide con el ID
del consolidado.

**Manejo de Errores**

Captura cualquier excepción lanzada durante el proceso y la traza para
facilitar la depuración en el registro de plugins.
