## Perfiles para ejecutar

```java

// clase security
@Profile(value = {"dev","qa","prod"})

// test controller
@ActiveProfiles("test")  
@AutoConfigureMockMvc(addFilters = false)

```