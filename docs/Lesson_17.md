在 Java 中，输入输出（I/O）操作主要分布在 **`java.io`** 包与 **`java.nio`**（New I/O）包下。
- **`java.io`** 提供了传统的 **流式 I/O**，包括字节流与字符流，以及常用的文件操作类。
- **`java.nio`**（从 Java 1.4 开始引入）提供了基于 **缓冲区（Buffer）** 与 **通道（Channel）** 的 I/O，支持**非阻塞 I/O**（NIO）。  
  下面将介绍它们的关键类、用法以及一些常见的应用场景。

---

## 一、`java.io` 包

### 1.1 字节流：`InputStream`、`OutputStream`

1. **概念**
    - 针对**二进制数据**（如图片、视频、音频等）的读写。
    - **InputStream**：读取数据的字节输入流；
    - **OutputStream**：写出数据的字节输出流。

2. **常见子类**
    - `FileInputStream` / `FileOutputStream`：与文件进行字节流操作；
    - `BufferedInputStream` / `BufferedOutputStream`：带缓冲功能的字节流，能提高读写效率；
    - `ByteArrayInputStream` / `ByteArrayOutputStream`：在内存中的字节数组上进行操作。

3. **示例**：从文件中读取字节流并写出到另一个文件。
   ```java
   try (FileInputStream fis = new FileInputStream("input.jpg");
        FileOutputStream fos = new FileOutputStream("output.jpg")) {
       
       byte[] buffer = new byte[1024];
       int len;
       while ((len = fis.read(buffer)) != -1) {
           fos.write(buffer, 0, len);
       }
   } catch (IOException e) {
       e.printStackTrace();
   }
   ```
    - 使用 `try-with-resources` 确保流在使用完后自动关闭。

### 1.2 字符流：`Reader`、`Writer`

1. **概念**
    - 针对**字符数据**（文本文件、字符串）的读写。
    - **Reader**：字符输入流；
    - **Writer**：字符输出流。

2. **常见子类**
    - `FileReader` / `FileWriter`：文件的字符读写；
    - `BufferedReader` / `BufferedWriter`：带缓冲功能的字符流；
    - `InputStreamReader` / `OutputStreamWriter`：字节流与字符流之间的桥梁，常用于指定字符编码。

3. **示例**：按行读取文本文件
   ```java
   try (BufferedReader br = new BufferedReader(new FileReader("test.txt"))) {
       String line;
       while ((line = br.readLine()) != null) {
           System.out.println(line);
       }
   } catch (IOException e) {
       e.printStackTrace();
   }
   ```

### 1.3 文件操作：`File`、`FileInputStream`、`FileOutputStream` 等

1. **`File` 类**
    - 表示**文件或目录**路径抽象，而非对文件本身的读写；
    - 提供诸如 `createNewFile()`, `delete()`, `mkdir()`, `listFiles()` 等方法。

2. **`FileInputStream` / `FileOutputStream`**
    - 继承自字节流 `InputStream` / `OutputStream`；
    - 用于对文件进行**字节级**的读写操作；
    - 与 `FileReader` / `FileWriter` 相比，前者适用于**二进制数据**，后者适用于**文本数据**。

3. **示例**：简单文件信息获取
   ```java
   File file = new File("example.txt");
   if (file.exists()) {
       System.out.println("File Size: " + file.length());
       System.out.println("Absolute Path: " + file.getAbsolutePath());
   } else {
       System.out.println("File does not exist.");
   }
   ```

### 1.4 缓冲流：`BufferedInputStream`、`BufferedReader` 等

1. **作用**
    - 提高 IO 效率。缓冲流会在内部分配一个缓冲区（如 8KB），将底层数据分块读入或写出，减少实际的磁盘或网络访问次数。

2. **常用缓冲流**
    - `BufferedInputStream` / `BufferedOutputStream`：针对字节流；
    - `BufferedReader` / `BufferedWriter`：针对字符流；支持 `readLine()` / `newLine()` 方法。

3. **示例**
   ```java
   try (BufferedInputStream bis = new BufferedInputStream(new FileInputStream("input.bin"));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("output.bin"))) {
       
       byte[] buffer = new byte[4096];
       int len;
       while ((len = bis.read(buffer)) != -1) {
           bos.write(buffer, 0, len);
       }
   }
   ```

---

## 二、`java.nio` 包（New I/O）

Java NIO 提供了与传统 IO 不同的编程模型，基于**缓冲区（Buffer）** 与 **通道（Channel）**，还支持**非阻塞 I/O** 与 **内存映射文件**等特性。

### 2.1 缓冲区（Buffer）

1. **概念**
    - `Buffer` 是一个容器对象，可包含特定类型的**连续数据**（如 `ByteBuffer`, `CharBuffer`, `IntBuffer` 等）；
    - 通过**读写指针**（position、limit、capacity 等）来管理数据。

2. **常见操作**
    - `allocate(int capacity)`：分配一个新的缓冲区；
    - `put(...)`：往缓冲区写数据；
    - `flip()`：切换为读模式（limit=position, position=0）；
    - `get()`：从缓冲区读取数据；
    - `clear()` 或 `compact()`：为下一次写入做准备等。

3. **示例**：使用 `ByteBuffer`
   ```java
   ByteBuffer buffer = ByteBuffer.allocate(1024);
   buffer.put((byte) 65);
   buffer.put((byte) 66);
   
   buffer.flip();  // 切换到读模式
   
   while (buffer.hasRemaining()) {
       System.out.println(buffer.get());
   }
   ```

### 2.2 通道（Channel）

1. **概念**
    - `Channel` 类似于传统 IO 中的“流”，但 **Channel** 是双向的，可读可写；
    - 常用的 Channel 实现包括 `FileChannel`, `SocketChannel`, `ServerSocketChannel`, `DatagramChannel` 等。

2. **FileChannel**
    - 通过 `FileInputStream` 或 `FileOutputStream` 或 `RandomAccessFile` 的 `getChannel()` 方法获取 `FileChannel`；
    - 可使用**内存映射文件**（`map()` 方法）或常规的 `read()` / `write()` 与 Buffer 进行数据交换。

3. **示例**：复制文件
   ```java
   try (FileChannel inChannel = new FileInputStream("source.txt").getChannel();
        FileChannel outChannel = new FileOutputStream("dest.txt").getChannel()) {
       
       ByteBuffer buffer = ByteBuffer.allocate(1024);
       while (inChannel.read(buffer) != -1) {
           buffer.flip();   // 切换读模式
           outChannel.write(buffer);
           buffer.clear();  // 切换写模式
       }
   } catch (IOException e) {
       e.printStackTrace();
   }
   ```

### 2.3 文件锁（FileLock）

1. **用途**
    - 在多进程或多线程环境下，对文件进行**锁定**，防止并发冲突或数据损坏；
    - 通过 `FileChannel` 的 `lock()` 或 `tryLock()` 方法获取 `FileLock` 对象。

2. **示例**
   ```java
   try (RandomAccessFile raf = new RandomAccessFile("test.lock", "rw");
        FileChannel channel = raf.getChannel()) {
       FileLock lock = channel.lock(); // 独占锁，阻塞式
       // do something with the file
       lock.release(); // 释放锁
   } catch (IOException e) {
       e.printStackTrace();
   }
   ```

### 2.4 内存映射文件（MappedByteBuffer）

1. **概念**
    - `FileChannel` 提供 `map()` 方法，可将文件的部分或全部内容**映射到内存**，从而直接在内存缓冲区操作。
    - 适合处理**大文件**或**随机访问**场景。

2. **示例**
   ```java
   try (RandomAccessFile raf = new RandomAccessFile("data.dat", "rw");
        FileChannel channel = raf.getChannel()) {
       
       // 将文件的前 100 字节映射到内存
       MappedByteBuffer mbb = channel.map(FileChannel.MapMode.READ_WRITE, 0, 100);
       
       // 读写操作就像操作 ByteBuffer
       mbb.put(0, (byte) 65);  // 修改第0字节
       System.out.println((char) mbb.get(0));
   } catch (IOException e) {
       e.printStackTrace();
   }
   ```
    - 这种方式可以**提升 I/O 性能**，尤其是在对文件的某一部分进行频繁访问时（操作系统底层会做内存映射管理）。

---

## 三、总结与对比

1. **`java.io` 流式 I/O**
    - **单向流**模型：输入流、输出流；
    - 操作**面向流**，一次处理一个字节或字符；
    - 适合**阻塞式**的 I/O 场景，代码简单易懂；
    - 如果需要提高效率，可加**缓冲流**（`BufferedInputStream`, `BufferedReader` 等）。

2. **`java.nio` 基于 Channel & Buffer**
    - **双向通道**，读写可以并行进行；
    - 提供**非阻塞 I/O**（Selector 机制）与**内存映射文件**等高级特性；
    - 适合**高并发**、**大数据量**场景；
    - 编程方式与传统 IO 不同，需要掌握缓冲区的 position、limit 等概念。

3. **文件锁与内存映射**（NIO 独有）
    - `FileLock`：在多进程 / 多线程环境下进行文件访问控制；
    - `MappedByteBuffer`：将文件映射到内存，提升对大文件或随机访问场景下的读写效率。

---

## 四、代码示例：NIO vs. IO

下面简单演示如何用传统 IO 与 NIO 来复制文件，感受两种方式的差异：

### 4.1 传统 IO

```java
public static void copyFileIO(String src, String dest) throws IOException {
    try (FileInputStream fis = new FileInputStream(src);
         FileOutputStream fos = new FileOutputStream(dest)) {
        byte[] buffer = new byte[8192];
        int len;
        while ((len = fis.read(buffer)) != -1) {
            fos.write(buffer, 0, len);
        }
    }
}
```

### 4.2 NIO

```java
public static void copyFileNIO(String src, String dest) throws IOException {
    try (FileChannel inChannel = new FileInputStream(src).getChannel();
         FileChannel outChannel = new FileOutputStream(dest).getChannel()) {

        ByteBuffer buffer = ByteBuffer.allocateDirect(8192); 
        // allocateDirect 在物理内存分配, 可能更快

        while (inChannel.read(buffer) != -1) {
            buffer.flip();           // 切换读模式
            outChannel.write(buffer);
            buffer.clear();          // 切换写模式
        }
    }
}
```

两者在使用方式上明显不同：
- 传统 I/O 面向流（调用 `read()` / `write()` 将数据拷贝到 / 从 `byte[]` 中）；
- NIO 面向通道（`Channel`）与缓冲区（`Buffer`），需要 `flip()`, `clear()` 等操作控制指针。

---

## 五、应用场景建议

1. **小规模、简单场景**
    - 可以用**传统 IO**，如 `FileReader/FileWriter`, `FileInputStream/FileOutputStream` + 适当的缓冲流即可。

2. **大文件处理或高并发**
    - 可以使用**NIO**，尤其是**内存映射**、**非阻塞**等特性，提升效率。

3. **网络通信**
    - **NIO** 提供 `SocketChannel`, `ServerSocketChannel`，配合 `Selector` 实现**多路复用**，适合服务器端高并发模型。

4. **多进程文件访问**
    - 考虑使用 NIO 的**文件锁**（`FileLock`）进行并发访问控制。

---

## 六、结语

- **`java.io` 包**：基于**流**的阻塞式 I/O，适合大多数简单读写场景；如需高效，可以加**缓冲流**。
- **`java.nio` 包**：基于**缓冲区**与**通道**的非阻塞式 I/O，提供**文件锁**与**内存映射**等高级功能，适用于**高并发**、**大文件**场景。

合理地选择 I/O 模型能显著提高程序性能与可扩展性，在实际开发中，应根据需求（文件大小、并发度、操作频率、网络 I/O 还是本地 I/O 等）来确定采用 **IO** 还是 **NIO**。祝你在项目中灵活运用，编写出高效、稳定的 Java I/O 程序。