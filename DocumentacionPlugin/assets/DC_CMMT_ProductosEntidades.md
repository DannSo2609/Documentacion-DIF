** Documentación del Plugin: Cálculo Automático de Montos y Tasas en
Productos de Embarque**

**Nombre del Plugin**

**DC_CMMT_ProductosEntidades.Proceso**

**Acción**

- **Update (Post-Operation)**

**Entidad Principal**

- **dc_productosdeembarque (puede ser adaptable a otros productos según
  contexto)**

**Descripción General**

**Este plugin se ejecuta automáticamente al actualizar un producto de
embarque. Su objetivo es calcular, validar y almacenar los montos
equivalentes en dólares, pesos y euros, así como las tasas de cambio
utilizadas para conversión, tanto para el costo como para la venta.
Garantiza que toda la información monetaria esté unificada y convertida
correctamente, evitando errores por monedas mal ingresadas o tasas
desactualizadas. El proceso cubre validaciones de campos obligatorios,
consulta y aplicación de tasas de cambio vigentes, cálculo de montos en
distintas monedas y actualización automática del registro.**

**Lógica Interna Principal**

1.  **Validación de Profundidad y Contexto**

    - **El plugin solo se ejecuta en la primera profundidad para evitar
      loops infinitos.**

2.  **Recuperación y Validación de Datos Clave**

    - **Recupera el producto objetivo junto a sus campos esenciales (ID,
      producto, totales, monedas, fecha de creación).**

    - **Lanza excepción si falta la moneda de compra o de venta.**

3.  **Obtención de Tasas y Monedas**

    - **Recupera la tasa del sistema vigente, lanza excepción si no
      existe.**

    - **Define los GUIDs de monedas estándar (peso, euro, dólar
      americano).**

4.  **Lógica de Conversión**

    - **Determina si el monto de venta es en pesos o euros y lo asigna a
      los campos correspondientes.**

    - **Consulta el tipo de cambio correcto para cada moneda (compra y
      venta) a dólares, usando la fecha de creación del producto.**

    - **Realiza la conversión a dólares redondeando a dos decimales.**

5.  **Actualización del Producto**

    - **Prepara y actualiza el registro con los montos convertidos y las
      tasas aplicadas.**

    - **Si fue disparado manualmente, marca que ya se calcularon los
      montos en dólares.**

6.  **Manejo de Errores**

    - **Cualquier error es trazado y relanzado para facilitar la
      depuración y evitar datos incompletos.**

**Validaciones Incluidas**

- **Verifica presencia de moneda de compra y venta. Si falta alguna,
  lanza excepción informativa al usuario.**

- **Valida existencia de una tasa del sistema activa antes de intentar
  cálculos.**

- **Lanza excepción si no existe un tipo de cambio válido para la fecha
  y monedas especificadas.**

**Entidades Involucradas**

- **dc_productosdeembarque (producto objetivo)**

- **dc_tasa (tasa del sistema)**

- **dc_tipodecambio (tipos de cambio históricos)**

- **transactioncurrency (monedas estándar)**

**Métodos Auxiliares Importantes**

- **ObtenerTasa: Recupera la primera tasa del sistema disponible.**

- **ObtenerTipoDeCambio: Busca el tipo de cambio más reciente antes de
  la fecha de creación para un par de monedas específico.**

- **ObtenerMontoEnDolares: Realiza la conversión y redondeo de montos a
  dólares.**

**Manejo de Errores**

- **Utiliza InvalidPluginExecutionException para dar mensajes claros si
  falta alguna moneda, tasa o tipo de cambio.**

- **Registra todos los errores en el servicio de trazas (tracingService)
  para facilitar soporte y debugging.**

- **Lanza el error sin modificar para asegurar visibilidad en CRM.**

**Consideraciones Finales**

- **El plugin asegura integridad y consistencia monetaria en productos
  de embarque.**

- **Permite ampliaciones para otras monedas agregando los GUIDs y lógica
  correspondiente.**

- **Facilita la trazabilidad y auditoría de conversiones de moneda
  dentro de Dynamics CRM.**

- **La lógica y estructura modular permiten fácil mantenimiento y
  futuras adaptaciones.**

**Resumen:**

**Este plugin automatiza el proceso de cálculo y conversión de montos en
productos de embarque, garantizando que toda la información monetaria
esté correctamente calculada y almacenada, lo que simplifica los
procesos comerciales y contables, evitando errores y asegurando
coherencia en los datos.**
