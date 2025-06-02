## Generic Repo
```java
@NoRepositoryBean  
public interface GenericRepo<T,ID>  extends JpaRepository<T,ID> {  
}
```

## ModelRepo extends Generic

```java
@Repository  
public interface CategoryRepo extends GenericRepo<Category,Long> {  
}
```

---
## Generic Service
```java
public interface GenericService<T,ID> {  
    // CRUD  
    List<T> listAll();  
    T findById(ID id);  
    T save (T t);  
    T update(T t , ID id);  
    void deleteById(ID id);  
}
```

## Model Service

```java
public interface CategoryService extends GenericService<Category,Long> {  
  
}
```

---
## Generic Impl
```java
public abstract class CRUDIMPL<T,ID> implements GenericService<T,ID> {  
  
    // ioc  
    protected abstract GenericRepo<T,ID> getRepo();  
  
  
    @Override  
    public List<T> listAll() {  
        return getRepo().findAll();  
    }  
  
    @Override  
    public T findById(ID id) {  
        return getRepo().findById(id)  
                .orElseThrow(() -> new ModelNotFoundException("ITEM NO ENCONTRADO : "+id));  
    }  
  
    @Override  
    public T save(T t) {  
        return getRepo().save(t);  
    }  
  
    @Override  
    public T update(T t, ID id) {  
        return getRepo().save(t);  
    }  
  
    @Override  
    public void deleteById(ID id) {  
        getRepo().deleteById(id);  
    }  
}
```

## ModelImplement
```java
@RequiredArgsConstructor  
@Service  
public class CategoryServiceImpl extends CRUDIMPL<Category,Long> implements CategoryService {  
  
    private final CategoryRepo categoryRepo;  
    private final CategoryMapper categoryMapper;  
  
    @Override  
    protected GenericRepo<Category, Long> getRepo() {  
        return categoryRepo;  
    }
}
```

---
