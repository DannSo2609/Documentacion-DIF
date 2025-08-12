**Documentación del Plugin: Validación de Número de Contenedor**

**🧩 Nombre del Plugin**

**DC_LCS_LimitarContenedor.Proceso**

**⚙️ Evento que lo Dispara**

**Create o Update (Post-Operation)**

**Aplica cuando se crea o actualiza un registro de contenedor.**

**🗂️ Entidad Principal**

- **dc_contenedores**

**🎯 Objetivo**

**Evitar que se guarde un número de contenedor (dc_numerocontenedor) que
contenga:**

- **Espacios**

- **Caracteres especiales como: @ \# \$ % \^ & \***

- **Cualquier carácter no alfanumérico**

**🧾 Campo Validado**

  -------------------------------------------------------------------------------------
  **Campo**                 **Tipo**     **Descripción**
  ------------------------- ------------ ----------------------------------------------
  **dc_numerocontenedor**   **String**   **Número identificador del contenedor
                                         ingresado por el usuario**

  -------------------------------------------------------------------------------------

**🔁 Lógica de Validación**

1.  **Recuperación del campo:**

    - **El plugin recupera el campo dc_numerocontenedor del registro
      actual.**

2.  **Validación:**

    - **Si el número:**

      - **Contiene espacios**

      - **O no es alfanumérico puro (a-z, A-Z, 0-9)**

    - **Entonces lanza la siguiente excepción:**

**perl**

**CopyEdit**

**El número de contenedor no debe contener espacios, ni caracteres
especiales como @,#,\$,%,\^,&,\***

**Por favor, ingrese un valor sin espacios y/o sin caracteres
especiales.**

**✅ Resultado Esperado**

**Impide guardar un número de contenedor inválido con formato
incorrecto.**

**🧪 Casos de Prueba Recomendados**

  -----------------------------------------------------------------------
  **Valor del Número de Contenedor**  **Resultado Esperado**
  ----------------------------------- -----------------------------------
  **CONT123456**                      **✅ Aceptado**

  **CONT 123456**                     **❌ Rechazado (contiene espacio)**

  **CONT@123456**                     **❌ Rechazado (contiene @)**

  **123456**                          **✅ Aceptado**

  **CONT-123456**                     **❌ Rechazado (contiene -)**

  **CONT_123456**                     **❌ Rechazado (contiene \_)**
  -----------------------------------------------------------------------

**🧹 Recomendaciones de Mejora**

- **🔒 Elimina el método Regex.IsMatch(\...) duplicado en lógica
  condicional si deseas optimizar la legibilidad.**

- **📛 En el mensaje de excepción puedes usar una lista más clara de
  caracteres no permitidos (\[\^a-zA-Z0-9\]) para evitar
  interpretaciones incorrectas.**

- **🧪 Asegúrate que la validación también se aplique desde Power
  Apps/Forms si se requiere experiencia de usuario inmediata.**
