### CategoryServiceTest

```java
import org.mockito.InjectMocks;  
import org.mockito.Mock;  
import org.springframework.test.context.junit.jupiter.SpringExtension;  
  
import java.util.Arrays;  
import java.util.List;  
import java.util.Optional;  
  
import static org.junit.jupiter.api.Assertions.*;  
import static org.mockito.Mockito.*;  
  
  
@Slf4j  
@ExtendWith(SpringExtension.class)  
public class CategoryServiceTest {  
  
    //  
    @Mock  
    private CategoryRepo categoryRepo;  
    @InjectMocks  
    private CategoryServiceImpl categoryService;  
  
  
    // ✅ Extraemos valores comunes a atributos reutilizables    private Category category1;  
    private Category category2;  
    private Category category3;  
  
    // valores preestablecidos  
    @BeforeEach  
    void setUp() {  
        // ✅ Inicialización común a todos los tests        category1 = new Category(1L, "Tecnología",  
                "Productos relacionados con computadoras, teléfonos y otros dispositivos electrónicos.", true);  
  
        category2 = new Category(2L, "Hogar",  
                "Artículos para el hogar como muebles, decoración y electrodomésticos.", true);  
  
        category3 = new Category(3L, "Ropa",  
                "Ropa y accesorios para hombres, mujeres y niños.", false);  
  
    }  
  
    // test de listar  
    @Test  
    void givenCategoriesExist_whenListAll_thenReturnsAllCategories() throws Exception {  
  
        List<Category> categories = Arrays.asList(category1, category2, category3);  
        when(categoryRepo.findAll()).thenReturn(categories);  
  
        List<Category> res = categoryService.listAll();  
        log.info("Lista de categorias: {}", res);  
  
        assertNotNull(res);  
        assertFalse(res.isEmpty());  
        assertEquals(res.size(),3);  
  
        verify(categoryRepo).findAll();  
  
    }  
    // test de busqueda  
    @Test  
    void givenCategoryId_whenFindById_thenReturnsCategory() throws Exception {  
        Long ID = 1L;  
        List<Category> categories = Arrays.asList(category1, category2, category3);  
  
        when(categoryRepo.findById(ID)).thenReturn(Optional.of(category1));  
  
        Category cat = categoryService.findById(ID);  
  
        assertNotNull(cat);  
        assertEquals(category1.getIdCategory(), cat.getIdCategory());  
        assertEquals(category1.getCategoryName(), cat.getCategoryName());  
        assertEquals(category1.getCategoryDescription(), cat.getCategoryDescription());  
        assertEquals(category1.isCategoryEnabled(), cat.isCategoryEnabled());  
        verify(categoryRepo).findById(ID);  
    }  
  
    @Test  
    void givenInvalidId_whenFindById_thenThrowsException() {  
        Long invalidId = 99L;  
        when(categoryRepo.findById(invalidId)).thenReturn(Optional.empty());  
  
        assertThrows(ModelNotFoundException.class, () -> {  
            categoryService.findById(invalidId);  
        });  
  
        verify(categoryRepo).findById(invalidId);  
    }  
  
    // test de guardar category  
    @Test  
    void givenValidCategory_whenSave_thenReturnsSavedCategory() throws Exception {  
  
        Category catsave = new Category(null, "Tecnología",  
                "Productos relacionados con computadoras, teléfonos y otros dispositivos electrónicos.", true);  
  
        when(categoryRepo.save(catsave)).thenReturn(category1);  
  
        Category res = categoryService.save(catsave);  
  
        assertNotNull(res);  
        assertEquals(category1.getIdCategory(), res.getIdCategory());  
        assertEquals(category1.getCategoryName(), res.getCategoryName());  
        assertEquals(category1.getCategoryDescription(), res.getCategoryDescription());  
        assertEquals(category1.isCategoryEnabled(), res.isCategoryEnabled());  
  
        verify(categoryRepo).save(catsave);  
    }  
  
    // test de modificar  
    @Test  
    void givenValidCategory_whenUpdate_thenReturnsUpdatedCategory() throws Exception {  
  
        Long ID = 1L;  
        Category newCat = new Category(1L, "Mineral",  
                "Minerales", false);  
  
        when(categoryRepo.findById(ID)).thenReturn(Optional.of(category1));  
        when(categoryRepo.save(newCat)).thenReturn(newCat);  
        Category res = categoryService.update(newCat, ID);  
  
        log.info(res.toString());  
        assertNotNull(res);  
        assertEquals(newCat.getIdCategory(), res.getIdCategory());  
        assertEquals(newCat.getCategoryName(), res.getCategoryName());  
        assertEquals(newCat.getCategoryDescription(), res.getCategoryDescription());  
        assertEquals(newCat.isCategoryEnabled(), res.isCategoryEnabled());  
  
        verify(categoryRepo).findById(ID);  
        verify(categoryRepo).save(newCat);  
  
    }  
  
    //test de eliminar  
    @Test  
    void givenValidCategory_whenDelete_thenReturnsDeletedCategory() throws Exception {  
        Long ID = 1L;  
        when(categoryRepo.findById(ID)).thenReturn(Optional.of(category1));  
        when(categoryRepo.existsById(ID)).thenReturn(false);  
  
        categoryService.deleteById(ID);  
  
        verify(categoryRepo).findById(ID);  
        verify(categoryRepo).deleteById(ID);  
        verify(categoryRepo,times(1)).deleteById(ID);  
  
        assertFalse(categoryRepo.existsById(ID));  
    }  
}
```

# Tests Unitarios para CategoryService

| **Aspecto**                  | **Descripción**                                                                                                  |
|-----------------------------|----------------------------------------------------------------------------------------------------------------|
| **Uso de Mockito para mocks**     | Se mockea `CategoryRepo` para simular la capa de acceso a datos, aislando la lógica del servicio.               |
| **Inyección con `@InjectMocks`**  | `CategoryServiceImpl` es inyectado con los mocks para testear la lógica real del servicio sin dependencias reales. |
| **Inicialización común**          | En `@BeforeEach` se crean categorías reutilizables para evitar repetir datos en cada test.                     |
| **Pruebas cubiertas:**            | - Listar todas las categorías<br>- Buscar por ID (existente y no existente)<br>- Guardar una nueva categoría<br>- Actualizar una categoría<br>- Eliminar una categoría |
| **Validaciones realizadas**       | - Comprobación de no nulos y tamaño en listas<br>- Igualdad de campos en entidades<br>- Verificación de excepciones lanzadas<br>- Verificación de llamadas a métodos mocks (`verify`) |
| **Uso de logs**                   | Se usa `log.info` para imprimir datos durante las pruebas para mayor trazabilidad.                             |
| **Manejo de excepciones**         | Test que valida que se lance `ModelNotFoundException` si no se encuentra una categoría por ID.                |
| **Verificación de interacciones** | Uso de `verify()` para asegurar que los métodos del repositorio son llamados la cantidad esperada.           |

> **Resumen:**  
> Estos tests aseguran que la lógica de negocio en el servicio funcione correctamente con todas las operaciones CRUD y que maneje errores de forma adecuada, usando Mockito para simular la capa de persistencia.
