在 Java 的面向对象编程（OOP）中，**封装（Encapsulation）**是非常重要的概念。它强调将**对象的属性和实现细节**隐藏起来，通过**公开的方法**来访问或修改对象的属性，从而实现数据的**安全性**和**可维护性**。本篇将介绍封装的概念、访问修饰符、get/set 方法以及 `this` 关键字的使用。

---

## 一、什么是封装

1. **定义**
   - 将类的**内部实现**（如属性、方法的实现细节）用访问修饰符隐藏起来，不对外暴露内部细节；
   - 对外只暴露必要的**公共方法（public method）**来**安全地**访问和修改对象的属性。

2. **目的**
   - **数据安全**：防止外部随意访问或修改内部数据。
   - **代码维护**：外部无需关注类的内部实现，通过公共方法（接口）与类进行交互，当内部实现改变时，外部接口不受影响。
   - **更好的复用性**：通过对外暴露有限的接口，让类本身可以独立演化或被重用。

---

## 二、访问修饰符

Java 中提供了四种主要的访问修饰符，来控制类、成员变量和方法的访问权限。

| 修饰符       | 同一个类中 | 同一个包中 | 子类（跨包） | 其他包中 |
|--------------|-----------|-----------|-------------|---------|
| **private**   | 可访问     | 不可访问   | 不可访问     | 不可访问 |
| **(默认)**（包级） | 可访问     | 可访问     | 不可访问     | 不可访问 |
| **protected** | 可访问     | 可访问     | 可访问       | 不可访问 |
| **public**    | 可访问     | 可访问     | 可访问       | 可访问   |

1. **private**
   - 只能在本类内部访问，最严格的访问级别，常用于类的**成员变量**或内部方法。
   - 能有效避免外部直接操作，必须通过公共方法访问。

2. **(默认)**（包级访问）
   - 若不写任何修饰符，则为**包级访问（default/package-private）**，只能在同一个包内被访问。

3. **protected**
   - 在同一个包中可以访问，在不同包的**子类**中也可访问，但其他包的非子类无法访问。
   - 常用于继承场景，子类可以访问父类中被 `protected` 修饰的属性或方法。

4. **public**
   - 最开放的访问级别，无论在什么包，都可以访问。
   - 通常类的构造方法、关键属性的 `getter/setter`、重要的业务方法等可以设置为 `public` 方便外部调用。

---

## 三、get/set 方法

1. **目的与作用**
   - 当成员变量被 `private` 修饰后，外部不能直接访问该变量，避免了不安全或非法的操作。
   - 若外部需要读取或修改这些属性，可以提供**公开的** `getter` / `setter` 方法，从而**控制**对属性的访问和赋值逻辑。

2. **命名规范**
   - `getter` 方法：`get` + 属性名首字母大写，如 `getName()`, `getAge()` 等；若是 `boolean` 属性，也常用 `isXxx()`。
   - `setter` 方法：`set` + 属性名首字母大写，如 `setName()`, `setAge()` 等。

3. **示例**
   ```java
   public class Person {
       private String name;   // private 修饰，外部无法直接访问
       private int age;       // private 修饰
       
       // getter
       public String getName() {
           return name;
       }
       
       // setter
       public void setName(String name) {
           // 在此可以加上必要的安全检查或数据验证
           this.name = name;
       }

       // getter
       public int getAge() {
           return age;
       }
       
       // setter
       public void setAge(int age) {
           // 例如：不允许年龄为负值等判断
           if (age < 0) {
               System.out.println("Age cannot be negative!");
               return;
           }
           this.age = age;
       }
   }
   ```

4. **优势**
   - 在 `setter` 中可以加入**数据校验**或其他逻辑，避免属性被赋值为非法数据。
   - 更好地**隔离**了外部和内部，实现可控的访问。

---

## 四、this 关键字

1. **概念**
   - `this` 代表**当前对象本身**，在类的内部使用。
   - 当方法或构造方法中的形参与成员变量同名时，可以用 `this` 来区分。

2. **主要用法**
   - **引用当前对象的成员变量**
     ```java
     public void setName(String name) {
         this.name = name;  // this.name 表示成员变量
     }
     ```
   - **在构造方法中调用本类的其他构造方法**
     ```java
     public Person(String name) {
         this(name, 0); // 调用 Person(String name, int age) 构造方法
     }
     
     public Person(String name, int age) {
         this.name = name;
         this.age = age;
     }
     ```
   - **返回当前对象**
      - 在某些流式调用的场景下，可以 `return this;` 实现链式操作。

3. **注意事项**
   - 只能在非静态方法或者构造方法中使用 `this`；静态方法中没有“当前对象”概念。
   - `this` 调用另一个构造方法时，必须是**构造方法的第一行语句**。

---

## 五、示例代码

以下是一个综合示例，展示了如何使用访问修饰符、getter/setter 方法以及 `this` 关键字来实现封装。

```java
public class Person {
    // 私有成员变量
    private String name;
    private int age;

    // 无参构造方法
    public Person() {
        // 可以做一些默认初始化
        System.out.println("调用了无参构造");
    }

    // 有参构造方法
    public Person(String name, int age) {
        this.name = name; // 用 this 区分成员变量和形参
        // 在 setAge 中可以对 age 做合法性检查
        setAge(age);
    }

    // Getter 方法
    public String getName() {
        return this.name;
    }

    // Setter 方法
    public void setName(String name) {
        this.name = name;
    }

    // Getter 方法
    public int getAge() {
        return this.age;
    }

    // Setter 方法（带有逻辑控制）
    public void setAge(int age) {
        if (age < 0) {
            System.out.println("年龄不能为负值，自动设置为 0");
            this.age = 0;
        } else {
            this.age = age;
        }
    }

    // 其他方法
    public void showInfo() {
        System.out.println("Name: " + this.name + ", Age: " + this.age);
    }
}

// 测试类
public class MainApp {
    public static void main(String[] args) {
        // 使用无参构造
        Person p1 = new Person();
        p1.setName("Alice");
        p1.setAge(20);
        p1.showInfo();  // Name: Alice, Age: 20

        // 使用有参构造
        Person p2 = new Person("Bob", -5); // 年龄传入-5，触发逻辑控制
        p2.showInfo();  // Name: Bob, Age: 0

        // 访问修饰符演示
        // p2.name = "Tom";  // Error: name 是 private，外部不可直接访问
        // 只能通过 setName 或构造方法赋值
        p2.setName("Tom");
        p2.setAge(25);
        p2.showInfo(); // Name: Tom, Age: 25
    }
}
```

**程序输出**：
```
调用了无参构造
Name: Alice, Age: 20
年龄不能为负值，自动设置为 0
Name: Bob, Age: 0
Name: Tom, Age: 25
```

---

## 六、总结

1. **封装（Encapsulation）**
   - 核心思想：通过访问修饰符隐藏内部实现，提供公共方法对外访问，保护数据安全。

2. **访问修饰符**
   - `private`：最严格，仅限当前类访问；常用于封装成员变量。
   - `(默认)`：包级访问；用于包内部共享。
   - `protected`：对子类和同包内可见；多用于继承场景。
   - `public`：完全开放访问；常用于对外提供的接口或方法。

3. **get/set 方法**
   - `getter` 读取属性，`setter` 修改属性；
   - 在 `setter` 中可以添加验证或逻辑控制；
   - 实现外部安全访问内部数据。

4. **this 关键字**
   - 代表当前对象本身；
   - 在方法或构造方法里区分同名变量；
   - 可以在构造方法内调用其他构造方法（必须在第一行）。

**封装**是面向对象编程的重要特征之一，通过合理使用访问修饰符、get/set 方法以及 `this` 关键字，可以保护内部数据的完整性，提升代码的可读性和可维护性。在后续的开发中，也会结合**继承（Inheritance）**、**多态（Polymorphism）**等概念，实现更强大、更灵活的面向对象设计。