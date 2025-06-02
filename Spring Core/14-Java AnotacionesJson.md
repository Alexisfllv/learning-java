## @Json
### Anotaciones para los campos 

```java
@AllArgsConstructor  
@NoArgsConstructor  
@Data  
public class User_DTO {  
  
    private Long idUser;  
  
    @NotBlank(message = "El nombre de usuario es obligatorio")  
    @Size(max = 50, message = "El nombre de usuario no debe exceder los 50 caracteres")  
    // cambiar nombre de campo
    @JsonProperty(value = "user_name")  
    private String userName;  
  
    @NotBlank(message = "La contraseña es obligatoria")  
    @Size(max = 60, message = "La contraseña no debe exceder los 60 caracteres")  
    // ACCESO SOLO A LECTURA  
    @JsonProperty(access = JsonProperty.Access.WRITE_ONLY)  
    private String password;  
  
    @NotNull(message = "El estado 'enabled' no puede ser nulo")  
    private Boolean enabled;  
  
  
    // mostrar solo campos de role dto o insertar datos  
    @JsonIncludeProperties(value = {"idRole", "roleName"} )  
    @NotNull(message = "El rol es obligatorio")  
    private Role_DTO role;  
}
```
### Anotaciones para clases 

```java
@Data  
@AllArgsConstructor  
@NoArgsConstructor  
@JsonInclude(JsonInclude.Include.NON_NULL)  
public class Sale_DTO {  
  
    private Long idSale;  
  
    @NotNull  
    private LocalDateTime  saleDateTime;  
  
    @NotNull  
    private Double saleTotal;  
  
    @NotNull  
    private Double saleTax;  
  
    @NotNull  
    private boolean saleEnabled;  
  
    // fks  
  
    @NotNull  
    private Client_DTO client;  
  
    @NotNull  
    @JsonIncludeProperties({"idUser"})  
    private User_DTO user;  
  
    // list  
    @JsonManagedReference  
    private List<SaleDetail_DTO> details;  
}
```