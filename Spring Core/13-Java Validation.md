## Dependecia para validaciones
```xml
<!-- Validacion S.B 2.3 ++ -->  
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-validation</artifactId>  
</dependency>
```

## caso de ejemplo caso USER

```java 
package com.example.dto;

import jakarta.validation.constraints.*;

@jakarta.annotation.Generated("Validated DTO example")
public record UserRequestDTO(

    // No debe estar vacío ni en blanco
    @NotBlank(message = "El nombre es obligatorio")
    @Size(max = 50, message = "El nombre no debe superar los 50 caracteres")
    @Pattern(regexp = "^[a-zA-ZáéíóúÁÉÍÓÚñÑ ]+$", message = "El nombre solo puede contener letras y espacios")
    String nombre,

    @NotBlank(message = "El apellido es obligatorio")
    @Size(max = 50, message = "El apellido no debe superar los 50 caracteres")
    @Pattern(regexp = "^[a-zA-ZáéíóúÁÉÍÓÚñÑ ]+$", message = "El apellido solo puede contener letras y espacios")
    String apellido,

    // Validación estándar de correo electrónico
    @NotBlank(message = "El correo es obligatorio")
    @Email(message = "Debe ingresar un correo válido")
    String email,

    // Debe contener solo números, y tener exactamente 8 dígitos
    @NotBlank(message = "El DNI es obligatorio")
    @Pattern(regexp = "^\\d{8}$", message = "El DNI debe tener exactamente 8 dígitos")
    String dni,

    // Número celular peruano (9 dígitos, empieza en 9)
    @NotBlank(message = "El número celular es obligatorio")
    @Pattern(regexp = "^9\\d{8}$", message = "El número celular debe empezar en 9 y tener 9 dígitos")
    String celular,

    // Edad entre 18 y 99 años
    @NotNull(message = "La edad es obligatoria")
    @Min(value = 18, message = "Debe ser mayor de edad")
    @Max(value = 99, message = "La edad máxima permitida es 99 años")
    Integer edad,

    // Contraseña segura: al menos una mayúscula, una minúscula, un número y 8 caracteres mínimo
    @NotBlank(message = "La contraseña es obligatoria")
    @Pattern(
        regexp = "^(?=.*[a-z])(?=.*[A-Z])(?=.*\\d).{8,}$",
        message = "La contraseña debe tener al menos 8 caracteres, una mayúscula, una minúscula y un número"
    )
    String password,

    // Campo opcional con límite de caracteres
    @Size(max = 255, message = "El campo biografía no debe superar los 255 caracteres")
    String biografia

) {}

```