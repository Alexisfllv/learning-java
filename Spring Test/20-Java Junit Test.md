  

# 🧪 JUnit - Guía Esencial para Pruebas Unitarias en Java


> 📘 Framework principal para pruebas unitarias en Java. Permite verificar el comportamiento correcto del código de forma automática.

---
## ✅ ¿Qué es una prueba unitaria?

> Una prueba unitaria **verifica una unidad de código en aislamiento**, normalmente un método.
---
## 📦 Dependencia Maven


```xml

<dependency>
<groupId>org.junit.jupiter</groupId>
<artifactId>junit-jupiter</artifactId>
<version>5.10.0</version> <!-- Usa la versión más reciente -->
<scope>test</scope>
</dependency>

```

---

## 🧱 Estructura Básica de un Test

```java

import org.junit.jupiter.api.*;

class MiClaseTest {

@BeforeEach
void setUp() {
// Preparación antes de cada test
}

@Test
void miMetodoDebeHacerAlgo() {
// Arrange

// Act

// Assert

Assertions.assertEquals(esperado, obtenido);
}

@AfterEach
void tearDown() {
// Limpieza después de cada test
}

}
```

---
 ## 🧪 Anotaciones esenciales de JUnit

| Anotación       | Descripción                                                  |
|------------------|-------------------------------------------------------------|
| `@Test`          | Marca un método como prueba                                 |
| `@BeforeEach`    | Ejecutado **antes de cada** prueba                          |
| `@AfterEach`     | Ejecutado **después de cada** prueba                        |
| `@BeforeAll`     | Ejecutado **una vez antes** de todas las pruebas            |
| `@AfterAll`      | Ejecutado **una vez después** de todas las pruebas          |
| `@Disabled`      | Desactiva temporalmente una prueba                          |

---

## 🧪 Aserciones Comunes (`Assertions.assert...`)

| Método                    | Descripción                                  |
|---------------------------|----------------------------------------------|
| `assertEquals(a, b)`      | Verifica que `a == b`                        |
| `assertTrue(condición)`   | Verifica que la condición es `true`         |
| `assertFalse(condición)`  | Verifica que la condición es `false`        |
| `assertNull(valor)`       | Verifica que el valor es `null`             |
| `assertNotNull(valor)`    | Verifica que el valor **no** es `null`      |
| `assertThrows()`          | Verifica que se lanza una excepción         |

---
## 🧪 Test de Servicios con Mockito

```java

@ExtendWith(MockitoExtension.class)

class ServicioTest {

@Mock
private Dependencia dependencia;

@InjectMocks
private Servicio servicio;

  
@Test
void deberiaRetornarValorEsperado() {

Mockito.when(dependencia.metodo()).thenReturn("valor");

  
String resultado = servicio.llamar();

Assertions.assertEquals("valor", resultado);

}
}

```

---
## 🌐 Test de Controladores con MockMvc (Spring Boot)


```java
@WebMvcTest(MiControlador.class)
class MiControladorTest {

@Autowired
private MockMvc mockMvc;

@MockBean
private MiServicio servicio;

@Test
void debeRetornar200() throws Exception {
Mockito.when(servicio.listar()).thenReturn(List.of(...));

mockMvc.perform(get("/api/endpoint"))
.andExpect(status().isOk())
.andExpect(jsonPath("$.data", hasSize(3)));
}

}
```

---
## 🧠 Buenas Prácticas

- ✅ **Nombres descriptivos** para tests: qué hacen, no cómo.
- ✅ Mantén **una sola afirmación lógica** por test (si es posible).
- ✅ Usa `@BeforeEach` para evitar repetición.
- ✅ Simula dependencias con `@Mock` y `@MockBean` para pruebas aisladas.
- ❌ No dependas de bases de datos reales ni servicios externos.
---
## 📎 Recursos Útiles

- [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)
- [Mockito Documentation](https://site.mockito.org/)
- [Spring Boot Testing Docs](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.testing)