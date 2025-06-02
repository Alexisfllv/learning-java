### Metodo de Recuperacion de ID a un generico
#### Recuperar la metodos o clases  en tiempo de ejecución

```java
@Override  
public T update(T t, ID id) {  
    try {  
        Class<?> clazz = t.getClass();  
        String methodName = "setId" + clazz.getSimpleName();  
        Method setIdMethod = clazz.getMethod(methodName, id.getClass());  
        setIdMethod.invoke(t, id);  
  
        getRepo().findById(id)  
                .orElseThrow(() -> new ModelNotFoundException("ITEM NO ENCONTRADO: " + id));  
        return getRepo().save(t);  
    } catch (NoSuchMethodException | IllegalAccessException | InvocationTargetException e) {  
        throw new RuntimeException("Error al usar reflexión en update()", e);  
    }  
}
```