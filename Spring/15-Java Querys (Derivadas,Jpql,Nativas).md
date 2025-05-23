### Ejemplos en un solo campo

```java
@Repository  
public interface CategoryRepo extends GenericRepo<Category,Long> {  
  
    // DerivedQueries Querys derivadas  
    // Select * from category c where c.name = '';    List<Category> findByCategoryName(String name );  
  
    // select * from category c where c.name like '%texto%'  
    // texto => '%para%'    List<Category> findByCategoryDescriptionLike(String texto);  
  
    // JPQL  
  
    @Query("from Category c where c.categoryName = :name and c.categoryDescription like %:desc%")  
    List<Category> getNameAndDescription(@Param("name") String name, @Param("desc") String desc);  
}
```

### A mas de una tabla JPQL

``` java
@Repository  
public interface ProductRepo extends GenericRepo<Product,Long> {  
  
    // jpql  
    @Query("from Product p where p.category.categoryName = :name")  
    List<Product> getProductsByCategory(String name);  
  
    // native query  
    @Query(nativeQuery = true, value = "SELECT * FROM product")  
    List<Product> listadodeProductos();  
  
    @Query(value = "SELECT * FROM product WHERE product_description LIKE :desc", nativeQuery = true)  
    List<Product> buscarPorDescripcion(@Param("desc") String desc);  
  
}
```

### Metodos Query para procedure y views

```java
@Repository  
public interface SaleRepo extends GenericRepo<Sale,Long> {  
  
    // LLAMADA DE VIEW select * from sale;  
    @Query(value = "select * from fn_sales", nativeQuery = true)  
    List<Object[]> listadoVentas();  
  
    @Query(value = "select * from fn_sales", nativeQuery = true)  
    List<Venta> listadoVentas2();  
  
    // procedure para convertir todo a true en sale  
    @Procedure(procedureName = "all_true")  
    void convertirTodasLasVentas();  
  
}
```

## Service Impl

``` java
@RequiredArgsConstructor  
@Service  
public class SaleServiceImpl extends CRUDIMPL<Sale,Long> implements SaleService {  
  
    private final SaleRepo saleRepo;  
  
  
    @Override  
    protected GenericRepo<Sale, Long> getRepo() {  
        return saleRepo;  
    }  
  
  
    @Override  
    public List<Venta_DTO> Ventas() {  
        return saleRepo.listadoVentas().stream()  
                .map(e -> new Venta_DTO(  
                        Integer.parseInt(String.valueOf(e[0])),  
                        LocalDate.parse(String.valueOf(e[1]), DateTimeFormatter.ofPattern("dd/MM/yyyy"))  
                ))  
                .collect(Collectors.toList());  
    }  
  
    @Override  
    public List<Venta> Ventas2() {  
        return saleRepo.listadoVentas2();  
    }  
  
    @Transactional  
    @Override    public void convertirSale() {  
        saleRepo.convertirTodasLasVentas();  
    }  
  
}
```