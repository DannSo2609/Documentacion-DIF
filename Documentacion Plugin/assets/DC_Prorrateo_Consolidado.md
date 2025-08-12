# Documentación funcional del plugin DC_Prorrateo_Consolidado

## Sistema: Dynacorp

## Descripción general

El plugin **DC_Prorrateo_Consolidado** para Dynacorp automatiza el prorrateo de cargos consolidados, individuales y de depósitos logísticos entre los embarques hijos de un consolidado. Su función principal es distribuir un monto total proporcionalmente según el peso (kilogramos o metros cúbicos) de cada embarque, creando los registros de cargo correspondientes para cada uno.

---

## Propósito

Permitir a los usuarios de Dynacorp calcular automáticamente la distribución de cargos sobre los embarques que componen un consolidado, garantizando un reparto proporcional, validando la existencia de datos críticos y asegurando la correcta actualización de los estados de procesamiento.

---

## Flujo general

1. **Condición de ejecución:**  
   El plugin se ejecuta únicamente si la profundidad del pipeline es 1 o menor (para evitar ciclos infinitos) y si al menos uno de los flags para cargos consolidados, individuales o depósitos logísticos está activado en el consolidado.

2. **Recuperación y validación del consolidado:**  
   Se recuperan todos los campos relevantes del consolidado.
   - Si falta algún dato obligatorio para el tipo de cargo a procesar (suplidor, lista de precios, producto, tipo de cargo, monto, depósito logístico), el proceso detiene ese bloque y lanza una excepción descriptiva.

3. **Obtención de embarques hijos:**  
   Recupera todos los embarques hijos asociados al consolidado y que no son a su vez "nietos" (embarques secundarios).

4. **Prorrateo y creación de cargos:**  
   Para cada tipo de cargo activado:
   - Se valida que existan todos los datos obligatorios para ese tipo.
   - Se suma el peso total de los embarques (según el tipo de cargo: kilogramos o metros cúbicos).
   - Para cada embarque, se calcula el monto proporcional del cargo en base a su peso relativo y se crea el registro de cargo correspondiente en la entidad adecuada.
   - Al finalizar, se desactiva el flag de procesamiento para evitar reprocesos accidentales.

5. **Gestión de errores:**  
   - Los errores críticos (como la ausencia de datos obligatorios) detienen el bloque correspondiente y se registran.
   - Los errores no críticos (como problemas al crear un cargo) se capturan internamente y permiten continuar con el resto del proceso.

6. **Notificaciones (comentado):**  
   El plugin incluye bloques comentados para crear notificaciones visuales en Dynacorp al finalizar el proceso, aunque esta funcionalidad está deshabilitada por defecto.

---

## Consideraciones adicionales

- El plugin valida que el monto a prorratear sea mayor a cero antes de procesar.
- Valida que cada embarque tenga el peso necesario para el tipo de cargo seleccionado.
- Es versátil y permite procesar uno, dos o los tres tipos de cargos en una sola ejecución.
- Desactiva el flag correspondiente tras el prorrateo exitoso para evitar reprocesos.
- Utiliza el servicio de trazado de Dynacorp para registrar el estado y errores del proceso, facilitando el diagnóstico.

---

## Resumen

El plugin **DC_Prorrateo_Consolidado** automatiza el reparto proporcional de cargos entre los embarques asociados a un consolidado en Dynacorp, asegurando la validez de los datos, la correcta distribución de montos y la integridad operativa del proceso. Es una herramienta esencial para el control financiero y logístico en operaciones consolidadas, minimizando errores manuales y optimizando la gestión administrativa.