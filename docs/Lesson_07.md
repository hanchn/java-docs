在 Java 中，**数组（Array）**是一种**定长**的、**同类型**数据的线性存储结构。它的优点是访问效率高（通过下标可以快速访问元素），缺点是长度固定，一旦创建后就不能动态改变大小。对于需要动态扩容的场景，则可以使用 **ArrayList** 等集合类。本篇将介绍 Java 中**一维数组、二维数组**以及**数组的初始化、遍历、复制**等常见操作，并简单比较 **Array** 与 **ArrayList** 的区别。

---

## 一、一维数组

### 1.1 声明与创建

1. **声明方式**
   ```java
   // 推荐写法
   int[] arr;
   
   // 旧写法（同样可行）
   int arr2[];
   ```
   在 Java 中，两种写法都可以声明数组，但推荐 `int[] arr;` 这种更清晰的方式。

2. **创建数组并分配空间**
   ```java
   // 创建长度为 5 的 int 型数组，元素默认值为 0
   arr = new int[5]; 
   ```
   创建数组时，系统会根据数组类型为每个元素赋默认值，例如：
   - 数值类型默认值为 0 或 0.0
   - `boolean` 默认值为 `false`
   - 引用类型（如 `String`）默认值为 `null`

3. **声明、创建、初始化一体化**
   ```java
   int[] arr3 = new int[5];   // 仅分配空间
   int[] arr4 = {1, 2, 3, 4}; // 静态初始化
   int[] arr5 = new int[]{10, 20, 30}; // 等效静态初始化
   ```

### 1.2 数组的长度

- 使用 `arrayName.length` 获取数组的长度。
  ```java
  int[] arr = {1, 2, 3};
  System.out.println(arr.length);  // 3
  ```

### 1.3 数组的遍历

- **for 循环**：
  ```java
  int[] arr = {1, 2, 3, 4, 5};
  for (int i = 0; i < arr.length; i++) {
      System.out.println("arr[" + i + "] = " + arr[i]);
  }
  ```
- **增强 for 循环**（for-each）：
  ```java
  for (int num : arr) {
      System.out.println(num);
  }
  ```

### 1.4 数组的复制

1. **使用 `System.arraycopy`**
   ```java
   int[] source = {1, 2, 3, 4};
   int[] dest = new int[6];
   System.arraycopy(source, 0, dest, 0, source.length);
   // 现在 dest 的前 4 个元素是 1,2,3,4，后两位是默认值 0
   ```
2. **使用 `Arrays.copyOf`**
   ```java
   int[] newArr = java.util.Arrays.copyOf(source, 10); 
   // 复制 source 数组到 newArr，newArr 长度为 10
   ```
3. **手动遍历复制**（不常用）
   ```java
   for (int i = 0; i < source.length; i++) {
       dest[i] = source[i];
   }
   ```

---

## 二、二维数组

在 Java 中，**二维数组**本质上是**数组的数组**。每个元素可以是一个一维数组，因此也可以是不规则的 “二维” 结构。

### 2.1 声明与创建

1. **标准二维数组**
   ```java
   // 声明
   int[][] matrix;
   
   // 创建 - 方式1：先指定行数和列数
   matrix = new int[3][4]; // 3 行，4 列
   
   // 创建 - 方式2：分配每一行的列数（不规则二维数组）
   int[][] irregular = new int[3][];  // 仅指定 3 行
   irregular[0] = new int[2];         // 第 1 行长度为 2
   irregular[1] = new int[3];         // 第 2 行长度为 3
   irregular[2] = new int[5];         // 第 3 行长度为 5
   ```
2. **静态初始化**
   ```java
   int[][] matrix2 = {
       {1, 2, 3},
       {4, 5, 6},
       {7, 8, 9}
   };
   ```

### 2.2 访问和修改二维数组元素

- **通过下标访问**
  ```java
  matrix[0][1] = 10; // 修改第一行第二列的值为 10
  int value = matrix[1][2]; // 读取第二行第三列的值
  ```
- 不规则二维数组需要注意每一行的长度不一定相同。可以用 `matrix[row].length` 获得第 `row` 行的列数。

### 2.3 遍历二维数组

- **双层 for 循环**：
  ```java
  for (int i = 0; i < matrix.length; i++) {
      for (int j = 0; j < matrix[i].length; j++) {
          System.out.print(matrix[i][j] + " ");
      }
      System.out.println();
  }
  ```
- **增强 for 循环**：
  ```java
  for (int[] row : matrix) {
      for (int element : row) {
          System.out.print(element + " ");
      }
      System.out.println();
  }
  ```

---

## 三、Array vs. ArrayList

在 Java 中，**数组**和 **ArrayList** 都可以用来存储一批元素，但它们有以下显著区别：

| 特性                             | Array                        | ArrayList                     |
|----------------------------------|------------------------------|--------------------------------|
| 长度是否可变                     | 固定长度，创建后不可变       | 动态可变，自动扩容            |
| 数据类型                        | 可以存储基本类型或引用类型   | 只能存储引用类型（泛型可指定包装类）  |
| 内存分配                        | 连续分配                     | 内部基于数组实现，自动管理扩容        |
| 访问效率（随机访问）             | O(1)                         | O(1)                           |
| 插入、删除（非末尾操作）         | O(n)                         | O(n)                           |
| 使用场景                        | 元素数量固定、性能要求高     | 元素数量不固定，需要增删操作  |
| 常用方法                        | 无特别的方法，需 `System.arraycopy` 等 | 提供 `add`, `remove`, `get`, `set` 等方法 |
| 是否支持泛型                    | 不支持泛型（需要自己定义类型）| 支持泛型 `ArrayList<T>`        |

### 3.1 使用 Array

- 当我们明确知道数据的**最大数量**或**固定大小**，而且主要是按照**下标索引**来操作时，原生数组可能更直接高效。

### 3.2 使用 ArrayList

- 在需要**灵活增删**元素、或不确定需要存储多少个元素时，更推荐使用 **ArrayList**。
- 例如：
  ```java
  List<String> list = new ArrayList<>();
  list.add("Apple");
  list.add("Banana");
  list.remove("Apple");
  for (String item : list) {
      System.out.println(item);
  }
  ```

---

## 四、代码示例

下面是一段综合示例，演示一维和二维数组的常见操作，以及与 `ArrayList` 简单对比。

```java
public class ArrayDemo {
    public static void main(String[] args) {
        // 1. 一维数组示例
        int[] nums = {10, 20, 30};
        System.out.println("nums length: " + nums.length);
        
        // 遍历数组
        for (int i = 0; i < nums.length; i++) {
            System.out.println("nums[" + i + "] = " + nums[i]);
        }
        
        // 2. 二维数组示例
        int[][] matrix = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };
        // 双层 for 循环遍历
        System.out.println("二维数组 matrix: ");
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                System.out.print(matrix[i][j] + " ");
            }
            System.out.println();
        }
        
        // 3. 数组复制
        int[] source = {1, 2, 3, 4};
        int[] dest = new int[6];
        System.arraycopy(source, 0, dest, 0, source.length);
        // 打印复制结果
        System.out.println("dest array after copy: ");
        for (int num : dest) {
            System.out.print(num + " ");
        }
        System.out.println();
        
        // 4. ArrayList 示例
        ArrayList<String> fruitList = new ArrayList<>();
        fruitList.add("Apple");
        fruitList.add("Banana");
        fruitList.add("Orange");
        System.out.println("Size of fruitList: " + fruitList.size());
        
        // 遍历 ArrayList
        for (String fruit : fruitList) {
            System.out.println(fruit);
        }
        
        // 动态添加/删除元素
        fruitList.remove("Banana");
        System.out.println("After remove: " + fruitList);
        
        // 与数组对比：数组长度固定
        // nums[3] = 40; // 会发生 ArrayIndexOutOfBoundsException
    }
}
```

**示例输出**（示例，仅供参考）：
```
nums length: 3
nums[0] = 10
nums[1] = 20
nums[2] = 30
二维数组 matrix: 
1 2 3 
4 5 6 
7 8 9 
dest array after copy: 
1 2 3 4 0 0 
Size of fruitList: 3
Apple
Banana
Orange
After remove: [Apple, Orange]
```

---

## 五、总结

1. **一维数组**
   - 声明时只需给定类型和变量名，创建时通过 `new` 分配空间。
   - 可静态初始化（直接写 `{}`）或动态初始化（`new int[]`）。
   - 访问元素通过下标，`array[i]`; 遍历可用 `for` 或增强 for。

2. **二维数组**
   - 本质是 “数组的数组”，可以是规则或不规则的二维结构。
   - 同样可以静态或动态初始化，访问方式 `matrix[row][col]`。
   - 遍历通常用**双层 for 循环**或**增强 for 循环**。

3. **数组复制**
   - `System.arraycopy` / `Arrays.copyOf` / 手动循环三种方式常用。
   - 注意数组越界问题，目标数组需要有足够的空间。

4. **Array vs. ArrayList**
   - **Array**：长度固定，存储效率高，适合已知大小及对随机访问需求高的场景。
   - **ArrayList**：动态扩容，提供丰富的操作方法，适合元素增删变化较多的场景，内部仍基于数组实现。

在实际开发中，根据项目需求选择合适的数据结构：如果需要**固定大小且访问高效**，选用原生**数组**；如果需要**动态调整大小**或操作便捷，则选用**ArrayList**或其他集合类。祝你在编程实践中熟练掌握并灵活运用数组及相关工具！