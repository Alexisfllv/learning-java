## Pageable
### Service

```java
// Pagniador lista de categorias  
Page<Category> findPage(Pageable pageable);
```

### Impl

```java
@Override  
public Page<Category> findPage(Pageable pageable) {  
    return categoryRepo.findAll(pageable);  
}
```

### Controller

``` java
@GetMapping("/pagination")  
public ResponseEntity<Page<Category_DTO>> findPage(Pageable pageable){  
    Page<Category_DTO> page = categoryService.findPage(pageable).map(category -> mapperUtil.map(category, Category_DTO.class,"categoryMapper"));  
  
    return ResponseEntity.ok(page);  
}  
  
@GetMapping("/pagination2")  
public ResponseEntity<Page<Category_DTO>> findPage2(  
        @RequestParam(value = "page", defaultValue = "0") int page,  
        @RequestParam(value = "size", defaultValue = "10") int size  
){  
    Page<Category_DTO> pageDTO = categoryService.findPage(PageRequest.of(page,size)).map(category -> mapperUtil.map(category, Category_DTO.class,"categoryMapper"));  
  
    return ResponseEntity.ok(pageDTO);  
}
```


---

## Sort

```java
// Categorias ordenadas  
List<Category> listadodeCategorys(String param);
```

## Impl

```java
@Override  
public List<Category> listadodeCategorys(String param) {  
  
    Sort.Direction direction = param.equals("ASC") ? Sort.Direction.ASC : Sort.Direction.DESC;  
  
    return categoryRepo.findAll(Sort.by(direction,"categoryName"));  
}
```
## Controller

```java
@GetMapping("/order")  
public ResponseEntity<List<Category_DTO>> findOrderByName(  
        @RequestParam(name = "param", defaultValue = "ASC") String param  
) {  
    List<Category_DTO> lists = mapperUtil.mapList(categoryService.listadodeCategorys(param), Category_DTO.class,"categoryMapper");  
  
    return ResponseEntity.ok(lists);  
}
```