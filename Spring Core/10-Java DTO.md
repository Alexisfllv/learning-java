## Entity

```java
@Entity  
@Data  
@AllArgsConstructor  
@NoArgsConstructor  
@Table(name = "category")  
@EqualsAndHashCode(onlyExplicitlyIncluded = true)  
public class Category {  
  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    @EqualsAndHashCode.Include  
    private Long idCategory;  
  
    @Column(name = "category_name",nullable = false, length = 50)  
    private String categoryName;  
  
    @Column(name = "category_description",nullable = false,length = 250)  
    private String categoryDescription;  
  
    @Column(name = "category_enabled",nullable = false)  
    private boolean categoryEnabled;  
  
  
}
```

## DTO - Clase

```java
@Data  
@AllArgsConstructor  
@NoArgsConstructor  
public class Category_DTO {  
    private Long idCategory;  
    private String categoryName;  
    private String categoryDescription;  
    private boolean categoryEnabled;  
}
```

## DTO - Record

```java
public record CategoryDTO(  
   Long idCategory,  
   String categoryName,  
   String categoryDescription,  
   boolean categoryEnabled  
) {}
```

