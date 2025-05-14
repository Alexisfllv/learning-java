  

# Java Collections Framework (hasta Java 21)

  

El **Java Collections Framework (JCF)** proporciona una arquitectura de interfaces y clases para almacenar y manipular colecciones de objetos. Desde **Java 1.2** hasta **Java 21**, ha evolucionado significativamente, mejorando tanto la eficiencia como la facilidad de uso.

  

## Principales interfaces

  

1. **`Collection`**: La raíz de la jerarquía de colecciones. Subinterfaces más comunes:

- **`List`**: Lista ordenada y accesible por índice (ej. `ArrayList`, `LinkedList`).

- **`Set`**: Colección que no permite duplicados (ej. `HashSet`, `TreeSet`).

- **`Queue`**: Colección que sigue el principio FIFO (ej. `LinkedList`, `PriorityQueue`).

- **`Deque`**: Subinterfaz de `Queue` que permite agregar/eliminar elementos desde ambos extremos (ej. `ArrayDeque`).

  

2. **`Map`**: No es una subinterfaz de `Collection`. Mapea claves a valores (ej. `HashMap`, `TreeMap`, `LinkedHashMap`).

  

## Implementaciones comunes

  

- **`ArrayList`**: Implementación de `List` basada en arreglos. Proporciona acceso rápido por índice.

- **`LinkedList`**: Implementación de `List` y `Deque` basada en listas enlazadas. Más eficiente para inserciones/eliminaciones.

- **`HashSet`**: Implementación de `Set` basada en una tabla hash. No mantiene el orden de los elementos.

- **`TreeSet`**: Implementación de `Set` que mantiene los elementos ordenados según su orden natural o un `Comparator`.

- **`HashMap`**: Implementación de `Map` basada en una tabla hash. No mantiene el orden de las claves.

- **`LinkedHashMap`**: Implementación de `Map` que mantiene el orden de inserción de las claves.

- **`TreeMap`**: Implementación de `Map` que mantiene las claves ordenadas.

  

## Nuevas características en Java 8+

  

1. **Streams API**: Introducción de **Streams** en Java 8 para trabajar con colecciones de manera declarativa y funcional.

```java

list.stream().filter(p -> p.getId() > 2).forEach(System.out::println);

```

  

2. **`forEach`**: Método por defecto en las colecciones para iterar sobre los elementos.

```java

list.forEach(item -> System.out.println(item));

```

  

3. **`removeIf`**: Elimina elementos de una colección que satisfacen una condición.

```java

list.removeIf(p -> p.getId() < 3);

```

  

4. **`Collectors`**: Utiliza métodos de colección funcional para agrupar, contar, y recolectar elementos de un `Stream`.

```java

List<Product> filtered = list.stream().filter(p -> p.getId() > 2).collect(Collectors.toList());

```

  

## Java 9+ Mejoras

  

1. **`List.of`**, **`Set.of`**, **`Map.of`**: Métodos para crear colecciones inmutables de manera más sencilla.

```java

List<String> list = List.of("A", "B", "C");

Set<String> set = Set.of("A", "B", "C");

Map<Integer, String> map = Map.of(1, "One", 2, "Two");

```

  

2. **`Collection.copyOf`**: Crea una copia inmutable de una colección existente.

```java

List<String> list = List.copyOf(existingList);

```

  

## Java 10+ Mejoras

  

1. **`var`**: Introducción de la inferencia de tipos con `var` para simplificar el código.

```java

var list = new ArrayList<String>(); // No es necesario declarar el tipo explícitamente

```

  

## Java 11+ Mejoras

  

1. **`toArray`**: Nuevo método en `Collection` que permite convertir una colección en un arreglo de manera más eficiente.

```java

String[] array = list.toArray(String[]::new);

```

  

## Java 16+ Mejoras

  

1. **`Stream.toList()`**: Método conveniente para convertir un stream en una lista inmutable.

```java

List<Product> list = products.stream().toList();

```

  

## Java 17-21

  

1. **`List` y `Set` Inmutables**: Mejoras en la creación de colecciones inmutables. El método `List.of` y `Set.of` ahora se usa más ampliamente para colecciones inmutables.

2. **Mejoras de rendimiento**: Optimización interna de colecciones como `HashMap` y `TreeMap`.

  

## Conclusión

  

Las colecciones en Java han evolucionado mucho desde sus primeras versiones. Con la introducción de las **Streams** en Java 8 y las mejoras en versiones posteriores, trabajar con colecciones se ha vuelto más fácil, eficiente y expresivo. Para colecciones inmutables, puedes usar los métodos `of` en las últimas versiones de Java. Además, las operaciones funcionales como `forEach`, `filter`, y `map` proporcionan una manera poderosa de manejar datos en colecciones.