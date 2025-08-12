# 📄 Documentación del Plugin: Cálculo Automático de Profit en Oportunidad

## Nombre del Plugin

DC_CPT_Oportunidad.Proceso

## Acción

Update (Post-Operation)

## Entidad Principal

dc_oportunidad

## Descripción General

Este plugin se ejecuta automáticamente al actualizar una oportunidad
(\`dc_oportunidad\`). Su objetivo es calcular y actualizar
automáticamente el profit (ganancia) de la oportunidad, sumando los
valores de venta y costo de todos los productos asociados de cada área
(Embarque, Aduanas, Transporte y Ventas), cuando el campo booleano
\`dc_calcularprofit\` está marcado en la oportunidad.\
\
La lógica centraliza y automatiza el cálculo del profit de la
oportunidad por cada área, evitando errores manuales y asegurando
integridad y coherencia en los datos del registro de oportunidad.

## Lógica Interna Principal

**1. Prevención de Recursividad\**
 - El plugin ejecuta su lógica solo si la profundidad de contexto es 1
(evita loops infinitos).\
**2. Validación de Condición\**
 - Solo procede si el campo booleano \`dc_calcularprofit\` está marcado
como verdadero en la oportunidad.\
**3. Cálculo de Profit por Área\**
 - Para cada área de productos (\`dc_productosdeembarque\`,
\`dc_productosaduana\`, \`dc_productostransporte\`,
\`dc_productosdeventas\`):\
- Busca todos los productos asociados a la cotización relacionada con la
oportunidad.\
- Suma los montos de costo (\`dc_montodolarescosto\`) y de venta
(\`dc_montodolaresventa\`).\
- Calcula el profit como la diferencia entre la suma de venta y la suma
de costo.\
- Redondea el resultado a dos decimales.\
**4. Actualización de la Oportunidad\**
 - Actualiza los campos de profit por cada área:\
- \`dc_profitdeembarque\`\
- \`dc_profitaduanas\`\
- \`dc_profittransporte\`\
- \`dc_profitdeventas\`\
- Marca el campo \`dc_calcularprofit\` como falso al finalizar el
cálculo.

## Validaciones Incluidas

\- Valida existencia de productos asociados antes de calcular.\
- Realiza sumas diferenciadas por área, asegurando acumulación correcta
de valores.\
- Usa cero como valor por defecto si un producto carece de algún campo.

## Entidades Involucradas

\- dc_oportunidad (oportunidad principal)\
- dc_productosdeembarque (productos de embarque)\
- dc_productosaduana (productos de aduana)\
- dc_productostransporte (productos de transporte)\
- dc_productosdeventas (productos de ventas)

## Métodos y Cálculos Importantes

\- ObtenerProfit:\
Suma los montos de venta y costo de los productos asociados a la
cotización y calcula la diferencia (profit).

## Manejo de Errores

\- Registra todos los errores en el servicio de trazas
(\`tracingService\`) para facilitar soporte y debugging.\
- Lanza el error sin modificar para asegurar visibilidad en CRM.

## Consideraciones Finales

\- El plugin automatiza el cálculo y actualización del profit en
oportunidades, eliminando pasos manuales y asegurando datos
consistentes.\
- Puede ser extendido fácilmente para nuevas áreas o reglas de negocio
adicionales.\
- Facilita la trazabilidad y control en el proceso comercial y
financiero.

## Resumen

Este plugin suma automáticamente los costos y ventas de todos los
productos relacionados a una oportunidad y actualiza los profits por
área, asegurando integridad y trazabilidad en la gestión financiera y
comercial.
