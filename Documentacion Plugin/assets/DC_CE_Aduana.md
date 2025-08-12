# Documentación del Plugin: Proceso de Actualización de Estado de Aduana

## Nombre del Plugin

DC_CE_Aduana.Proceso

## Acción

Update (Post-Operation)

## Entidad Principal

dc_aduanas

## Descripción General

Este plugin se ejecuta sobre la entidad dc_aduanas y su propósito es
actualizar el estado del registro de una aduana en función de múltiples
condiciones relacionadas con su propietario, ejecutivo, declaraciones,
despachos y facturación. Se activa también desde entidades relacionadas
como dc_despachotransporte y dc_declaraciones cuando contienen una
referencia a una aduana.

## Lógica Interna

1\. Verifica la profundidad de ejecución y la existencia del Target.

2\. Determina la entidad origen (dc_aduanas, dc_despachotransporte o
dc_declaraciones).

3\. Recupera la entidad dc_aduanas y sus campos clave.

4\. Verifica si el estado actual está en la lista de estados sin acción
(por ejemplo, Cancelado).

5\. Si no tiene propietario, actualiza el estado a Activo.

6\. Marca la aduana como asignada y cambia el estado a En Proceso.

7\. Si tiene ejecutivo, cambia el estado a En Declaración.

8\. Verifica si ya fue declarada (por número o todas las declaraciones).

9\. Si fue declarada, cambia el estado a Declarado.

10\. Si se deben enviar valores, cambia el estado a Valores Enviados.

11\. Si tiene fecha de pago, cambia el estado a Valores Pagados.

12\. Si existen despachos, cambia el estado a Despacho Solicitado.

13\. Si todos los despachos tienen fecha de salida, cambia el estado a
Pendiente de Factura.

14\. Si está marcado como facturado, cambia el estado a Facturado.

## Validaciones Incluidas

\- Verifica profundidad de ejecución para evitar recursividad.

\- Verifica existencia de campos obligatorios como ownerid, ejecutivo,
número de declaración, etc.

\- Verifica que todas las declaraciones estén en estado 26 (Declarado).

\- Verifica que todos los despachos tengan fecha de salida.

## Entidades Involucradas

\- dc_aduanas (entidad principal)

\- dc_declaraciones (relacionadas por campo dc_aduanas)

\- dc_despachotransporte (relacionadas por campo dc_aduanas)

## Métodos Auxiliares

\- GetAduanaColumns: Devuelve los campos necesarios para procesar la
aduana.

\- ObtenerDeclaraciones: Recupera las declaraciones relacionadas a la
aduana.

\- TodasDeclaradas: Verifica si todas las declaraciones están en estado
26.

\- ObtenerDespachos: Recupera los despachos relacionados a la aduana.

\- TodosDespachosConFecha: Verifica si todos los despachos tienen fecha
de salida.

\- UpdateStatus: Actualiza el campo dc_estadodelregistro con un nuevo
estado.

\- UpdateEntity: Actualiza un campo específico de una entidad.

## Manejo de Errores

El plugin captura excepciones durante la ejecución y las registra
mediante el servicio de trazabilidad. En caso de error, lanza una
InvalidPluginExecutionException con un mensaje descriptivo para
facilitar el diagnóstico.
