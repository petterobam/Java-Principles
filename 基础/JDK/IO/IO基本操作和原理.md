### Java 的 IO 操作

Java 的 IO (输入/输出) 操作涉及读取和写入数据，包括文件、网络连接、数据流等。Java 的 IO 库提供了多种类和接口来处理不同类型的数据源和目标。

#### Java IO 类层次结构

Java 的 IO 类层次结构主要分为两大类：字节流和字符流。

1. **字节流 (Byte Streams)**：
   - **输入流**：`InputStream` 及其子类，例如 `FileInputStream`, `ByteArrayInputStream`, `BufferedInputStream`。
   - **输出流**：`OutputStream` 及其子类，例如 `FileOutputStream`, `ByteArrayOutputStream`, `BufferedOutputStream`。

2. **字符流 (Character Streams)**：
   - **输入流**：`Reader` 及其子类，例如 `FileReader`, `StringReader`, `BufferedReader`。
   - **输出流**：`Writer` 及其子类，例如 `FileWriter`, `StringWriter`, `BufferedWriter`。

#### 常用的 IO 类和方法

##### 字节流
- **FileInputStream** 和 **FileOutputStream** 用于读取和写入文件的字节数据。

```java
// 使用 FileInputStream 读取文件
try (FileInputStream fis = new FileInputStream("input.txt")) {
    int content;
    while ((content = fis.read()) != -1) {
        System.out.print((char) content);
    }
} catch (IOException e) {
    e.printStackTrace();
}

// 使用 FileOutputStream 写入文件
try (FileOutputStream fos = new FileOutputStream("output.txt")) {
    String str = "Hello, World!";
    fos.write(str.getBytes());
} catch (IOException e) {
    e.printStackTrace();
}
```

##### 字符流
- **FileReader** 和 **FileWriter** 用于读取和写入文件的字符数据。

```java
// 使用 FileReader 读取文件
try (FileReader fr = new FileReader("input.txt")) {
    int content;
    while ((content = fr.read()) != -1) {
        System.out.print((char) content);
    }
} catch (IOException e) {
    e.printStackTrace();
}

// 使用 FileWriter 写入文件
try (FileWriter fw = new FileWriter("output.txt")) {
    fw.write("Hello, World!");
} catch (IOException e) {
    e.printStackTrace();
}
```

##### 缓冲流
- **BufferedReader** 和 **BufferedWriter** 用于提供缓冲功能，提高读写性能。

```java
// 使用 BufferedReader 读取文件
try (BufferedReader br = new BufferedReader(new FileReader("input.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}

// 使用 BufferedWriter 写入文件
try (BufferedWriter bw = new BufferedWriter(new FileWriter("output.txt"))) {
    bw.write("Hello, World!");
    bw.newLine();
    bw.write("Goodbye, World!");
} catch (IOException e) {
    e.printStackTrace();
}
```

#### Java IO 设计原理

Java 的 IO 设计基于以下原则和模式：

1. **抽象与实现分离**：Java IO 库中的所有类都继承自抽象类或实现了接口，例如 `InputStream`, `OutputStream`, `Reader`, `Writer`。这提供了灵活性，使得开发者可以通过实现这些抽象类或接口来扩展功能。

2. **装饰器模式 (Decorator Pattern)**：Java IO 广泛使用了装饰器模式来增强基本流的功能。例如，`BufferedInputStream` 和 `BufferedOutputStream` 提供缓冲功能，而 `DataInputStream` 和 `DataOutputStream` 提供对基本数据类型的读写支持。

3. **链式设计**：通过将一个流包装在另一个流中，Java IO 可以实现功能的链式扩展。例如，可以将一个 `FileInputStream` 包装在一个 `BufferedInputStream` 中，然后再包装在一个 `DataInputStream` 中，从而实现文件读写、缓冲和基本数据类型读写的综合功能。

4. **统一接口**：所有输入流都继承自 `InputStream`，所有输出流都继承自 `OutputStream`。同样，所有字符输入流都继承自 `Reader`，所有字符输出流都继承自 `Writer`。这使得不同类型的流可以通过相同的接口进行操作，增强了代码的通用性和可维护性。

#### 示例：装饰器模式的应用

```java
// 读取二进制文件，并将其内容缓冲到内存中，同时读取基本数据类型
try (DataInputStream dis = new DataInputStream(new BufferedInputStream(new FileInputStream("data.bin")))) {
    int data = dis.readInt();
    System.out.println("Read int: " + data);
} catch (IOException e) {
    e.printStackTrace();
}

// 写入基本数据类型到文件，并使用缓冲来提高性能
try (DataOutputStream dos = new DataOutputStream(new BufferedOutputStream(new FileOutputStream("data.bin")))) {
    dos.writeInt(12345);
} catch (IOException e) {
    e.printStackTrace();
}
```

在这个示例中，`FileInputStream` 和 `FileOutputStream` 提供了基础的文件读写功能，`BufferedInputStream` 和 `BufferedOutputStream` 提供了缓冲功能，而 `DataInputStream` 和 `DataOutputStream` 提供了对基本数据类型的读写支持。通过这种方式，Java IO 提供了高度可扩展和灵活的读写操作。