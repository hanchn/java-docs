在 Java 中，**内部类（Inner Class）** 是定义在另一个类的内部或方法中的类。内部类通常用来对外隐藏实现细节、逻辑封装或者方便访问外部类的成员。Java 提供了多种形式的内部类，主要包括：**成员内部类**、**静态内部类**、**局部内部类**和**匿名内部类**。下面将一一介绍它们的定义、特性以及常见的应用场景。

---

## 一、成员内部类（Non-Static Inner Class）

1. **定义**
    - 成员内部类是**定义在外部类的成员位置**（与成员变量、成员方法并列）的一个类，不带 `static` 关键字。
    - 也被称为**非静态内部类**，一般会持有一个对外部类对象的引用，允许直接访问外部类的实例成员。

2. **示例**

   ```java
   public class Outer {
       private String outerField = "Outer Field";

       // 成员内部类
       public class Inner {
           public void innerMethod() {
               System.out.println("Accessing: " + outerField);
               // 直接访问外部类的实例字段
           }
       }

       public void outerMethod() {
           // 创建内部类对象
           Inner inner = new Inner();
           inner.innerMethod();
       }
   }

   public class MainApp {
       public static void main(String[] args) {
           // 必须先有外部类对象，才能创建成员内部类对象
           Outer outer = new Outer();
           // 1. 通过外部类的方法间接使用内部类
           outer.outerMethod();

           // 2. 直接创建内部类对象
           Outer.Inner innerObj = outer.new Inner();
           innerObj.innerMethod();
       }
   }
   ```
   **输出示例：**
   ```
   Accessing: Outer Field
   Accessing: Outer Field
   ```

3. **特点**
    - **依赖外部类实例**：要先创建外部类对象，再通过“外部类对象的引用”来创建内部类对象。
    - 可以**直接访问外部类的实例成员**（包括 `private`）。
    - 内部类中**不能定义任何静态成员**（除非是常量 `final static`），因为它属于外部类的实例层级。

---

## 二、静态内部类（Static Inner Class）

1. **定义**
    - 使用 `static` 修饰的内部类，称为**静态内部类**。它**不依赖**外部类的对象实例，但依赖外部类的**类**本身。

2. **示例**

   ```java
   public class Outer {
       private static String staticField = "Static Field";
       private String outerField = "Instance Field";

       // 静态内部类
       public static class Inner {
           public void innerMethod() {
               // 可以直接访问外部类的静态成员
               System.out.println("Access static field: " + staticField);
               // System.out.println("Access instance field: " + outerField); // Error：无法直接访问非静态成员
           }
       }
   }

   public class MainApp {
       public static void main(String[] args) {
           // 静态内部类不需要外部类对象
           Outer.Inner innerObj = new Outer.Inner();
           innerObj.innerMethod();
       }
   }
   ```
   **输出示例：**
   ```
   Access static field: Static Field
   ```

3. **特点**
    - 与外部类**实例**没有绑定关系，可以直接使用 `new Outer.Inner()` 创建对象。
    - 只能**直接访问外部类的静态成员**；如果要访问外部类的实例成员，需要显式传递外部类对象的引用。
    - 常用于将某些与外部类紧密关联，但**不需要访问外部类实例**的功能逻辑进行封装。

---

## 三、局部内部类（Local Inner Class）

1. **定义**
    - 在**方法**或**代码块**内部定义的类，作用域只在该方法或代码块中。
    - 也可看作是“方法体中的内部类”，一般不常见，但可用于对特定逻辑进行简洁的封装。

2. **示例**

   ```java
   public class Outer {
       public void outerMethod() {
           final String localVar = "Local Variable";

           // 局部内部类
           class LocalInner {
               public void showVar() {
                   // Java 8 开始，局部变量只需事实上的 final (effectively final) 即可
                   System.out.println(localVar);
               }
           }

           LocalInner li = new LocalInner();
           li.showVar();
       }
   }

   public class MainApp {
       public static void main(String[] args) {
           Outer outer = new Outer();
           outer.outerMethod();
       }
   }
   ```
   **输出示例：**
   ```
   Local Variable
   ```

3. **特点**
    - **作用域有限**：只能在定义它的方法或代码块中使用。
    - 可以访问该方法中的**局部变量**（要求该变量是“effectively final”，即不会在后续修改）。
    - 常在内部类只对某个方法有用时使用，逻辑更加内聚。

---

## 四、匿名内部类（Anonymous Inner Class）

1. **定义**
    - 匿名内部类是**没有类名**的内部类，通常在**实现接口**或**继承抽象类**的场景下直接“new”出来，用来快速覆盖方法实现。
    - 语法格式：
      ```java
      new 接口名或父类类型() {
          // 覆盖或实现抽象方法
      }
      ```
2. **示例（实现接口）**

   ```java
   public interface Greeting {
       void sayHello();
   }

   public class MainApp {
       public static void main(String[] args) {
           // 匿名内部类实现接口
           Greeting greet = new Greeting() {
               @Override
               public void sayHello() {
                   System.out.println("Hello from Anonymous Inner Class!");
               }
           };
           greet.sayHello();
       }
   }
   ```
   **输出示例：**
   ```
   Hello from Anonymous Inner Class!
   ```

3. **特点**
    - **一次性使用**：常用于快速创建接口或抽象类的实例，不用专门定义一个子类。
    - **不能重复实例化**：匿名内部类没有类名，如果想再次使用相同实现逻辑，需要再写一次 “new ...”。
    - 代码简洁，但可读性有时较差。

4. **常见用途**
    - 为回调（Callback）或事件监听注册一个短小的实现。
    - 线程或定时任务中，直接创建 `Runnable` 的匿名实现。

---

## 五、应用场景

1. **成员内部类**
    - 当内部类需要**频繁访问外部类的实例成员**，且想保持和外部类的强耦合关系时使用。
    - 典型例子：**迭代器模式**中，外部类是集合，内部类是迭代器，需要访问集合的私有数据结构。

2. **静态内部类**
    - 与外部类实例关联不大，但逻辑上又强依赖外部类时，可放在外部类内作为静态内部类。
    - 例如：外部类是工具类，静态内部类是和工具方法紧密相关的辅助类，但不需要访问外部类的实例成员。

3. **局部内部类**
    - 仅在某个**方法或代码块**内使用，用于临时封装一些逻辑或行为，使代码更有层次，避免公共类污染。
    - 通常在**方法内部**处理更加“私有化”的业务时使用。

4. **匿名内部类**
    - 常用于**回调**、**事件处理**、**策略实现**等需要**快速定义一个一次性实现**的场合。
    - 让代码更加精简，免去定义一个单独的实现类或子类文件。

---

## 六、总结

1. **成员内部类**（非静态）：
    - 挂靠在外部类对象上；可以直接访问外部类实例成员；创建时需依赖外部类的实例。

2. **静态内部类**：
    - `static` 修饰，与外部类实例无关；只可访问外部类的静态成员。

3. **局部内部类**（Local Inner Class）：
    - 定义在方法或代码块内部，作用域仅限于该方法或代码块中；可访问局部变量（要求 effectively final）。

4. **匿名内部类**（Anonymous Inner Class）：
    - 没有类名，用于实现接口或继承父类的一次性实现；代码简洁、灵活，用于回调或事件处理等场景。

**应用场景**主要基于对“内部类与外部类关系”的需求、对“代码可读性和封装”的考虑，以及是否需要“一次性实现”或“多次使用”来选择具体的内部类形式。合理使用内部类可以使代码更加**紧凑**、**内聚**，同时保护内部实现细节，提升可读性与可维护性。祝你在实际项目中灵活选择合适的内部类类型，实现高质量的 Java 代码。