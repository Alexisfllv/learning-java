## ¿Qué son?

Los **genéricos** permiten que clases, interfaces y métodos trabajen con **tipos parametrizados**, mejorando la seguridad y reutilización de código.

  
---

## 📌 Ventajas

- Detectan errores en tiempo de compilación.
- Evitan castings innecesarios.
- Permiten código más limpio y reutilizable.

---

## 🧠 Sintaxis básica

```java

public class Caja<T> {

private T contenido;

public void guardar(T contenido) { this.contenido = contenido; }

public T obtener() { return contenido; }

}

```

Uso:

```java

Caja<String> caja = new Caja<>();

caja.guardar("Hola");

String texto = caja.obtener();

```

---

## 🔼 Upper Bounded Wildcard


```java

List<? extends Number>

```


> Acepta `Number` o cualquier subclase como `Integer`, `Double`, etc.

✔ Puedes **leer**

❌ No puedes **escribir** (excepto `null`)

---
## 🔽 Lower Bounded Wildcard


```java

List<? super Integer>

```

> Acepta `Integer` o cualquier superclase como `Number`, `Object`, etc.

❌ Lectura limitada

✔ Puedes **escribir** objetos `Integer`

---

## 📊 Comparación rápida


## 📊 Comparación de Wildcards en Java

| Wildcard         | ¿Qué acepta?       | ¿Lectura? | ¿Escritura? |
|------------------|--------------------|-----------|--------------|
| `<? extends T>`  | `T` y subclases    | ✅ Sí      | ❌ No         |
| `<? super T>`    | `T` y superclases  | ❌ Limitado| ✅ Sí         |


---

## 🧬 Jerarquía útil (ejemplo con Number)


```

Object

└── Number

├── Integer

├── Double

├── Float

└── Long

```


``` mathematica
Object
├── Number
│   ├── Byte
│   ├── Short
│   ├── Integer
│   ├── Long
│   ├── Float
│   └── Double
├── Boolean
├── Character
├── String
├── Enum<E extends Enum<E>>
├── Throwable
│   ├── Error
│   └── Exception
│       ├── RuntimeException
│       └── (otras excepciones)
├── Iterable<T>
│   └── Collection<E>
│       ├── List<E>
│       │   ├── ArrayList
│       │   ├── LinkedList
│       │   └── Vector
│       ├── Set<E>
│       │   ├── HashSet
│       │   ├── TreeSet
│       │   └── LinkedHashSet
│       └── Queue<E>
│           ├── PriorityQueue
│           └── LinkedList
├── Map<K, V>
│   ├── HashMap
│   ├── TreeMap
│   ├── LinkedHashMap
│   └── Hashtable
└── Thread

```

WildCards