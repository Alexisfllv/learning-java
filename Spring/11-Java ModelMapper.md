## Inyección de ModelMapper

```java
private final ModelMapper modelMapper;
```
## Metodos de Transformación

```java
// conversion de parametros  entiy,dto .  dto,entity  
  
private Category_DTO convertTODto(Category category){  
    return modelMapper.map(category, Category_DTO.class);  
}  
  
private Category convertEntity(Category_DTO dto){  
    return modelMapper.map(dto, Category.class);  
}
```