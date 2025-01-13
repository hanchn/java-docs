在 Java 中，为了高效地进行多线程编程与并发控制，**`java.util.concurrent`** 包提供了**线程池**、**并发工具类**以及一些高水平的并发构造。而在 Java 集合框架与工具类方面，除了常见的 `List`, `Set`, `Map` 等容器外，还包含了 `Collections`, `Arrays`, `Objects` 等常用的辅助工具类。下面将依次进行介绍。

---

## 一、`java.util.concurrent` 包

### 1.1 线程池（Executor、ExecutorService、ThreadPoolExecutor）

1. **Executor / ExecutorService**
    - `Executor`：是一个执行任务的接口，只有一个 `execute(Runnable command)` 方法，用于提交任务。
    - `ExecutorService`：继承 `Executor`，提供了更丰富的功能，比如 `submit(Callable<T>)`、关闭线程池 (`shutdown()`)、任务执行结果管理 (`Future<?>`) 等。

2. **ThreadPoolExecutor**
    - 是一个可配置的线程池核心实现类。
    - 构造参数示例：
      ```java
      public ThreadPoolExecutor(
          int corePoolSize,          // 核心线程数
          int maximumPoolSize,       // 最大线程数
          long keepAliveTime,        // 空闲线程存活时间
          TimeUnit unit,             // 时间单位
          BlockingQueue<Runnable> workQueue,  // 任务队列
          ThreadFactory threadFactory,         // 线程工厂 (可选)
          RejectedExecutionHandler handler     // 拒绝策略 (可选)
      )
      ```
    - 工作原理：
        - 当提交任务时，如果当前运行的线程数少于 corePoolSize，则立即新建线程执行任务；
        - 若已达 corePoolSize，则将任务放入队列；
        - 若队列已满且线程数未达 maximumPoolSize，则继续创建线程；
        - 若线程数已达 maximumPoolSize 则执行**拒绝策略**。

3. **常用线程池工厂方法（Executors）**
    - `newFixedThreadPool(int nThreads)`：固定大小的线程池；
    - `newCachedThreadPool()`：可缓存的线程池，线程空闲超时会被回收；
    - `newSingleThreadExecutor()`：单线程池，任务串行执行；
    - `newScheduledThreadPool(int corePoolSize)`：支持定时调度的线程池。
    - **注意**：Java 8 之后，官方建议自定义 `ThreadPoolExecutor` 或使用更安全的方式配置参数，而不是直接使用上述快捷方法（以免资源耗尽或 OOM）。

4. **使用示例**
   ```java
   ExecutorService pool = Executors.newFixedThreadPool(4);
   for (int i = 0; i < 10; i++) {
       pool.execute(() -> {
           System.out.println(Thread.currentThread().getName() + " is running a task");
       });
   }
   pool.shutdown(); // 不再接收新任务
   ```

---

### 1.2 并发工具类

#### 1.2.1 `CountDownLatch`

1. **作用**
    - 倒计数锁存器：允许一个或多个线程等待**其他线程**完成某些操作后再继续执行。
    - 初始化时定义一个计数值 `count`，每当一个任务完成后，调用 `countDown()` 将计数减 1。当计数降为 0 时，等待的线程才会继续执行。

2. **示例**
   ```java
   int taskCount = 3;
   CountDownLatch latch = new CountDownLatch(taskCount);

   for (int i = 0; i < taskCount; i++) {
       new Thread(() -> {
           try {
               // 执行任务
           } finally {
               latch.countDown(); // 完成后减 1
           }
       }).start();
   }

   latch.await(); // 主线程阻塞，直到 countDownLatch 计数为 0
   System.out.println("All tasks done!");
   ```

#### 1.2.2 `CyclicBarrier`

1. **作用**
    - 让一组线程**相互等待**，直到到达某个屏障点（Barrier）后再一起继续运行。
    - 和 `CountDownLatch` 区别：`CyclicBarrier` 可在执行完成后**重用**（循环），而 `CountDownLatch` 一次性。

2. **示例**
   ```java
   int parties = 3;
   CyclicBarrier barrier = new CyclicBarrier(parties, () -> {
       System.out.println("All parties arrived, let's proceed.");
   });

   for (int i = 0; i < parties; i++) {
       new Thread(() -> {
           try {
               System.out.println(Thread.currentThread().getName() + " reached barrier.");
               barrier.await(); // 等待其它线程
           } catch (Exception e) {
               e.printStackTrace();
           }
       }).start();
   }
   ```

#### 1.2.3 `Semaphore`

1. **作用**
    - 信号量，用于**控制访问资源的线程数**；常见的场景如限流、限制某段代码最多只能有 N 个线程同时执行等。
    - `new Semaphore(int permits)` 表示可同时允许 permits 个线程访问。

2. **示例**
   ```java
   Semaphore semaphore = new Semaphore(3); // 最多允许 3 个线程同时执行
   for (int i = 0; i < 10; i++) {
       new Thread(() -> {
           try {
               semaphore.acquire(); // 获取一个许可
               // 执行关键操作
               System.out.println(Thread.currentThread().getName() + " acquired permit.");
           } catch (InterruptedException e) {
               e.printStackTrace();
           } finally {
               semaphore.release(); // 释放许可
           }
       }).start();
   }
   ```

#### 1.2.4 `Exchanger`

1. **作用**
    - 线程间交换数据的同步点，两个线程在交换点互相等待，对方到达后交换数据。
    - 常见应用场景：两个线程间配对（生产者-消费者、加密-解密等），互相交换数据。

2. **示例**
   ```java
   Exchanger<String> exchanger = new Exchanger<>();
   new Thread(() -> {
       String dataA = "Thread A Data";
       try {
           String received = exchanger.exchange(dataA);
           System.out.println("Thread A received: " + received);
       } catch (InterruptedException e) {
           e.printStackTrace();
       }
   }).start();

   new Thread(() -> {
       String dataB = "Thread B Data";
       try {
           String received = exchanger.exchange(dataB);
           System.out.println("Thread B received: " + received);
       } catch (InterruptedException e) {
           e.printStackTrace();
       }
   }).start();
   ```

#### 1.2.5 `ForkJoinPool`

1. **概念**
    - Java 7 引入的**分而治之**并行框架，适合**任务可拆分**并且**结果可合并**的场景（如递归、分块计算）。
    - 内部使用**工作窃取算法**提高线程利用率。

2. **使用示例**（简单求和任务）
   ```java
   ForkJoinPool forkJoinPool = new ForkJoinPool();
   long result = forkJoinPool.invoke(new SumTask(1, 1000000));
   System.out.println("Sum result: " + result);

   // 定义任务
   static class SumTask extends RecursiveTask<Long> {
       private long start, end;
       private static final long THRESHOLD = 10000;

       SumTask(long start, long end) {
           this.start = start;
           this.end = end;
       }

       @Override
       protected Long compute() {
           if (end - start <= THRESHOLD) {
               long sum = 0;
               for (long i = start; i <= end; i++) sum += i;
               return sum;
           } else {
               long mid = (start + end) / 2;
               SumTask left = new SumTask(start, mid);
               SumTask right = new SumTask(mid + 1, end);
               left.fork();
               long rightResult = right.compute();
               long leftResult = left.join();
               return leftResult + rightResult;
           }
       }
   }
   ```

---

## 二、集合与工具类

### 2.1 `Collections` 工具类

1. **作用**
    - 提供对集合（List、Set、Map）进行**排序**、**搜索**、**同步化**、**不可变化**等操作的工具方法。

2. **常用方法**
    - `Collections.sort(List<T>)`：对 List 进行排序；
    - `Collections.binarySearch(List<T>, key)`：在有序 List 中进行二分搜索；
    - `Collections.shuffle(List<T>)`：随机打乱顺序；
    - `Collections.unmodifiableList(...)`：返回不可修改的视图；
    - `Collections.synchronizedList(...)`：返回线程安全的包装。

3. **示例**
   ```java
   List<Integer> list = Arrays.asList(3, 1, 2);
   Collections.sort(list);
   System.out.println(list); // [1, 2, 3]

   int index = Collections.binarySearch(list, 2);
   System.out.println("Index of 2: " + index); // 1
   ```

### 2.2 `Arrays` 工具类

1. **作用**
    - 对**数组**进行**排序**、**搜索**、**填充**、**转换**等操作的工具方法。
    - 与 `Collections` 相似，但作用对象为**原生数组**（int[], String[] 等）。

2. **常用方法**
    - `Arrays.sort(int[] a)`：对数组进行排序；
    - `Arrays.binarySearch(int[] a, int key)`：二分搜索；
    - `Arrays.copyOf(...)` / `Arrays.copyOfRange(...)`：复制数组；
    - `Arrays.asList(T... a)`：把数组转换为一个**固定大小**的 `List`；
    - `Arrays.equals(...)`：比较数组内容是否相等等。

3. **示例**
   ```java
   int[] arr = {3, 1, 2};
   Arrays.sort(arr);
   System.out.println(Arrays.toString(arr)); // [1, 2, 3]

   System.out.println(Arrays.binarySearch(arr, 2)); // 返回索引 1
   ```

### 2.3 `Objects` 工具类

1. **作用**
    - 在 Java 7 引入，提供**空指针安全**的辅助方法、对象比较、哈希计算等。
    - 替代常见的**null 检查**、比较逻辑等。

2. **常用方法**
    - `Objects.requireNonNull(obj, message)`：若 `obj` 为 null 则抛出 `NullPointerException`；
    - `Objects.equals(a, b)`：安全比较，对 null 进行处理；
    - `Objects.hash(...)`：基于可变参数生成哈希值；
    - `Objects.toString(obj, defaultVal)`：对 obj 的字符串表示，为 null 时返回默认值。

3. **示例**
   ```java
   String str = null;
   try {
       Objects.requireNonNull(str, "String should not be null");
   } catch (NullPointerException e) {
       e.printStackTrace();
   }

   System.out.println(Objects.equals("abc", "abc")); // true
   System.out.println(Objects.hash("a", 123, true)); // 计算多字段哈希
   ```

---

## 三、总结

1. **`java.util.concurrent`**
    - 提供了**线程池**（`ExecutorService`, `ThreadPoolExecutor`, `ForkJoinPool`）与**并发工具类**（`CountDownLatch`, `CyclicBarrier`, `Semaphore`, `Exchanger`），
    - 帮助简化多线程编程，增强并发控制、任务调度和数据交换。

2. **集合工具类**
    - **`Collections`**：对 Java 集合进行**排序**、**查找**、**线程安全包装**、**不可变包装**等操作；
    - **`Arrays`**：专门针对数组的**排序**、**搜索**、**复制**等操作；
    - **`Objects`**：提供**空指针安全**的比较、哈希、字符串表示等辅助方法。

通过熟练掌握这些类与工具方法，能在**并发编程**与**集合操作**中写出简洁、安全、高效的代码。根据业务场景选择合适的并发工具（如`CountDownLatch`或`CyclicBarrier`），使用**线程池**管理线程资源，以及利用 `Collections` / `Arrays` / `Objects` 工具类提高编程效率与可读性。祝你在项目中充分运用这些特性，构建稳定、健壮的 Java 应用！