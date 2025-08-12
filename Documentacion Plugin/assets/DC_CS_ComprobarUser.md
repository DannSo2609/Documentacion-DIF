**Documentación del Plugin: Comprobación de Usuario para Integración
Dynamics**

**Nombre del Plugin**

**DC_CS_ComprobarUser.Proceso**

**Acción**

- **Update (Post-Operation) (Se ejecuta al actualizar un registro)**

**Entidad Principal**

- **dc_ordenesdecompra**

**Descripción General**

**Este plugin verifica si una orden de compra debe ser integrada con
Dynamics según el campo booleano dc_integraradynamics. Si no debe
integrarse (false), valida que el usuario que modificó el registro sea
uno de los autorizados para permitir la acción. En caso contrario,
bloquea la operación lanzando una excepción.**

**Lógica Interna Principal**

1.  **Prevención de Recursividad**

    - **Ejecuta solo si la profundidad de contexto es 1 para evitar
      loops infinitos.**

2.  **Validación del Campo Integración**

    - **Solo actúa si el campo dc_integraradynamics es falso.**

3.  **Obtención del Usuario Modificador**

    - **Recupera el registro de orden de compra con el campo modifiedby
      para identificar quién hizo la última modificación.**

4.  **Validación del Usuario**

    - **Compara el GUID del usuario modificador con un listado de GUIDs
      autorizados (Carlos, Ivette, Jharison).**

5.  **Manejo de Acceso No Autorizado**

    - **Si el usuario no está autorizado, lanza una excepción para
      restringir la acción.**

**Validaciones Incluidas**

- **Prevención de recursividad.**

- **Verificación del campo dc_integraradynamics.**

- **Validación estricta del usuario que modifica el registro.**

**Entidades Involucradas**

- **dc_ordenesdecompra (entidad principal donde se realiza la
  validación)**

**Manejo de Errores**

- **Registra errores en el servicio de trazas (ITracingService).**

- **Lanza excepciones para asegurar bloqueo visible en el sistema.**

**Consideraciones Finales**

- **Plugin que refuerza la seguridad y control sobre quién puede
  modificar registros que no deben integrarse automáticamente.**

- **Fácil de actualizar para agregar o quitar usuarios autorizados.**

**✅ Resumen**

**El plugin DC_CS_ComprobarUser.Proceso controla que sólo usuarios
autorizados puedan modificar órdenes de compra que no se integran con
Dynamics, bloqueando acciones de usuarios no permitidos y asegurando el
control sobre la integración.**
