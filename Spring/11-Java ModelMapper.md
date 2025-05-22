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


## Creacion de genericos para llamada de Mapper
#### metodos para mapeo en listas u objetos.

```java
@Component  
@RequiredArgsConstructor  
public class MapperUtil {  
  
  
    private final ApplicationContext applicationContext;  
  
  
    // lista  
  
    public <S,T> List<T> mapList(List<S> source, Class<T> targetClass, String... mapperQualifier){  
  
        ModelMapper modelMapper = getModelMapper(mapperQualifier);  
  
        return source.stream()  
                .map(sourceElement -> modelMapper.map(sourceElement, targetClass))  
                .collect(Collectors.toList());  
    }  
  
    // un solo elemento  
  
    public <S,T> T map(S source, Class<T> targetClass, String... mapperQualifier){  
        ModelMapper modelMapper = getModelMapper(mapperQualifier);  
        return modelMapper.map(source, targetClass);  
    }  
  
    private ModelMapper getModelMapper(String[] mapperQualifier) {  
  
        if(mapperQualifier.length == 0 || mapperQualifier == null || mapperQualifier[0].isEmpty()){  
            return applicationContext.getBean("defaultMapper",ModelMapper.class);  
        }else{  
            return applicationContext.getBean(mapperQualifier[0],ModelMapper.class);  
        }  
    }  
}
```