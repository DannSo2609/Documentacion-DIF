**Documentación del Plugin: Facturación Automática de Embarques**

**Nombre del Plugin**

**Facturación Automática de Embarques**

**Acción**

Este plugin se ejecuta automáticamente cuando se actualiza un registro
de embarque (**dc_embarques**).

**Entidad Principal**

**dc_embarques** (registro principal)

**Descripción General**

El objetivo del plugin es automatizar la generación de facturas
asociadas a cada embarque. Garantiza que solo se facture cuando se
cumplen todas las condiciones de negocio y que las facturas incluyan los
productos correctos (locales, internacionales, a colectar o reembolso),
además de registrar la comisión del vendedor correspondiente.

**Lógica Interna Principal**

1.  **Validación de Recursividad y Existencia del Registro**

    - El plugin evita ejecutarse recursivamente y solo opera si hay un
      registro objetivo válido.

2.  **Recuperación y Validación de Datos Clave**

    - Recupera el embarque y valida que tenga cliente, expediente y
      vendedor definidos; de lo contrario, detiene la ejecución y
      muestra un mensaje claro.

3.  **Condiciones de Negocio para la Facturación**

    - Analiza el tipo de operación y fechas clave (ATA, ATD) para
      decidir si el embarque puede ser facturado.

**Tipos de Facturación Soportados**

- **Locales**

- **Internacionales**

- **A Colectar**

- **Reembolso**

**Validaciones Incluidas**

- Verificación de datos del cliente

- Confirmación de fechas clave

- Validación de productos a facturar

**Entidades Involucradas**

- **dc_facturas** (factura generada o actualizada)

- **dc_productosdeembarque**

- **dc_productoscolectar**

- **dc_productosreembolso**

**Métodos Auxiliares Importantes**

- Métodos para la recuperación de datos

- Métodos para la validación de condiciones de negocio

**Manejo de Errores**

El plugin incluye manejo de errores para asegurar que cualquier problema
durante la ejecución sea registrado y notificado adecuadamente.

**Consideraciones Finales**

Es importante revisar y probar el plugin en un entorno de desarrollo
antes de implementarlo en producción para asegurar su correcto
funcionamiento.

**Resumen**

Este plugin automatiza la facturación de embarques, asegurando que se
cumplan todas las condiciones de negocio y que las facturas sean
precisas y completas.

------------------------------------------------------------------------

Espero que estas mejoras sean de tu agrado. ¿Hay algo más en lo que
pueda asistirte?
