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

---

## Excepciones Adicionales ,Validaciones , Excepcion Padre

#### extends `ResponseEntityExceptionHandler`

```java
@RestControllerAdvice  
public class GlobalErrorHandler extends ResponseEntityExceptionHandler {  
  
    // Exception  
    @ExceptionHandler(Exception.class)  
    public ResponseEntity<CustomErrorResponse> handleDefaultException(Exception ex , WebRequest req){  
        CustomErrorResponse error = new CustomErrorResponse  
                (LocalDateTime.now(), ex.getMessage(),req.getDescription(false));  
        return new ResponseEntity<>(error, HttpStatus.INTERNAL_SERVER_ERROR);  
    }  
  
  
  
//    // datos mal enviados @Valid  
//    @ExceptionHandler(MethodArgumentNotValidException.class)  
//    public ResponseEntity<CustomErrorResponse> handleMethodArgumentNotValidException(MethodArgumentNotValidException ex , WebRequest req){  
//        CustomErrorResponse error = new CustomErrorResponse  
//                (LocalDateTime.now(), ex.getMessage(),req.getDescription(false));  
//        return new ResponseEntity<>(error, HttpStatus.BAD_REQUEST);  
//    }  
  
  
    // metodo sobreescrito de ReponseEntityExceptionHandler  
    @Override  
    protected ResponseEntity<Object> handleMethodArgumentNotValid(MethodArgumentNotValidException ex, HttpHeaders headers, HttpStatusCode status, WebRequest req) {  
  
        String message = ex.getBindingResult().getFieldErrors().stream()  
                .map( error -> error.getField() + " : " + error.getDefaultMessage()).collect(Collectors.joining(","));  
  
        CustomErrorResponse error = new CustomErrorResponse  
                (LocalDateTime.now(), message,req.getDescription(false));  
        return new ResponseEntity<>(error, HttpStatus.BAD_REQUEST);  
    }  
}
```