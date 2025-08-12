**📄 Documentación del Plugin: Consolidación Automática de Archivos BL**

**Nombre del Plugin**

**DC_Consolidar_XML.Proceso**

**Acción**

- **Update (Post-Operation)**

**Entidad Principal**

- **dc_filebl**

**Descripción General**

**Este plugin se ejecuta automáticamente al actualizar un archivo BL
(dc_filebl). Su propósito es consolidar toda la información relevante
del embarque a partir de los datos del archivo BL, creando registros
relacionados de consolidado, embarques (HBLs), contenedores y
distribuciones en las entidades correspondientes. El proceso automatiza
el flujo logístico, enlaza correctamente los datos y reduce errores
manuales. La lógica cubre desde la validación de ejecución, recuperación
de la información del archivo BL, hasta la creación y vinculación de
todos los registros logísticos asociados (consolidado, HBL,
contenedores, puertos, etc.) 1.**

**Lógica Interna Principal**

1.  **Prevención de Recursividad**

    - **El plugin solo ejecuta su lógica si la profundidad de contexto
      es 1 (evita loops infinitos) 1.**

2.  **Verificación de Procesamiento**

    - **Solo procede si el campo booleano dc_procesado del archivo BL
      está marcado como verdadero 1.**

3.  **Recuperación y Consolidación de Datos**

    - **Recupera el archivo BL con todos los datos clave (puertos,
      agente, embarcador, consignatario, etc.) 1.**

    - **Invoca el método de consolidación principal, que crea el
      registro de consolidado copiando y relacionando todos los
      atributos relevantes 1.**

4.  **Creación de Registros Asociados**

    - **Crea los registros de embarques (HBLs) asociados al consolidado,
      asignando fechas, puertos, descripción de carga, agentes,
      consignatarios y otros datos 1.**

    - **Crea o relaciona los contenedores correspondientes y genera la
      distribución de contenedores entre consolidado y HBL 1.**

    - **Vincula correctamente los puertos de origen y destino a través
      de búsqueda por código ISO 1.**

5.  **Limpieza y Validación de Textos**

    - **Limpia textos largos, como la descripción de la carga, y trunca
      si excede la longitud máxima permitida 1.**

    - **Valida la existencia de cada dato antes de asignarlo para evitar
      errores de referencia o nulos 1.**

6.  **Manejo de Relaciones y Referencias**

    - **Utiliza métodos auxiliares para buscar y relacionar puertos y
      contenedores según códigos y números de BL 1.**

    - **Vuelve a buscar entidades recién creadas para obtener sus
      referencias y asociarlas a otras nuevas 1.**

**Validaciones Incluidas**

- **Verifica la presencia y validez de todos los datos antes de asignar
  referencias 1.**

- **Controla que los textos no excedan la longitud máxima aceptada por
  CRM 1.**

- **Si no se encuentra un puerto o contenedor, asigna null para evitar
  referencias inválidas 1.**

- **El proceso sólo ejecuta su lógica de consolidación si el archivo BL
  está marcado como procesado 1.**

**Entidades Involucradas**

- **dc_filebl (archivo BL principal) 1**

- **dc_consolidado (registro consolidado creado) 1**

- **dc_embarques (embarques/HBLs asociados) 1**

- **dc_contenedores (contenedores creados/relacionados) 1**

- **dc_distribucioncontenedores (distribución de contenedores por
  consolidado/HBL) 1**

- **dc_fileblbilloflading (HBLs fuente) 1**

- **dc_fileblcontainer (contenedores fuente) 1**

- **dc_puertos (puertos de origen y destino) 1**

**Métodos Auxiliares Importantes**

- **SaveConsolidado: Crea un nuevo consolidado a partir del archivo BL y
  asigna todos los campos y referencias 1.**

- **SaveHbls: Crea registros de embarque (HBL) por cada HBL fuente y los
  vincula al consolidado 1.**

- **CreateHBLSContainers: Crea los registros de contenedores para cada
  HBL 1.**

- **SearchContainersForHBL: Recupera los registros de contenedores según
  su ID 1.**

- **SavefileBillOfLandingContainers: Asocia un contenedor a un
  consolidado y un HBL, creando la distribución correspondiente 1.**

- **QueryHBLs: Busca todos los HBLs asociados a un número master BL 1.**

- **SaveContenedor: Crea un contenedor consolidado a partir del
  contenedor fuente 1.**

- **QueryPorts: Busca la referencia a un puerto por código ISO 1.**

- **QueryContainers: Obtiene un contenedor por número master BL 1.**

**Manejo de Errores**

- **Utiliza InvalidPluginExecutionException para dar mensajes claros si
  ocurre algún problema de datos 1.**

- **Registra todos los errores en el servicio de trazas (tracingService)
  para facilitar soporte y debugging 1.**

- **Lanza el error sin modificar para asegurar visibilidad en CRM 1.**

**Consideraciones Finales**

- **El plugin automatiza la consolidación logística, asegurando
  integridad y trazabilidad de los datos en todos los registros
  asociados 1.**

- **Permite ampliaciones agregando más campos o entidades, siguiendo el
  mismo patrón modular 1.**

- **Facilita la auditoría y análisis de embarques consolidados,
  contenedores y su distribución dentro del sistema 1.**

**Resumen:**

**Este plugin consolida automáticamente toda la información logística de
un archivo BL, creando y relacionando todos los registros necesarios
para una gestión eficiente y robusta de los embarques y contenedores en
Dynamics CRM 1.**

**Espero que esto te sea de ayuda. Si necesitas más asistencia, no dudes
en decírmelo.**
