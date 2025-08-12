**Documentación del Plugin: Duplicar Oportunidad**

**🔖 Nombre del Plugin**

**DC_Duplicar_Oportunidad.Proceso**

**🔄 Acción**

- **Update (Post-Operation)**

**🔑 Entidad Principal**

- **dc_oportunidad**

**🔹 Descripción General**

**Este plugin se ejecuta cuando se actualiza una oportunidad
(dc_oportunidad) y está marcado el campo booleano
dc_duplicaroportunidad. Su función es duplicar completamente una
oportunidad, incluyendo todos sus datos, configuraciones y entidades
hijas relacionadas:**

- **Volúmenes marítimos**

- **Pesos volumétricos**

- **Productos de ventas**

- **Productos de embarque**

- **Productos de aduana**

- **Productos de transporte**

**Todo esto se asocia a una nueva oportunidad creada a partir del
original.**

**📊 Proceso General del Plugin**

1.  **Validación Inicial**

    - **Verifica que el plugin no se ejecute en profundidad mayor a 1
      (para evitar loops).**

    - **Revisa si el campo dc_duplicaroportunidad está marcado como
      verdadero.**

2.  **Recuperación Completa de la Oportunidad Original**

    - **Se recupera el registro original usando RetrieveRequest con
      todos los campos (ColumnSet(true)).**

3.  **Creación de Nueva Oportunidad**

    - **Se clonan los valores principales de la oportunidad en una nueva
      entidad.**

    - **Incluye todas las secciones:**

      - **Información general**

      - **Origen y destino**

      - **Tipo de embarque, transporte y operación**

      - **Características de la carga**

      - **Gestores y suplidores**

      - **Profit**

      - **Notas, dimensiones, peso y volumen**

      - **Comentarios logísticos y datos adicionales**

4.  **Clonado de Entidades Relacionadas Se realiza una búsqueda y
    posterior creación de registros relacionados con la nueva
    oportunidad:**

  ---------------------------------------------------------------------------
  **Entidad         **Entidad CRM**              **Propósito**
  Relacionada**                                  
  ----------------- ---------------------------- ----------------------------
  **Pesos           **dc_pesovolumetrico**       **Clona dimensiones/pesos**
  Volumétricos**                                 

  **Volúmen         **dc_volumenmaritimo**       **Clona registros de
  Marítimo**                                     volumen**

  **Productos de    **dc_productosdeventas**     **Copia productos de venta**
  Ventas**                                       

  **Productos de    **dc_productosdeembarque**   **Copia productos para
  Embarque**                                     transporte marítimo**

  **Productos de    **dc_productosaduana**       **Copia productos para
  Aduana**                                       gestión aduanal**

  **Productos de    **dc_productostransporte**   **Copia productos de
  Transporte**                                   transporte terrestre**
  ---------------------------------------------------------------------------

5.  **Relación Nueva**

    - **Todos los registros relacionados se asocian al nuevo ID de
      oportunidad generado.**

6.  **Finalización**

    - **El proceso finaliza con todos los registros duplicados.**

**📊 Campos Clave Usados en la Duplicación**

- **dc_cliente, dc_correo, dc_telefono**

- **dc_tipodeembarque, dc_tipodetransporte, dc_tipodeoperacion**

- **dc_metrocubico, dc_piescubico, dc_centimetroscubicos**

- **dc_profitdeembarque, dc_profitaduanas, dc_profittransporte,
  dc_profitdeventas**

- **dc_oportunidad (relación para registros hijos)**

**⚠️ Validaciones**

- **Si la entidad original no contiene un campo, se le asigna un valor
  por defecto (0 o null).**

- **Se validan los OptionSet para peso y volumen antes de asignar
  valores específicos como toneladas, kilogramos, etc.**

- **Cada bloque de clonado realiza comprobación Contains para evitar
  errores de campos nulos.**

**📊 Beneficios del Plugin**

- **Acelera la creación de oportunidades similares**

- **Mejora la eficiencia operativa**

- **Evita errores manuales de copiado**

- **Mantiene coherencia entre datos comerciales**

**📅 Escenario de Uso Típico**

**Cuando un ejecutivo necesita replicar una oportunidad ya existente con
todos sus detalles y condiciones, este plugin permite hacerlo con un
solo clic al activar el campo dc_duplicaroportunidad.**

**🔐 Recomendaciones**

- **Asegurar que todos los campos requeridos existan en el formulario.**

- **Incluir lógica adicional si se agregan nuevas entidades
  relacionadas.**

- **Validar que los permisos de usuario permitan crear registros
  relacionados.**
