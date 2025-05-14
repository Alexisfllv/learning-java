## SPRING DATA JPA - MYSQL PROPERTIES

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/rentadb
spring.datasource.username=tu_usuario
spring.datasource.password=tu_contraseña
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

# Dependencias Pom.xml

``` xml
<!-- DATA JPA -->  
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-data-jpa</artifactId>  
</dependency>

<!-- SPRING WEB -->  
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-web</artifactId>  
</dependency>

<!-- MYSQL -->  
<dependency>  
    <groupId>com.mysql</groupId>  
    <artifactId>mysql-connector-j</artifactId>  
    <scope>runtime</scope>  
</dependency>

<!-- LOMBOK -->  
<dependency>  
    <groupId>org.projectlombok</groupId>  
    <artifactId>lombok</artifactId>  
    <optional>true</optional>  
</dependency>

<!-- MODELMAPPER -->  
<dependency>  
    <groupId>org.modelmapper</groupId>  
    <artifactId>modelmapper</artifactId>  
    <version>3.2.0</version>  
</dependency>

<!-- SWAGGER -->  
<dependency>  
    <groupId>org.springdoc</groupId>  
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>  
    <version>2.3.0</version>  
</dependency>

```

## Configuración de ModelMapper

```java
@Configuration  
public class MapperConfig {  
    @Bean  
    public ModelMapper modelMapper() {  
        return new ModelMapper();  
    }  
}
```

