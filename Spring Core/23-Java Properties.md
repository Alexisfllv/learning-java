# Uso Correcto de Properties y Perfiles en Spring Boot (Java Backend)

## Introducción
Spring Boot utiliza archivos de configuración (`application.properties` o `application.yml`) para definir parámetros de la aplicación. Los **perfiles** permiten separar configuraciones específicas para distintos entornos (desarrollo, pruebas, producción, etc.), facilitando la gestión y despliegue.

---

## Archivos de configuración comunes

- `application.properties`  
  Contiene configuraciones comunes para todos los entornos.

- `application-dev.properties`  
  Configuraciones específicas para el perfil **dev** (desarrollo).

- `application-test.properties`  
  Configuraciones para pruebas automatizadas.

- `application-qa.properties`  
  Configuraciones para entorno de preproducción.

- `application-prod.properties`  
  Configuraciones para producción.

---
## Perfiles comunes y sus usos

| Perfil | Propósito                     | Características típicas                                 |
| ------ | ----------------------------- | ------------------------------------------------------- |
| `dev`  | Desarrollo local / equipo dev | BD local/dev, logging detallado, mocks opcionales       |
| `test` | Pruebas automatizadas         | BD en memoria (H2), sin side effects (no emails, mocks) |
| `qa`   | Preproducción                 | Similar a prod, para validación final                   |
| `prod` | Producción                    | Config segura, optimizada, sin logs detallados          |
|        |                               |                                                         |

---

### Nombre de Jar

```xml
<build>
  <finalName>sales-api</finalName>
</build>
```

## Ejecucion desde terminal 

```bash
mvn spring-boot:run -Dspring-boot.run.profiles=qa,prod
```

### Ejecucion desde jar 

``` xml
<plugin>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
```

``` bash
mvn clean package
```

```bash
java -jar target/sales-api.jar --spring.profiles.active=qa
```