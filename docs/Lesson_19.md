在 Java 中，**集合框架（Collections Framework）**是一套专门为存储和操作对象而设计的类和接口结构，主要位于 `java.util` 包下。它将常见的数据结构（如**列表**、**集合**、**映射**等）进行了抽象定义与具体实现，大大提升了开发效率与可维护性。下面将从总体概念、核心接口、以及继承关系图等方面进行介绍。

---

## 一、集合框架的总体概念

1. **目的**
    - 统一管理和操作各种**容器**（如 `List`, `Set`, `Queue`, `Map` 等），提供**标准化**的 API。
    - 实现**不同特性**的数据结构（如哈希表、链表、红黑树等），让开发者可以根据需要灵活选择。

2. **优点**
    - **可维护性**：统一的接口与抽象让代码更易读、易维护。
    - **可扩展性**：可以基于集合框架编写自己的容器实现。
    - **丰富的功能**：常见数据结构、并发集合（如 `ConcurrentHashMap`）等都已内置。

---

## 二、Collection、Map 两大根接口

在 Java 集合框架中，有两条最主要的分支：
- **`Collection`**：存储**单列数据**（元素），如 `List`, `Set`, `Queue`；
- **`Map`**：存储**键值对**（key-value），如 `HashMap`, `LinkedHashMap`, `TreeMap`。

### 2.1 `Collection` 接口

1. **概念**
    - `Collection` 是**单列**集合层次的**根接口**，里面的元素一般是可迭代（`Iterator`）的，对外部只暴露“元素”这一层概念。

2. **子接口**
    - `List`：元素**有序**、可重复。如 `ArrayList`, `LinkedList`, `Vector` 等。
    - `Set`：元素**无序**、不可重复。如 `HashSet`, `LinkedHashSet`, `TreeSet`。
    - `Queue` / `Deque`：队列或双端队列结构。如 `LinkedList` 也可实现 `Deque`，`ArrayDeque`。

3. **常见方法**
    - `add(E e)`, `remove(Object o)`, `size()`, `isEmpty()`, `contains(Object o)`, `clear()`, `iterator()`, 等。

### 2.2 `Map` 接口

1. **概念**
    - `Map` 用于存储**键值对**（key-value），key 不可重复，一个 key 对应一个 value。

2. **子接口 & 常见实现类**
    - `HashMap`：基于哈希表实现；key 的顺序不保证；
    - `LinkedHashMap`：在 `HashMap` 的基础上用**双向链表**维护插入顺序；
    - `TreeMap`：基于红黑树实现，key 有序；
    - `Hashtable`：早期线程安全版本，已不常用；
    - `ConcurrentHashMap`：并发环境下使用的线程安全 Map。

3. **常见方法**
    - `put(K key, V value)`, `get(Object key)`, `remove(Object key)`, `containsKey(Object key)`, `keySet()`, `values()`, `entrySet()`, 等。

---

## 三、继承关系图（示意）

下面给出一个简化的 Java 集合框架继承关系**示意图**（不含并发与队列等所有细节，仅展示常见核心类）：

``` 
                    ┌────────────────────┐
                    │      Iterable      │
                    └─────────┬──────────┘
                              │
                    ┌─────────▼───────────┐
                    │     Collection<E>    │  (接口)
                    └──────┬─────┬────────┘
                           │     │
                 ┌────────┴┐ ┌──┴──────────────────┐
                 │  List<E>│ │         Set<E>       │  (接口)
                 │  (接口) │ │         (接口)       │
                 └─────┬───┘ └─────┬────────────────┘
                       │           │
             ┌─────────▼────────┐ │     ┌──────────────────────┐
             │  ArrayList<E>     │ │     │     HashSet<E>       │
             │ LinkedList<E>     │ │     │ LinkedHashSet<E>     │
             │ Vector<E>         │ │     │     TreeSet<E>       │
             └────────────────────┘ │     └──────────────────────┘
                                     │
                                     │
             ┌───────────────────────▼───────────────────────────┐
             │                     Map<K,V>                      │  (接口)
             └─────────┬──────────────────────────────────────────┘
                       │
             ┌─────────▼─────────────┐
             │      HashMap<K,V>      │
             │   LinkedHashMap<K,V>   │
             │      TreeMap<K,V>      │
             │      Hashtable<K,V>    │
             └────────────────────────┘
```

- **Iterable**：最顶层的可迭代接口，让 `for-each` 循环可以遍历集合。
- **Collection**：单列集合的根接口。
- **List**、**Set**：作为 Collection 子接口，对有序/无序、可重复/不可重复做了不同约束。
- **Map**：与 Collection 同级，用 key-value 方式存储数据，不继承 Collection，但常被一同归纳为 Java 集合框架的一部分。

---

## 四、总结

1. **Java 集合框架**分为两大核心分支：
    - **`Collection`** 系列：**单列**集合，如 `List`, `Set`, `Queue`；
    - **`Map`** 系列：**键值对**集合，如 `HashMap`, `TreeMap` 等。

2. **`Collection`**
    - `List`：有序、可重复；
    - `Set`：无序（或排好特殊序）、不可重复；
    - 扩展的 `Queue`、`Deque` 为特殊的**队列**与**双端队列**模型。

3. **`Map`**
    - 键不可重复；
    - `HashMap`/`LinkedHashMap` 基于哈希表；`TreeMap` 基于红黑树，支持按 key 排序；
    - `Hashtable` 是老版本线程安全 map。

4. **继承关系**
    - `Iterable` → `Collection` → `List` / `Set` / …；
    - `Map` 与 `Collection` 并列，不继承彼此，但常被集合在一起称为“集合框架”。

掌握 Java 集合框架的结构与核心特性，有助于在开发中根据需求（顺序、重复性、线程安全、排序等）选择最合适的实现类，从而得到更高效率与更佳可维护性的代码。祝你在项目中合理运用这些容器！