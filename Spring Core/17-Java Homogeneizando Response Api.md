### Clase Record GenResponse

```java
@JsonInclude(JsonInclude.Include.NON_NULL)  
public record GenResponse<T>(  
    int status,  
    String message,  
    List<T> data  
) {  
}
```

### Aplicando a Controller 

```java
@GetMapping  
public ResponseEntity<GenResponse<Category_DTO>> listAllCategory(){  
    List<Category_DTO> lists = mapperUtil.mapList(categoryService.listAll(), Category_DTO.class,"categoryMapper");  
    return  ResponseEntity.ok(new GenResponse<>(200, "success",lists));  
}  
  
@GetMapping("/{id}")  
public ResponseEntity<GenResponse<Category_DTO>> findById(@PathVariable Long id){  
    Category obj = categoryService.findById(id);  
    Category_DTO ct =  mapperUtil.map(obj, Category_DTO.class,"categoryMapper");  
    return ResponseEntity.ok(new GenResponse<>(200, "success", Arrays.asList(ct)));  
}
```

### Aplicando a Exception

```java
// GenResponse VERSION 2 CON RESPONSE  
@ExceptionHandler(ModelNotFoundException.class)  
public ResponseEntity<GenResponse<CustomErrorResponse>> handleModelNotFoundException(ModelNotFoundException ex , WebRequest req){  
    CustomErrorResponse error = new CustomErrorResponse  
            (LocalDateTime.now(), ex.getMessage(),req.getDescription(false));  
    return new ResponseEntity<>(new GenResponse<>(400, "error", Arrays.asList(error)), HttpStatus.NOT_FOUND);  
}
```