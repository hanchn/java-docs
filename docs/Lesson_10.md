
在 Java 的面向对象编程（OOP）体系中，**继承（Inheritance）**是一种非常重要的机制。它允许一个类（子类）继承另一个类（父类）或基类的属性和方法，从而实现**代码复用**和**层次化设计**。同时，结合关键字 `extends`、`super` 以及方法重写与重载等概念，可以让我们的代码更加灵活和可维护。下面将详细介绍继承的核心要点。

---

## 一、继承（Inheritance）

1. **概念**
    - 继承是一种**类与类之间**的**层次关系**，子类（subclass）可以**继承**父类（superclass）的**属性和方法**，从而**复用**父类的成员并**扩展**或**改写**父类的功能。

2. **实现方式**
    - 在 Java 中使用 `extends` 关键字来表示继承关系：
      ```java
      public class 子类 extends 父类 {
          // 子类特有的属性和方法
      }
      ```
    - 一个类最多只能有**一个**直接父类（单继承），但可通过**多层继承**实现继承链。

3. **好处**
    - **提高代码复用性**：避免重复编写相同逻辑，将通用的属性或方法放在父类里，子类自动继承。
    - **扩展性和可维护性**：子类可以对父类的功能进行扩展或改写。

4. **注意事项**
    - Java 不支持**多继承**（即一个子类不能同时继承多个父类），但可以使用**接口**（`implements`）来弥补这一限制。
    - 如果一个类没有显式继承任何类，那么它默认继承自 `java.lang.Object`。

---

## 二、extends 关键字

1. **语法**
   ```java
   public class Dog extends Animal {
       // Dog 类是 Animal 类的子类
   }
   ```

2. **继承关系举例**
   ```java
   public class Animal {
       protected String name;
       public void eat() {
           System.out.println(name + " is eating...");
       }
   }

   public class Dog extends Animal {
       // Dog 继承了 Animal 的 name 属性和 eat() 方法
       public void bark() {
           System.out.println(name + " is barking...");
       }
   }
   ```
    - 子类 `Dog` 可以直接使用 `name` 属性和 `eat()` 方法，无需重复定义。
    - 由于 `name` 被 `protected` 修饰，子类可以直接访问它；若是 `private`，则需要父类提供 `get/set` 方法来访问。

---

## 三、super 关键字

1. **概念**
    - `super` 代表**父类对象**的引用，可用于在子类中**访问或调用**父类的成员（属性、方法、构造方法）。

2. **主要用法**
    1. **调用父类的属性或方法**
       ```java
       public class Dog extends Animal {
           public void showName() {
               // 访问父类的 name 属性
               System.out.println("Name: " + super.name);
               // 调用父类的 eat() 方法
               super.eat();
           }
       }
       ```
    2. **调用父类构造方法**
        - 在子类构造方法的**第一行**使用 `super(...)` 来调用**父类**的**有参/无参**构造方法。
       ```java
       public class Animal {
           public Animal(String name) {
               this.name = name;
           }
       }
 
       public class Dog extends Animal {
           public Dog(String name) {
               super(name); // 调用父类的有参构造方法
           }
       }
       ```
        - 如果子类构造方法中没有显式调用 `super(...)`，系统会默认调用父类的**无参构造方法**（前提是父类有无参构造）。

3. **注意**
    - `super(...)` 或 `this(...)` 调用构造方法时，**只能出现在构造方法体的第一行**。
    - 如果父类只有**有参构造**而没有**无参构造**，则子类必须显式调用父类有参构造，否则会报错。

---

## 四、方法重写（override）与方法重载（overload）

1. **方法重写（Override）**
    - **定义**：子类**重新实现**与父类方法**同名、同参数列表、同返回值类型**的方法，以实现**不同的功能**。
    - **关键点**：
        - 子类与父类的方法签名（方法名、参数列表）必须**完全相同**。
        - 返回类型可以是**父类方法返回类型的子类型**（协变返回类型）。
        - 访问修饰符不能比父类方法更严格，如父类是 `public`，子类也必须是 `public`。
        - 可以使用 `@Override` 注解来让编译器校验重写的正确性。
    - **示例**：
      ```java
      public class Animal {
          public void eat() {
              System.out.println("Animal is eating.");
          }
      }
 
      public class Dog extends Animal {
          @Override
          public void eat() { 
              // 重写父类的 eat()
              System.out.println("Dog is eating dog food.");
          }
      }
      ```

2. **方法重载（Overload）**
    - **定义**：在**同一个类**或**父子类**中，存在**同名不同参数列表**（参数个数或类型不同）的方法，是一种**编译时多态**。
    - **关键点**：
        - 方法名称相同，但**参数类型或个数不同**。
        - 与返回类型或访问修饰符无关，仅看方法签名。
    - **示例**：
      ```java
      public class Calculator {
          public int add(int a, int b) {
              return a + b;
          }
 
          public double add(double a, double b) {
              return a + b;
          }
      }
      ```
    - 重载与继承没有必然联系，但父子类也能通过**方法签名**的不同（参数列表不同）来实现重载。

3. **区别总结**

   | 对比       | 方法重写（override）                             | 方法重载（overload）                                 |
      |------------|--------------------------------------------------|-------------------------------------------------------|
   | 发生场景   | 父类与子类之间                                   | 同一个类、或父子类（只要参数列表不同也可以算重载）    |
   | 方法签名   | **完全相同**（方法名、参数列表、返回类型兼容）   | **参数列表不同**，方法名相同                          |
   | 访问修饰符 | 子类方法不能比父类更严格                          | 与访问修饰符无关                                      |
   | 体现的多态 | **运行时多态**                                    | **编译时多态**                                        |
   | 是否有 `@Override` 注解 | 通常加 `@Override` 标注编译器进行检查           | 不适用                                               |

---

## 五、final 类和 final 方法

1. **final 类**
    - 被 `final` 修饰的类**不可被继承**。
    - **示例**：`public final class String { ... }`
    - `String` 类就是一个 `final` 类，它无法被继承。

2. **final 方法**
    - 被 `final` 修饰的方法**不能被子类重写**。
    - 用于**锁定方法**，确保其在子类中保持一致的实现。
    - **示例**：
      ```java
      public class Animal {
          public final void run() {
              System.out.println("Animal is running.");
          }
      }
 
      public class Dog extends Animal {
          // 这里不能重写 run() 方法，否则编译报错
          // public void run() { ... } // Error
      }
      ```

3. **使用场景**
    - 当你不希望某个类被继承，或不希望某个方法被覆盖时，就可以使用 `final`。
    - 有时出于安全或逻辑的原因，需要强制某些行为在所有子类中保持不变，就可以将其定义为 `final`。

---

## 六、综合示例

下面是一段完整的示例，演示了继承、`extends`、`super` 关键字、方法重写以及 `final` 的用法。

```java
// 父类
public class Animal {
    protected String name;

    public Animal(String name) {
        this.name = name;
    }

    public void eat() {
        System.out.println(name + " is eating...");
    }
    
    public final void breathe() {
        System.out.println(name + " is breathing...");
    }
}

// 子类
public class Dog extends Animal {

    public Dog(String name) {
        // 调用父类的有参构造
        super(name);
    }

    // 重写父类的 eat() 方法
    @Override
    public void eat() {
        System.out.println(name + " is eating dog food.");
    }

    // 定义子类特有的方法
    public void bark() {
        System.out.println(name + " is barking...");
    }

    // 由于父类的 breathe() 是 final，这里不能重写
    // public void breathe() { ... } // Error
}

public class MainApp {
    public static void main(String[] args) {
        Animal animal = new Animal("Generic Animal");
        animal.eat();      // Generic Animal is eating...
        animal.breathe();  // Generic Animal is breathing...

        // 多态：父类引用指向子类对象
        Animal dog = new Dog("Buddy");
        dog.eat();         // Buddy is eating dog food. (运行时多态，调用 Dog 的 eat)
        dog.breathe();     // Buddy is breathing... (父类方法，且为 final)
        
        // 只能通过强制类型转换来调用子类特有的方法
        if (dog instanceof Dog) {
            ((Dog) dog).bark();  // Buddy is barking...
        }
    }
}
```

**运行结果**（示例）：
```
Generic Animal is eating...
Generic Animal is breathing...
Buddy is eating dog food.
Buddy is breathing...
Buddy is barking...
```

---

## 七、总结

1. **继承（Inheritance）**
    - 使用 `extends` 建立父类与子类的层次关系，实现代码的复用与扩展。
    - Java 只支持**单继承**；所有类默认继承自 `Object`。

2. **extends 关键字**
    - 表示“子类 extends 父类”；子类继承父类的属性、方法。
    - 子类可以复写（override）父类方法，也可添加新特性。

3. **super 关键字**
    - 在子类中表示父类的引用，可访问父类的属性、方法或调用父类构造方法。
    - 在子类构造的第一行可使用 `super(...)` 来调用父类构造。

4. **方法重写（override）与方法重载（overload）**
    - **重写**：子类改写父类的同名同参数方法，体现**运行时多态**。
    - **重载**：同一类（或父子类）中方法名相同、参数列表不同，体现**编译时多态**。

5. **final 类和 final 方法**
    - `final` 类：不允许被继承。
    - `final` 方法：不允许被子类重写。

通过继承，可以减少代码的冗余度、提高开发效率和可维护性；合理运用 `super` 关键字和方法重写机制，可以使得子类对父类的功能进行拓展或定制化，同时保留父类原有的实现。  
在设计类层次结构时，应遵循**单一职责原则**、**开闭原则**等面向对象设计原则，避免出现过于复杂的继承关系。祝你在编程中熟练掌握继承的用法，写出结构清晰、易于扩展的面向对象代码！