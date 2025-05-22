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