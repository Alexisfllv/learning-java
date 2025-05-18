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


## ModelMapper TypeMap

```java
@Configuration  
public class MapperConfig {  
  
  
    @Bean("defaultMapper")  
    public ModelMapper modelMapper() {  
        return new ModelMapper();  
    }  
  
    @Bean("categoryMapper")  
    public ModelMapper categoryMapper() {  
        ModelMapper mapper = new ModelMapper();  
  
        // handle missmatches  
        // lectura        mapper.createTypeMap(Category.class, Category_DTO.class)  
                .addMapping(e ->e.getCategoryName(),((destination, value) -> destination.setNombreCortoCategory((String) value)));  
  
        // escritura  
        mapper.createTypeMap(Category_DTO.class, Category.class)  
                .addMapping(e ->e.getNombreCortoCategory(),((destination, value) -> destination.setCategoryName((String) value)));  
  
  
        return mapper;  
    }  
}
```