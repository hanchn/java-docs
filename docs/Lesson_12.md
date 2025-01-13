在 Java 面向对象编程（OOP）中，**抽象（Abstraction）**是将事物的**共性**或**本质特征**提取出来，以简化复杂度、突出关键点的过程。Java 中主要通过 **抽象类（abstract class）** 和 **接口（interface）** 来实现抽象的概念。它们各自适合不同的应用场景，都在一定程度上支持“设计层次的抽象”。

下面将详细介绍抽象类、抽象方法、接口（含默认方法和静态方法）的使用，以及它们之间的区别与应用场景。

---

## 一、抽象类（abstract class）

1. **定义**
   - 使用 `abstract` 关键字修饰的类即为**抽象类**；其中可以包含抽象方法（没有方法体）和非抽象方法（有具体实现）。
   - **示例**：
     ```java
     public abstract class Animal {
         // 普通成员变量
         protected String name;
         
         // 抽象方法：无方法体，只提供方法签名
         public abstract void makeSound();
         
         // 非抽象方法（具体方法）
         public void sleep() {
             System.out.println(name + " is sleeping...");
         }
     }
     ```

2. **抽象方法（abstract method）**
   - 被 `abstract` 关键字修饰，没有方法体，只有方法签名，强制子类去**实现**它。
   - 只能存在于抽象类或接口中，**不能**出现在普通类中。

3. **特点**
   - **无法被实例化**：不能使用 `new` 来创建抽象类的对象（`new Animal()` 会报错）。
   - **可包含成员变量、构造方法、普通方法、抽象方法**等，和普通类类似，只是**抽象方法**必须留给子类实现。
   - 通常用作**父类**，让子类**继承**并实现抽象方法。

4. **示例使用**
   ```java
   public abstract class Animal {
       protected String name;

       public Animal(String name) {
           this.name = name;
       }

       public abstract void makeSound();

       public void sleep() {
           System.out.println(name + " is sleeping...");
       }
   }

   // 子类必须实现抽象方法
   public class Dog extends Animal {
       public Dog(String name) {
           super(name);
       }

       @Override
       public void makeSound() {
           System.out.println(name + " barks!");
       }
   }

   // 测试
   public class MainApp {
       public static void main(String[] args) {
           // Animal a = new Animal("Generic"); // 错误：抽象类不能实例化
           Animal dog = new Dog("Buddy");
           dog.makeSound();  // Buddy barks!
           dog.sleep();      // Buddy is sleeping...
       }
   }
   ```

---

## 二、接口（interface）

1. **定义**
   - 接口是更“纯粹”的抽象概念，使用 `interface` 关键字定义，通常只包含**常量**和**抽象方法**。
   - 接口中的方法默认都是 `public abstract`，即使不写 `public`、`abstract`，编译器也会自动加上。
   - 接口的成员变量默认是 `public static final`（常量）。

2. **默认方法（default method）**
   - Java 8 引入了**默认方法**（使用 `default` 修饰），可以在接口中提供**方法的默认实现**，使得接口可以拥有部分实现逻辑。
   - 这样，即使在接口中添加新方法，也不会强制所有实现类立刻去实现该方法，从而保持向后兼容。
   - **示例**：
     ```java
     public interface Movable {
         void move();

         // 默认方法
         default void stop() {
             System.out.println("Stopped moving by default.");
         }
     }
     ```

3. **静态方法（static method）**
   - Java 8 还允许在接口中定义**静态方法**（使用 `static` 修饰），可以在接口内部提供工具方法或公共逻辑，供外部直接使用 `接口名.方法名()` 调用。
   - **示例**：
     ```java
     public interface Utility {
         static void printInfo(String info) {
             System.out.println("Info: " + info);
         }
     }
     ```

4. **实现接口**
   - 类通过 `implements` 关键字来**实现**接口，并**重写**接口中的抽象方法。
   - 可以实现**多个接口**（解决 Java 单继承的局限），接口与接口之间也可以多继承。
   - **示例**：
     ```java
     public interface Movable {
         void move();
         default void stop() {
             System.out.println("Default stopping...");
         }
     }

     public class Car implements Movable {
         @Override
         public void move() {
             System.out.println("Car is moving.");
         }

         // 可以选择重写默认方法
         @Override
         public void stop() {
             System.out.println("Car is stopping by braking.");
         }
     }
     ```

---

## 三、接口与抽象类的区别

1. **多继承与单继承**
   - **抽象类**只能通过 `extends` 关键字，被**单一**的子类继承；一个子类只能有一个直接父类（不管是否抽象）。
   - **接口**可以使用 `implements` 来**多实现**，一个类可以实现多个接口，接口也可以**多继承**其他接口。

2. **是否包含实现**
   - **抽象类**可以包含**抽象方法**和**已实现的方法**，也可以包含成员变量、静态代码块、构造器等。
   - **接口**在 Java 8 之前只能包含抽象方法和常量（以及 Java 8 之后的默认方法、静态方法），**没有构造方法**，也不可以包含普通成员变量（只能是常量）。

3. **成员的访问修饰**
   - **抽象类**可以有 `private`, `protected`, `public` 等修饰符的成员。
   - **接口**中所有成员默认就是 `public`，变量是 `public static final`，方法是 `public abstract`（默认方法或静态方法也是 `public`）。

4. **应用场景**
   - **抽象类**：用于表征一组**紧密相关**的类，体现一种**“is-a”** 的继承关系，可以有部分默认实现。一般当多个子类确实“类本质上相似”，并且需要在父类中提供一些默认属性或方法实现时使用抽象类。
   - **接口**：侧重定义功能**规范**或**契约**，可以被完全不同的类实现。更注重行为层面的抽象，不涉及具体属性。如果需要**多实现**，或只是定义一个能力或角色（如 `Comparable`, `Runnable`），就使用接口。

---

## 四、示例：接口 vs. 抽象类

```java
// 抽象类
public abstract class Animal {
    protected String name;
    
    public Animal(String name) {
        this.name = name;
    }

    public abstract void makeSound();

    public void sleep() {
        System.out.println(name + " is sleeping.");
    }
}

// 接口
public interface Movable {
    void move();

    default void stop() {
        System.out.println("Stopped moving by default.");
    }

    static void printInfo(String info) {
        System.out.println("Info: " + info);
    }
}

// 具体类继承抽象类、实现接口
public class Dog extends Animal implements Movable {
    public Dog(String name) {
        super(name);
    }

    @Override
    public void makeSound() {
        System.out.println(name + " barks.");
    }

    @Override
    public void move() {
        System.out.println(name + " is running.");
    }

    // 可以选择重写接口的默认方法 stop()
    // 也可以保留默认实现
    @Override
    public void stop() {
        System.out.println(name + " stops running.");
    }
}

// 测试
public class MainApp {
    public static void main(String[] args) {
        Dog dog = new Dog("Buddy");
        dog.makeSound();  // Buddy barks.
        dog.sleep();      // Buddy is sleeping.
        dog.move();       // Buddy is running.
        dog.stop();       // Buddy stops running.

        // 调用接口的静态方法
        Movable.printInfo("This is a static method in an interface.");
    }
}
```

**输出示例**：
```
Buddy barks.
Buddy is sleeping.
Buddy is running.
Buddy stops running.
Info: This is a static method in an interface.
```

---

## 五、总结

1. **抽象类（abstract class）**
   - 通过 `abstract` 修饰类和方法；可包含抽象方法和已实现的方法；
   - 不能被实例化；只允许**单继承**；
   - 适合描述有**相同本质**，但需要部分抽象的实体。

2. **接口（interface）**
   - 定义一组方法规范或能力；可包含抽象方法、默认方法（Java 8+）、静态方法；
   - 常量变量是 `public static final`，方法默认为 `public abstract`；
   - 类可实现多个接口，接口间也可多继承；
   - 适合描述“行为能力”或功能扩展，并支持多实现。

3. **接口与抽象类的区别及应用场景**
   - 抽象类：强调**“继承关系”**，并可以有构造方法、成员变量、部分实现；
   - 接口：强调**“功能规范”**，可以被多实现；更加灵活，但不能包含实例属性（只能是常量），也不能被实例化，没有构造方法；
   - 选择使用抽象类还是接口，主要看“类是否本质相同”以及“是否需要多实现某个功能契约”。

通过合理运用**抽象类**和**接口**，我们能设计出结构更清晰、可扩展性更好的系统。通常而言，如果两个类在本质上具有密切的类继承关系，并且需要提供部分默认实现，可选择**抽象类**；若只是要定义一组公共功能或行为规范，同时需要多实现的灵活性，则选择**接口**。祝你在实际开发中灵活运用抽象和接口特性，打造优雅的面向对象设计!