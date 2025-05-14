  
# ☕ Stream Básico - Java

## 📁 Carpeta: `StreamBasic01`

---
## 🔹 filter()

Filtra elementos del stream según una condición booleana.

```java

.stream().filter(person -> person.getName().length() >= 5)

```

Ejemplo: mostrar personas con nombres de 5 letras o más.

---
## 🔹 map()

Transforma los elementos del stream. Ejemplo: multiplicar el salario x2.

```java

.stream()

.filter(p -> p.getName().length() >= 5)

.map(p -> new Person(p.getId(), p.getName(), p.getSalary().multiply(new BigDecimal("2"))))

```

---
## 🔹 forEach()

Ejecuta una acción por cada elemento. Ejemplo: imprimir o convertir a mayúsculas.

```java
.stream()
.map(p -> new Person(p.getId(), p.getName().toUpperCase(), p.getSalary()))
.forEach(System.out::println)
```

---
## 🔹 sorted()

Ordena los elementos del stream.
```java
// Orden natural
.stream().sorted()

// Orden por nombre
.stream().sorted(Comparator.comparing(Person::getName))
```

---
## 🧾 Modelo Person

Usado en todos los ejemplos:

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
class Person {
Long idPerson;
String name;
BigDecimal salary;
}
```

---

# 📁 Collectors02 - Resumen

  

## 1. GroupingByExample

- 🎯 **Objetivo:** Agrupar elementos por una clave.

- ✅ **Ventaja:** Organización por categoría (ej. por nombre).

- 🔁 **Retorno:** `Map<K, List<V>>`

  

## 2. PartitioningByExample

- 🎯 **Objetivo:** Separar en dos grupos lógicos (true/false).

- ✅ **Ventaja:** Útil para divisiones binarias (ej. salario > 300).

- 🔁 **Retorno:** `Map<Boolean, List<V>>`

  

## 3. MappingExample

- 🎯 **Objetivo:** Transformar los valores agrupados.

- ✅ **Ventaja:** Permite personalizar qué se guarda por grupo.

- 🔁 **Retorno:** `Map<K, List<M>>`

  

## 4. SummarizingExample

- 🎯 **Objetivo:** Resumen estadístico de números.

- ✅ **Ventaja:** Obtienes count, sum, min, max y avg en uno.

- 🔁 **Retorno:** `DoubleSummaryStatistics`

  

## 5. CountingExample

- 🎯 **Objetivo:** Contar elementos por grupo.

- ✅ **Ventaja:** Más expresivo que contar manualmente.

- 🔁 **Retorno:** `Map<K, Long>`

  

## 6. JoiningExample

- 🎯 **Objetivo:** Concatenar Strings del stream.

- ✅ **Ventaja:** Ideal para mostrar texto unido (ej. nombres).

- 🔁 **Retorno:** `String`