## Clase ModelNotFoundExcepcion

```java
public class ModelNotFoundException extends RuntimeException {  
  
    public ModelNotFoundException(String message) {  
        super(message);  
    }  
}
```

## Mensaje de Error Custom

```java
public record CustomErrorResponse(  
        LocalDateTime datetime,  
        String message,  
        String path  
        ) {  
}
```

## Aplicar al Error Global Exception Handler

```java
@RestControllerAdvice  
public class GlobalErrorHandler {  
  
    @ExceptionHandler(ModelNotFoundException.class)  
    public ResponseEntity<CustomErrorResponse> handleModelNotFoundException(ModelNotFoundException ex , WebRequest req){  
        CustomErrorResponse error = new CustomErrorResponse  
                (LocalDateTime.now(), ex.getMessage(),req.getDescription(false));  
        return new ResponseEntity<>(error, HttpStatus.NOT_FOUND);  
    }
```

## Metodo con clase ProblemDetail java 17+

``` java
@ExceptionHandler(ModelNotFoundException.class)  
public ProblemDetail handleModelNotFoundException(ModelNotFoundException ex , WebRequest req){  
    ProblemDetail pd = ProblemDetail.forStatusAndDetail(HttpStatus.NOT_FOUND, ex.getMessage());  
    pd.setTitle("Model not found ");  
    pd.setType(URI.create(req.getContextPath()));  
    pd.setProperty("path", req.getDescription(false));  
    pd.setProperty("timestamp", LocalDateTime.now());  
    pd.setProperty("message", ex.getMessage());  
    return pd;  
}
```

## Implemntar como orElseThrow Supplier

```java
@Override  
public T findById(ID id) {  
    return getRepo().findById(id)  
            .orElseThrow(() -> new ModelNotFoundException("ITEM NO ENCONTRADO : "+id));  
}
```
