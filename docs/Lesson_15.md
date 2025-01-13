在 Java 中，`java.lang` 包提供了 Java 语言最核心的类，包括 `Object`, `String`, `StringBuilder`, `StringBuffer`, `Math`, `System`, `Runtime` 以及各种包装类（如 `Integer`, `Boolean` 等）。这些类在 Java 程序开发中极为常用和重要。下面将逐一介绍它们的主要特性与用法。

---

## 1. `Object` 类

1. **地位**
    - `java.lang.Object` 是 Java 类层次结构的根类，所有类都直接或间接继承自 `Object`。

2. **常见方法**
    - `public String toString()`：返回对象的字符串表示；一般建议重写（override）以便输出更有意义的信息。
    - `public boolean equals(Object obj)`：比较两个对象是否“逻辑相等”；默认比较地址，需要在子类中重写以比较内容。
    - `protected Object clone()`：创建并返回当前对象的浅拷贝（Shallow Copy）。实现克隆需实现 `Cloneable` 接口并重写该方法。
    - `public int hashCode()`：返回对象哈希值，通常和 `equals` 同时重写并遵循相等对象哈希一致的原则。
    - `protected void finalize()`：在垃圾回收前的回调，不建议使用。

3. **示例**
   ```java
   public class Person {
       private String name;
       private int age;

       public Person(String name, int age) {
           this.name = name;
           this.age = age;
       }

       @Override
       public String toString() {
           return "Person{name='" + name + "', age=" + age + "}";
       }

       @Override
       public boolean equals(Object obj) {
           if (this == obj) return true;
           if (obj == null || getClass() != obj.getClass()) return false;
           Person person = (Person) obj;
           return age == person.age && name.equals(person.name);
       }

       @Override
       public int hashCode() {
           // 简单实现
           return name.hashCode() * 31 + age;
       }
   }
   ```

---

## 2. `String`、`StringBuilder`、`StringBuffer`

### 2.1 `String`

1. **特点**
    - `String` 对象**不可变**（Immutable）；一旦创建就无法修改内部字符序列。
    - 存储在**字符串常量池**（String Pool）中的字符串字面量可以被重复引用以节省空间。

2. **常见操作**
    - `length()`：返回字符串长度；
    - `charAt(int index)`：获取指定位置字符；
    - `substring(int beginIndex, int endIndex)`：截取子串；
    - `equals()` / `equalsIgnoreCase()`：比较字符串内容是否相同；
    - `indexOf()`, `lastIndexOf()`：查找子串位置；
    - `split(String regex)`, `replaceAll()`：分割与替换等。

3. **示例**
   ```java
   String str1 = "Hello";
   String str2 = "Hello";
   System.out.println(str1 == str2);      // true，同一个常量池引用
   System.out.println(str1.equals(str2)); // true，内容相同
   ```

### 2.2 `StringBuilder` 和 `StringBuffer`

1. **相同点**
    - 都是可变字符序列，用于在需要频繁拼接字符串时提升效率；
    - 提供常见操作，如 `append()`, `insert()`, `delete()`, `reverse()` 等。

2. **区别**
    - `StringBuilder`：**线程不安全**，但效率高，常用于**单线程**场景；
    - `StringBuffer`：**线程安全**，在大多数方法上都加了 `synchronized` 修饰，效率比 `StringBuilder` 略低，常用于**多线程**场景。

3. **示例**
   ```java
   StringBuilder sb = new StringBuilder("Hello");
   sb.append(" World"); 
   System.out.println(sb.toString()); // Hello World

   StringBuffer sbf = new StringBuffer("Java");
   sbf.insert(4, " SE");
   System.out.println(sbf.toString()); // Java SE
   ```

---

## 3. `Math` 类

1. **用途**
    - 提供**常用数学函数**与数学常量，如三角函数、指数函数、开方、绝对值、随机数等。

2. **常见方法**
    - `Math.abs(-5)` 返回 5
    - `Math.max(a, b)`, `Math.min(a, b)`
    - `Math.sqrt(16)` 返回 4.0
    - `Math.pow(2, 3)` 返回 8.0
    - `Math.random()` 返回 `[0,1)` 范围的随机 `double` 值
    - `Math.ceil()`, `Math.floor()`, `Math.round()` 等。

3. **常量**
    - `Math.PI`：圆周率 π
    - `Math.E`：自然常数 e

4. **示例**
   ```java
   System.out.println(Math.abs(-10));    // 10
   System.out.println(Math.sqrt(25));   // 5.0
   System.out.println(Math.pow(3, 2));  // 9.0
   System.out.println(Math.max(10, 20));// 20
   ```

---

## 4. `System` 类

1. **用途**
    - 提供**系统级**操作方法和属性，如标准输入输出、系统属性、垃圾回收等。

2. **常见方法**
    - `System.currentTimeMillis()`：获取当前时间的毫秒值。
    - `System.exit(int status)`：终止当前 Java 虚拟机。
    - `System.gc()`：提示进行垃圾回收。
    - `System.arraycopy(Object src, int srcPos, Object dest, int destPos, int length)`：数组复制。
    - `System.getProperty(String key)`：获取系统属性（如 `os.name`, `user.home` 等）。

3. **示例**
   ```java
   System.out.println("Current Time: " + System.currentTimeMillis());
   System.out.println(System.getProperty("os.name"));    // Windows 10
   System.arraycopy(srcArr, 0, destArr, 0, srcArr.length);
   ```

---

## 5. `Runtime` 类

1. **用途**
    - `Runtime` 类代表 Java 程序的**运行时环境**，可用于获取运行时信息并执行系统命令。

2. **获取实例**
    - `Runtime.getRuntime()`：返回 `Runtime` 类的唯一实例（单例模式）。

3. **常见方法**
    - `exec(String command)`：执行系统命令，如 `"notepad"`，`"calc"` 等；
    - `freeMemory()`：返回 Java 虚拟机中的空闲内存量；
    - `totalMemory()`：Java 虚拟机能使用的总内存量；
    - `gc()`：运行垃圾回收（与 `System.gc()` 类似）。

4. **示例**
   ```java
   Runtime rt = Runtime.getRuntime();
   System.out.println("Free memory: " + rt.freeMemory());
   try {
       rt.exec("notepad"); // 在Windows上打开记事本
   } catch (IOException e) {
       e.printStackTrace();
   }
   ```

---

## 6. 包装类（Wrapper Class）

### 6.1 列表

- **原始类型** 和 **包装类** 的对应关系：  
  | 基本类型  | 包装类       |
  |-----------|--------------|
  | `byte`    | `Byte`       |
  | `short`   | `Short`      |
  | `int`     | `Integer`    |
  | `long`    | `Long`       |
  | `float`   | `Float`      |
  | `double`  | `Double`     |
  | `char`    | `Character`  |
  | `boolean` | `Boolean`    |

### 6.2 作用

1. **对象化**
    - 有些场合（如集合框架、泛型）只能使用对象，需将基本类型**包装**成对象。
    - Java 5 引入**自动装箱**（Autoboxing）和**自动拆箱**（Unboxing），简化了包装与基本类型之间的转换。

2. **提供常用方法**
    - `parseXxx(String s)`：将字符串转换为基本类型；
    - `valueOf(String s)`：返回包装类对象；
    - `xxxValue()`：将包装类对象转换为对应的基本类型；
    - 各种**常量**，如 `Integer.MAX_VALUE`, `Double.POSITIVE_INFINITY` 等。

### 6.3 示例

```java
Integer iObj = 10;         // 自动装箱 -> Integer.valueOf(10)
int i = iObj;              // 自动拆箱 -> iObj.intValue()

// 常见方法
int val = Integer.parseInt("123");  // 123
Integer num = Integer.valueOf("456");
System.out.println(Integer.MAX_VALUE);  // 2147483647

Boolean bObj = Boolean.valueOf("true");
boolean b = bObj; // true
```

---

## 7. 小结

1. **`Object`**
    - Java 的根类，提供基础的 `toString()`, `equals()`, `hashCode()`, `clone()` 等方法。

2. **`String`、`StringBuilder`、`StringBuffer`**
    - `String`：不可变，常用于字符串常量；
    - `StringBuilder`：可变字符序列，线程不安全但效率高；
    - `StringBuffer`：可变字符序列，线程安全，效率略低。

3. **`Math`**
    - 常用数学函数与常量，如 `abs`, `pow`, `sqrt`, `random`, `PI` 等。

4. **`System`**
    - 系统级操作，提供 `currentTimeMillis()`, `arraycopy()`, `getProperty()`, `exit()` 等方法。

5. **`Runtime`**
    - 运行时环境信息与操作，如 `exec()` 执行系统命令、内存信息、垃圾回收触发等。

6. **包装类（Wrapper Class）**
    - 为基本类型提供对象形态和实用方法；
    - 支持自动装箱拆箱，并提供 `parseXxx`, `valueOf` 等静态方法和各种常量。

`java.lang` 包下的这些核心类极大地简化了开发工作，也是编写 Java 程序时最常用到的基础工具。掌握它们的用法和特点，能够提升编程效率与代码质量。祝你在学习和项目中熟练运用这些类，写出更优雅、高效的 Java 代码！