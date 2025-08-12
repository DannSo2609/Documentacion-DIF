# DC_SR_Embarques

## Descripción General

**DC_SR_Embarques** es un plugin desarrollado para el ecosistema Dynacorp, orientado a la gestión y actualización automática de los consolidados de embarque en procesos logísticos. Su objetivo principal es asegurar la integridad y actualización de los totales de peso y volumen de los consolidado (MAWB/MBL) cada vez que se modifica un embarque hijo, sumando correctamente todos los valores relevantes de las entidades relacionadas.

---

## Objetivo del Plugin

- Calcular y actualizar automáticamente los totales de peso y volumen (en toneladas, kilogramos, libras, metros cúbicos, pies cúbicos, centímetros cúbicos y cantidad de piezas) en los registros de consolidado de embarque.
- Garantizar que los valores del consolidado reflejen siempre la suma de todos los embarques hijos directos, excluyendo los nietos (subniveles adicionales), para mantener la trazabilidad logística y evitar errores de acumulación.

---

## Funcionamiento

1. **Detección de embarque consolidado:**  
   El plugin se activa cuando se realiza una operación sobre un embarque y detecta si este embarque está vinculado a un consolidado (campo `dc_mawbmbl`). Solo actúa si existe esta relación.

2. **Recuperación de embarques hijos:**  
   Busca todos los embarques cuyo campo `dc_mawbmbl` apunta al consolidado actual, obteniendo los valores de peso, volumen y cantidad de piezas de cada uno.

3. **Filtrado de embarques nietos:**  
   Solo considera para el cálculo los embarques que no sean nietos (es decir, que no estén a su vez relacionados como hijos de otro embarque).

4. **Suma de totales:**  
   Suma los valores de todos los embarques hijos directos en los campos configurados (toneladas, kilogramos, libras, metros cúbicos, pies cúbicos, centímetros cúbicos, cantidad de piezas).

5. **Actualización automática del consolidado:**  
   Los valores acumulados se actualizan en el registro del consolidado padre, garantizando que los totales reflejen la realidad de la operación logística en tiempo real.

6. **Gestión de errores:**  
   Cualquier error detectado en la recuperación o procesamiento de los datos se almacena internamente, permitiendo rastrear problemas sin interrumpir el flujo principal del proceso.

---

## Ventajas y Beneficios

- **Automatización:** Elimina la necesidad de realizar actualizaciones manuales de los totales en los consolidados, minimizando errores humanos.
- **Integridad de datos:** Solo suma los embarques hijos directos, evitando sobre-acumulaciones con registros de niveles inferiores (nietos).
- **Trazabilidad:** Asegura que el usuario y los sistemas relacionados tengan siempre acceso a los datos de volumen y peso más actualizados para la toma de decisiones y la generación de reportes.
- **Robustez:** Maneja errores individuales sin detener el proceso global, permitiendo que la mayoría de los datos se procesen incluso si algún embarque presenta problemas.
- **Escalabilidad:** Preparado para escenarios con múltiples hijos y grandes volúmenes de datos.

---

## Consideraciones de Uso

- El plugin se ejecuta automáticamente al operar sobre la entidad de embarques (`dc_embarques`) y únicamente si el registro tiene asignado un consolidado (`dc_mawbmbl`).
- Los campos actualizados en el consolidado son: toneladas, kilogramos, libras, metros cúbicos, pies cúbicos, centímetros cúbicos y cantidad de piezas.
- No incluye embarques que sean nietos para evitar errores de doble conteo.
- Los errores internos se almacenan y pueden ser utilizados para diagnosticar problemas posteriores.

---

## Posibles Extensiones

- Notificación al usuario: El plugin tiene un bloque preparado (actualmente comentado) para notificar visualmente al usuario sobre la actualización exitosa del consolidado, incluyendo un enlace directo al registro.
- Ampliación de campos o lógica: Puede adaptarse fácilmente para sumar otros campos o aplicar reglas adicionales según nuevas necesidades del negocio.

---

## Recomendaciones de Mantenimiento

- Revisar los nombres lógicos de los campos y relaciones en caso de cambios en el modelo de datos.
- Monitorear los errores internos almacenados para detectar patrones de fallo o inconsistencias en los registros.
- Actualizar la lógica del plugin si se agregan nuevos niveles jerárquicos o relaciones adicionales en los embarques y consolidados.
