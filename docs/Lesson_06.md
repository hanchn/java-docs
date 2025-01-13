在 Java 中，**流程控制语句**用来控制程序的执行顺序和逻辑分支。根据不同的判断条件、循环结构和跳转方式，可以实现灵活多变的流程控制逻辑。下面将系统介绍 Java 的常见流程控制语句及其用法和注意事项。

---

## 一、分支结构

### 1.1 if-else

#### 1.1.1 基本语法

- **格式**：
  ```java
  if (条件表达式) {
      // 条件为 true 时执行的代码块
  } else {
      // 条件为 false 时执行的代码块
  }
  ```
- **示例**：
  ```java
  int score = 75;
  if (score >= 60) {
      System.out.println("及格");
  } else {
      System.out.println("不及格");
  }
  ```

#### 1.1.2 多重 if-else

- **格式**：
  ```java
  if (条件1) {
      // ...
  } else if (条件2) {
      // ...
  } else if (条件3) {
      // ...
  } else {
      // ...
  }
  ```
- **示例**：
  ```java
  if (score >= 90) {
      System.out.println("优秀");
  } else if (score >= 80) {
      System.out.println("良好");
  } else if (score >= 60) {
      System.out.println("及格");
  } else {
      System.out.println("不及格");
  }
  ```

#### 1.1.3 嵌套 if

- 可以在 if 块或 else 块中再嵌套 if 语句，注意提高代码可读性，避免嵌套层次过深。

---

### 1.2 switch-case

#### 1.2.1 基本语法

- **格式**：
  ```java
  switch (表达式) {
      case 常量1:
          // 匹配到表达式值 == 常量1 时执行
          break;
      case 常量2:
          // 匹配到表达式值 == 常量2 时执行
          break;
      ...
      default:
          // 如果所有 case 都没匹配到，则执行此处代码（可选）
  }
  ```
- **示例**：
  ```java
  int day = 3;
  switch (day) {
      case 1:
          System.out.println("Monday");
          break;
      case 2:
          System.out.println("Tuesday");
          break;
      case 3:
          System.out.println("Wednesday");
          break;
      default:
          System.out.println("Other day");
  }
  ```

#### 1.2.2 注意事项

1. **表达式类型**：Java 7 之前，`switch` 的表达式只支持 `byte`, `short`, `int`, `char` 以及它们对应的包装类。Java 7 开始还支持 `String` 类型。
2. **break 关键字**：如不写 `break`，则会出现**“穿透”（Fall-Through）**现象，执行完当前 case 代码后继续执行下一个 case。
3. **default 子句**：可选，但通常建议写上，用于处理**所有未匹配情况**。

---

## 二、循环结构

### 2.1 while 循环

#### 2.1.1 基本语法

- **格式**：
  ```java
  while (布尔表达式) {
      // 当布尔表达式为 true 时，重复执行的代码块
  }
  ```
- **示例**：
  ```java
  int i = 0;
  while (i < 5) {
      System.out.println("i = " + i);
      i++;
  }
  ```

#### 2.1.2 可能出现的问题

- 如果在循环体内未对循环条件进行更新或处理，则可能导致**死循环**。

---

### 2.2 do-while 循环

#### 2.2.1 基本语法

- **格式**：
  ```java
  do {
      // 先执行一次该代码块
  } while (布尔表达式);
  ```
- **示例**：
  ```java
  int i = 0;
  do {
      System.out.println("i = " + i);
      i++;
  } while (i < 5);
  ```

#### 2.2.2 特点

- 与 `while` 不同，`do-while` 无论条件如何，**至少执行一次**循环体。

---

### 2.3 for 循环

#### 2.3.1 基本语法

- **格式**：
  ```java
  for (初始化表达式; 布尔表达式; 更新表达式) {
      // 当布尔表达式为 true 时，重复执行的代码块
  }
  ```
- **示例**：
  ```java
  for (int i = 0; i < 5; i++) {
      System.out.println("i = " + i);
  }
  ```

#### 2.3.2 变量作用域

- 在 `for` 循环的初始化表达式中声明的变量（如 `int i = 0`），作用域仅在该 `for` 循环内部。

#### 2.3.3 省略写法

- 三个表达式都可以省略，但分号不能省略。例如：
  ```java
  int i = 0;
  for (; i < 5;) {
      System.out.println("i = " + i);
      i++;
  }
  ```

---

### 2.4 增强 for 循环（for-each）

#### 2.4.1 基本语法

- **格式**：
  ```java
  for (元素类型 变量名 : 数组或集合) {
      // 遍历当前元素
  }
  ```
- **示例（遍历数组）**：
  ```java
  int[] arr = {1, 2, 3, 4};
  for (int num : arr) {
      System.out.println(num);
  }
  ```
- **示例（遍历集合）**：
  ```java
  List<String> list = Arrays.asList("Apple", "Banana", "Orange");
  for (String item : list) {
      System.out.println(item);
  }
  ```

#### 2.4.2 特点

- 简化了常规 `for` 循环中下标操作，**无法**直接获得索引值，如果需要下标或遍历时需要修改元素值，建议使用普通 `for` 循环或迭代器。

---

## 三、循环控制关键字

### 3.1 break

- **含义**：结束当前循环或 `switch` 语句。
- **示例**：
  ```java
  for (int i = 0; i < 10; i++) {
      if (i == 5) {
          break; // 跳出循环
      }
      System.out.println("i = " + i);
  }
  // 输出： 0 1 2 3 4
  ```

### 3.2 continue

- **含义**：跳过本次循环，继续下一个循环迭代。
- **示例**：
  ```java
  for (int i = 0; i < 5; i++) {
      if (i == 2) {
          continue; // 跳过后续执行，进入下一个循环
      }
      System.out.println("i = " + i);
  }
  // 输出： 0 1 3 4
  ```

### 3.3 label（标签语句）

- **含义**：给代码块或循环指定一个标签（标识），通常与 `break` 或 `continue` 搭配，用于**跳出外层循环**或**继续外层循环**。
- **示例**：
  ```java
  outerLoop: for (int i = 0; i < 3; i++) {
      for (int j = 0; j < 3; j++) {
          if (j == 1) {
              break outerLoop; 
              // 直接跳出 outerLoop 这个标签对应的for循环
          }
          System.out.println("i = " + i + ", j = " + j);
      }
  }
  ```
   - 输出：
     ```
     i = 0, j = 0
     ```
     当 `j == 1` 时，执行 `break outerLoop;`，外层循环直接结束。

- 另一种用法是 `continue outerLoop;`，会跳到 outerLoop 的下一次循环迭代。

> **注意**：除非必须，否则不推荐使用标签跳转，会降低代码的可读性，应尽量使用更清晰的逻辑结构来避免嵌套和跳转。

---

## 四、综合示例

下面是一个小示例，演示了各种流程控制语句的综合应用：

```java
public class FlowControlDemo {
    public static void main(String[] args) {
        
        // 1. if-else
        int score = 85;
        if (score >= 90) {
            System.out.println("优秀");
        } else if (score >= 80) {
            System.out.println("良好");
        } else if (score >= 60) {
            System.out.println("及格");
        } else {
            System.out.println("不及格");
        }
        
        // 2. switch-case
        int day = 3;
        switch (day) {
            case 1:
                System.out.println("Monday");
                break;
            case 2:
                System.out.println("Tuesday");
                break;
            case 3:
                System.out.println("Wednesday");
                break;
            default:
                System.out.println("Other day");
        }
        
        // 3. while
        int i = 0;
        while (i < 3) {
            System.out.println("While loop i = " + i);
            i++;
        }
        
        // 4. do-while
        int j = 0;
        do {
            System.out.println("Do-while loop j = " + j);
            j++;
        } while (j < 3);
        
        // 5. for
        for (int k = 0; k < 3; k++) {
            System.out.println("For loop k = " + k);
        }
        
        // 6. 增强 for
        int[] arr = {1, 2, 3, 4};
        for (int num : arr) {
            System.out.println("num = " + num);
        }
        
        // 7. break, continue, label
        outer: for (int m = 0; m < 3; m++) {
            for (int n = 0; n < 3; n++) {
                if (n == 1) {
                    System.out.println("break outer at m = " + m + ", n = " + n);
                    break outer;  // 直接跳出outer循环
                }
                System.out.println("m = " + m + ", n = " + n);
            }
        }
    }
}
```

---

## 五、总结

1. **分支结构**
   - `if-else` 和 `switch-case`：根据条件对程序进行分支。
   - `if-else` 更灵活，适用于范围或复杂条件；`switch-case` 更适合**离散**的整数或字符串匹配。

2. **循环结构**
   - `while`：先判断条件，再执行循环体。
   - `do-while`：先执行一次循环体，再判断条件。
   - `for`：最常用的循环结构，语法简洁；更新表达式与循环条件容易管理。
   - **增强 for**：简化遍历数组或集合的操作，无法直接获取索引或修改集合结构。

3. **循环控制关键字**
   - `break`：结束当前循环或 switch。
   - `continue`：跳过当前一次循环。
   - `label + break/continue`：用于多重循环时，跳出或继续到外层循环，不常用但在一些复杂场景下提供了解决思路。

合理地选择和组合这些流程控制语句，可以编写出逻辑清晰、简洁明了的程序。应避免过深的嵌套和不必要的标签跳转，保持代码易读易维护。祝你在实际开发中能灵活运用，编写出流畅高效的 Java 程序。