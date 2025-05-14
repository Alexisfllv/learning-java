# Proyectos Objetivos
## **Loom**
### Entregar hilos mas ligeros llamados "virtual threads".
## **Valhalla**
### Generar un tercer tipo : clases ,primitivos ,"values type".
## **Panama**
### Comunicación entre aplicaciones java y librerías en C.
## **Amber**
### Mejorar la sintaxis del lenguaje, agregar nuevas funcionalidades.

---
# Ingredientes
## Java Temurin 21 LTS JDK
## STS,VS,IJ

---
## **Aspectos basicos Java**

- ### *Metodo main*
- ### *Clases*
- ### *Atributos*
- ### *Variables y Promotions*
- ### *Modificadores de acceso*
- ### *Metodos accesores*
- ### *Uso de Lombok*
---
# Anotaciones principales de Lombok

- **@Getter / @Setter**: Genera automáticamente los métodos getter y setter para los campos de la clase.

- **@ToString**: Genera el método `toString()` incluyendo todos los campos (se puede excluir alguno).

- **@EqualsAndHashCode**: Genera los métodos `equals()` y `hashCode()` basados en los campos.

- **@NoArgsConstructor**: Genera un constructor sin argumentos.

- **@AllArgsConstructor**: Genera un constructor con todos los campos.

- **@RequiredArgsConstructor**: Genera un constructor con los campos `final` o anotados con `@NonNull`.

- **@Data**: Atajo que combina `@Getter`, `@Setter`, `@ToString`, `@EqualsAndHashCode` y `@RequiredArgsConstructor`.

- **@Builder**: Implementa el patrón Builder para construir objetos de forma fluida.

- **@Value**: Marca la clase como inmutable, similar a `@Data` pero con todos los campos `final` y sin setters.
---
# Funciones de Lombok

```xml
<dependency>  
    <groupId>org.projectlombok</groupId>  
    <artifactId>lombok</artifactId>  
    <optional>true</optional>  
</dependency>
```
---
## @ToString

* @ToString.Include
* @ToString.Exclude
### Comportamiento automatico
## @ToString(onlyExplicitlyIncluded = true)
---
## @EqualsAndHashCode

* @EqualsAndHashCode.Include
* @EqualsAndHashCode.Exclude
### Comportamiento automatico
## @EqualsAndHashCode(onlyExplicitlyIncluded = true)

---
## @Builder

### Creacion de objeto
```java
Product.ProductBuilder p1 = Product.builder();  
p1.id(1);  
p1.name("p1");  
System.out.println(p1.build());
```
Salida:
``` bash
Customer(idCustomer=2, name=dem2, isActive=false)
```
---
