在 Java 中，**日期与时间处理**、**随机数生成**以及**正则表达式**等功能主要分布在 `java.util` 包（同时也包括一些在 `java.time` 包中）。本篇将从以下几个方面来介绍它们的关键类与用法：

1. **日期与时间**
    - `Date`、`Calendar`（传统日期类，较旧）
    - `LocalDate`、`LocalDateTime`、`DateTimeFormatter`（Java 8 引入的新时间 API）
2. **随机数**
    - `Random` 类
3. **格式化**
    - `Formatter`
4. **正则表达式**
    - `java.util.regex` 包
    - `Pattern`、`Matcher` 的使用

---

## 一、日期与时间

### 1.1 传统日期类：`Date` 与 `Calendar`

#### 1.1.1 `Date`

1. **定义**
    - `java.util.Date` 最初用于表示**特定的时间点**（精确到毫秒）。
    - 其一些方法（如 `getYear()`, `getMonth()` 等）已经废弃，通常配合 `DateFormat`/`SimpleDateFormat` 或 `Calendar` 使用。

2. **常见操作**
   ```java
   Date now = new Date();          // 获取当前时间
   long timestamp = now.getTime(); // 获取自1970-01-01起的毫秒数
   System.out.println(now);        // 输出当前时间的字符串表示
   ```

#### 1.1.2 `Calendar`

1. **定义**
    - `Calendar` 是一个抽象类，常用实现是 `GregorianCalendar`；与 `Date` 一起使用，用于更灵活地操作日期和时间组件（年、月、日、时、分、秒等）。

2. **常见操作**
   ```java
   Calendar cal = Calendar.getInstance(); // 默认为当前系统时区当前时间
   int year = cal.get(Calendar.YEAR);
   int month = cal.get(Calendar.MONTH) + 1; // 月份从0开始
   int day = cal.get(Calendar.DAY_OF_MONTH);
   cal.add(Calendar.DAY_OF_MONTH, 7);       // 日期加 7 天
   Date date = cal.getTime();              // 转成 Date 对象
   ```

3. **不足**
    - `Calendar` API 相对繁琐，线程不安全。自 Java 8 引入全新的 `java.time` 包后，建议使用新的日期时间类。

---

### 1.2 Java 8 新时间 API（`java.time` 包）

Java 8 引入了全新的日期时间库，主要位于 `java.time` 包下，常见类包括 `LocalDate`, `LocalDateTime`, `LocalTime`, `Instant`, `ZonedDateTime` 等，用于更好地处理日期时间问题。

#### 1.2.1 `LocalDate` 与 `LocalDateTime`

1. **`LocalDate`**
    - 只包含**日期**部分（年、月、日），不包含时间信息，也**无时区**概念。
    - 常用方法：
      ```java
      LocalDate now = LocalDate.now();            // 当前日期
      LocalDate specificDate = LocalDate.of(2025, 1, 13); // 指定日期
      int year = now.getYear();
      int month = now.getMonthValue();
      int day = now.getDayOfMonth();
      LocalDate tomorrow = now.plusDays(1);       // 加一天
      ```
2. **`LocalDateTime`**
    - 同时包含**日期和时间**（年、月、日、时、分、秒、纳秒），无时区。
    - 常用方法：
      ```java
      LocalDateTime now = LocalDateTime.now();
      LocalDateTime specificTime = LocalDateTime.of(2025, 1, 13, 10, 30, 0);
      LocalDateTime nextHour = now.plusHours(1);
      ```

#### 1.2.2 `DateTimeFormatter`

1. **用途**
    - 用于**格式化**或**解析**日期时间对象，代替旧的 `SimpleDateFormat`，且**线程安全**。

2. **示例**

   ```java
   // 格式化
   LocalDateTime now = LocalDateTime.now();
   DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
   String formatted = now.format(formatter);
   System.out.println(formatted);  // e.g. 2025-01-13 10:30:00

   // 解析
   String dateStr = "2025-01-13 10:30:00";
   LocalDateTime parsedDateTime = LocalDateTime.parse(dateStr, formatter);
   System.out.println(parsedDateTime); // 2025-01-13T10:30
   ```

---

## 二、随机数：`Random`

1. **`Random` 类**
    - 位于 `java.util` 包，用于生成**伪随机数**。
    - 常见方法：
      ```java
      Random rand = new Random();             // 使用默认种子（当前时间）
      int rInt = rand.nextInt();             // 生成任意int整数
      int rBound = rand.nextInt(10);         // [0, 10) 范围
      double rDouble = rand.nextDouble();    // [0, 1) 范围
      boolean rBoolean = rand.nextBoolean(); // true或false
      ```
2. **种子**
    - `new Random(long seed)` 可以指定**种子**来生成可重现的随机序列。
    - 如果不显式指定，默认使用**系统时间**作为种子。

3. **`ThreadLocalRandom`**（补充）
    - 在并发环境下，使用 `ThreadLocalRandom` 可减少竞争冲突、提升效率。
    - 用法：
      ```java
      int value = ThreadLocalRandom.current().nextInt(10);
      ```

---

## 三、格式化：`Formatter`

1. **`Formatter` 类**
    - 提供类似 C 语言 `printf` 的格式化输出功能。
    - 在 `System.out.printf()` 中就使用了内部的 `Formatter`。

2. **示例**
   ```java
   // 1. 使用 System.out.printf()
   System.out.printf("Hello, %s! You are %d years old.\n", "Alice", 20);

   // 2. 直接使用 Formatter
   Formatter formatter = new Formatter();
   formatter.format("Name: %s, Score: %.2f", "Bob", 95.678);
   String result = formatter.toString();
   formatter.close();   // 需手动关闭
   System.out.println(result);   // Name: Bob, Score: 95.68
   ```

3. **常用占位符**
    - `%s`：字符串
    - `%d`：整数
    - `%f`：浮点数，常配合精度指定
    - `%tF`：日期输出为 `yyyy-MM-dd` 等（也可用新时间 API 的 `DateTimeFormatter` 进行更多自定义）。

---

## 四、正则表达式：`java.util.regex`

### 4.1 基础概念

1. **正则表达式（Regular Expression）**
    - 一种匹配字符串的**模式**，用于字符串**搜索**、**替换**、**验证**等。
    - 具有丰富的语法，如 `.`、`*`、`+`、`?`、`[]`、`()`、`|` 等。

2. **常见示例**
    - 验证邮箱：`^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$`
    - 匹配数字：`^\d+$`（只包含数字）

### 4.2 `Pattern` 与 `Matcher`

1. **`Pattern`**
    - 表示**编译后的正则表达式**对象；
    - 使用 `Pattern.compile(String regex)` 创建。

2. **`Matcher`**
    - 用于**匹配**和**操作**文本，依赖一个 `Pattern` 对象和输入文本；
    - 获取方式：`Pattern pattern = Pattern.compile("regex"); Matcher matcher = pattern.matcher("input");`

3. **常用方法**
    - `matches()`：是否整个文本都匹配；
    - `find()`：在文本中**查找下一个**匹配子串；
    - `group()`：返回上一次匹配到的子串；
    - `replaceAll(String replacement)`：替换所有匹配；也可直接使用 `String` 类的 `replaceAll(regex, replacement)`。

4. **示例**
   ```java
   String text = "My phone is 1234567890, call me at 0987654321 too!";
   Pattern pattern = Pattern.compile("\\d{10}"); // 匹配10位数字
   Matcher matcher = pattern.matcher(text);

   while (matcher.find()) {
       System.out.println("Found phone number: " + matcher.group());
   }

   // 输出：
   // Found phone number: 1234567890
   // Found phone number: 0987654321
   ```

---

## 五、综合示例

下面是一段简单的综合示例，展示了日期时间操作、随机数、格式化输出、以及正则匹配。

```java
import java.util.*;
import java.time.*;
import java.time.format.*;
import java.util.regex.*;

public class UtilDemo {
    public static void main(String[] args) {
        // 1. 日期时间（Java 8）
        LocalDate today = LocalDate.now();
        LocalDateTime now = LocalDateTime.now();
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        System.out.println("Current Date: " + today);
        System.out.println("Current DateTime: " + now.format(formatter));

        // 2. 随机数
        Random rand = new Random();
        int randomInt = rand.nextInt(100); // 0~99
        double randomDouble = rand.nextDouble(); // 0~1
        System.out.println("Random int <100: " + randomInt);
        System.out.println("Random double <1: " + randomDouble);

        // 3. Formatter
        String name = "Alice";
        int age = 25;
        Formatter fmt = new Formatter();
        fmt.format("Name: %s, Age: %d", name, age);
        System.out.println(fmt.toString());
        fmt.close();

        // 4. 正则表达式
        String text = "Emails: alice@example.com, bob@test.org";
        String emailRegex = "[\\w.%+-]+@[\\w.-]+\\.[A-Za-z]{2,}";
        Pattern pattern = Pattern.compile(emailRegex);
        Matcher matcher = pattern.matcher(text);
        while (matcher.find()) {
            System.out.println("Found email: " + matcher.group());
        }
    }
}
```

**示例输出**（示例，仅供参考）：
```
Current Date: 2025-01-13
Current DateTime: 2025-01-13 10:30:45
Random int <100: 42
Random double <1: 0.7123456789
Name: Alice, Age: 25
Found email: alice@example.com
Found email: bob@test.org
```

---

## 六、总结

1. **日期与时间**
    - **旧版**：`Date` + `Calendar`；
    - **新版**：`LocalDate`, `LocalDateTime`, `DateTimeFormatter` 等（推荐使用），线程安全且 API 更友好。

2. **随机数**
    - `Random`：常用随机数生成；
    - `ThreadLocalRandom`：多线程环境下优先使用。

3. **格式化**
    - `Formatter`/`String.format()`/`System.out.printf()`：支持类似 C `printf` 的格式输出。

4. **正则表达式**
    - `java.util.regex` 包提供 `Pattern`（编译正则）与 `Matcher`（执行匹配），用来进行字符串的查找与处理。

使用好 `java.util` 包及 Java 8 的新日期时间 API 能让我们在开发时更高效地处理日期、时间、随机数以及字符串的复杂操作。多加练习和实践，才能熟练掌握这些类提供的丰富功能。祝你编码愉快！