# Documentación del Plugin DC_LCC_Facturas

## Sistema: Dynacorp

---

## Descripción General

El plugin `DC_LCC_Facturas` automatiza la gestión del crédito de clientes en función del estado y valor de sus facturas dentro del sistema Dynacorp (Dynamics 365). Su objetivo es recalcular los totales, controlar límites de crédito, suspender o liberar crédito y aplicar retenciones financieras automáticamente según políticas, basándose en la evolución de las facturas y la situación crediticia del cliente.

---

## Propósito

- Automatizar el monitoreo y la gestión del crédito de los clientes al generar o actualizar facturas.
- Garantizar que los límites de crédito y las condiciones de pago se apliquen y controlen en tiempo real.
- Suspender o liberar el crédito de un cliente de forma automática según el estado de sus facturas.
- Mejorar la integridad y agilidad de los procesos comerciales y financieros.

---

## Funcionamiento Detallado

### 1. Control de Ejecución

- El plugin se ejecuta solo en el primer nivel de profundidad del pipeline para evitar reentradas innecesarias.
- Detecta la operación (`Create` o `Update`) sobre una factura.

### 2. Recalcular Totales de la Factura

- Al crear o actualizar una factura, el plugin fuerza el recálculo de los totales de la factura correspondiente.

### 3. Recuperación de Datos Clave

- Obtiene de la factura: estado actual y cliente asociado.
- Recupera la información clave del cliente: condición de pago, suspensión y retención de crédito, límite y crédito restante.

### 4. Validación y Control de Crédito

- Si el cliente **no está exento de retención de operaciones**:
  - Recupera todas las facturas activas (estado 32) del cliente.
  - Si el total de facturas supera el límite de crédito, actualiza el crédito restante y habilita la suspensión de crédito.
  - Si no se supera el límite, solo actualiza el crédito restante.

### 5. Control por Estado de Factura

- Si la factura es actualizada:
  - **Al cambiar a Estado 32 (activa):**
    - Verifica si alguna factura sobrepasa el plazo según la condición de pago.
    - Si es así, suspende el crédito y lo retiene por finanzas.
    - Si el total supera el límite de crédito, suspende el crédito.
  - **Al cambiar a Estado 33 (pagada o cerrada):**
    - Resta el total de la factura al crédito restante del cliente.
    - Si el resultado está por debajo del límite de crédito, libera la suspensión y la retención financiera.

### 6. Cálculo de Condiciones de Pago

- Traduce la condición de pago (opción seleccionada) en el número de días permitido para el pago (por ejemplo: 15, 30, 45, 60, 90 días).

### 7. Verificación de Facturas Vencidas

- Calcula el número de días desde la factura más antigua hasta la fecha actual.
- Si supera el plazo permitido por la condición de pago, suspende el crédito y lo retiene.

### 8. Manejo de Errores

- Todos los errores producidos durante el proceso se almacenan internamente, permitiendo la continuidad del proceso y facilitando el diagnóstico posterior.

---

## Beneficios

- **Automatización integral:** Elimina la gestión manual del crédito, garantizando el cumplimiento automático de las políticas financieras.
- **Prevención de riesgo:** Suspende automáticamente a clientes con facturas vencidas o con exceso de crédito.
- **Agilidad administrativa:** Libera el crédito de clientes en cuanto sus pagos son registrados.
- **Transparencia:** Asegura que cada cambio en facturas impacte de inmediato la situación crediticia del cliente.

---

## Casos de Uso

- **Control de riesgo financiero:** Suspensión automática de crédito a clientes con facturas vencidas o que superan su límite de crédito.
- **Liberación de crédito:** Reactivación del crédito y levantamiento de retenciones al registrar el pago de facturas.
- **Gestión dinámica del crédito disponible:** Cálculo y actualización en tiempo real del crédito restante para cada cliente tras cada operación.

---

## Consideraciones

- El plugin depende de la correcta parametrización de clientes, facturas y condiciones de pago en Dynacorp.
- Si falta alguna información clave (por ejemplo, condición de pago), el proceso muestra un mensaje de error y detiene la facturación.
- El proceso únicamente gestiona clientes que no estén marcados como exentos de restricciones de crédito.
- El correcto funcionamiento requiere que los estados de factura y cliente estén alineados con la lógica del negocio.

---

## Resumen

El plugin `DC_LCC_Facturas` es esencial para la gestión financiera y el control de riesgo en Dynacorp, automatizando la aplicación de límites de crédito y las acciones de suspensión o liberación de crédito de clientes, en función de la dinámica real de facturación y pagos.