<img src="images\0.png" style="zoom:200%;" />

# IO流

**主要内容**

- File类
- 递归算法
- IO流介绍
- 文件流使用
- try-with-source
- 缓冲流
- 序列化与反序列化

**学习目标**

| 知识点           | 要求 |
| ---------------- | ---- |
| File类           | 掌握 |
| 递归算法         | 掌握 |
| IO流介绍         | 掌握 |
| 文件流使用       | 掌握 |
| try-with-source  | 掌握 |
| 缓冲流           | 掌握 |
| 序列化与反序列化 | 掌握 |



## 一. File类

### **1. 介绍**

​	在JDK中存在一个java.io.File类，这个类是对应的是操作系统中的<font style='color:blue'>**一个文件**或**一个文件夹**</font>。

​	通过File可以实现系统中文件的创建、删除、查看、重命名等操作。

### 2. 路径强调

​	File类操作资源时有两种路径写法：

​		1. 全路径。即：从磁盘名称开始的路径。

​		2. 项目路径。从项目跟目录开始的路径。从项目的所在位置开始获取的。

​	在windows中，文件路径都是通过右斜杠\表示子目录。

​	例如：在D盘下名称为soft的的文件夹，在soft文件夹中有名称为jdk的文件夹。对应的路径为：

 <img src="images\1.png"/>

​	在Java中如果字符串的值中出现右斜线表示转义字符。会对后面的一个字符进行转义。\n \t 所以在Java中如果想要表示上面的路径可以使用两个右斜线或一个左斜线。\\  /

​	对应windows中D:\soft\jdk的路径，在Java中路径的字符串写法为

 <img src="images\2.png"/>

### **3.** File类构造方法

 <img src="images\3.png"/>

​	File 一共提供了四个构造方法，都是有参构造，并没有提供我们能使用的无参构造。其中我们比较常用的是File(String)和File(String,String)和File(File,String)

​	File(String): 参数表示文件或文件夹全路径。

​	File(String,String):中第一个参数表示父文件夹路径，第二个参数表示操作资源路径。

​	File(File,String):第一个参数表示父文件夹对象，第二个参数表示操作资路径。

### **4.** 创建文件夹

​	File中有两个方法能够创建文件夹。

​	<font style='color:blue'>**mkDir()**：</font>创建路径中最后一层文件夹。如果希望创建的文件夹已经存在或路径中前面文件夹不存在，返回false。

​	<font style='color:blue'>**mkDirs()**：</font>创建文件夹，如果路径中的文件夹不存在，先创建路径中的文件夹, 然后再创建指定的文件夹。返回值表示是否创建成功。

​	代码示例：

```java
public static void main(String[] args) {
  File file = new File("D:/a/b");
  //只会创建b, 如果a不存在创建失败
  boolean mkdir = file.mkdir();
  System.out.println(mkdir);

  File file1 = new File("D:/c/d");
  //c不存在会先创建c再创建d
  boolean mkdirs = file1.mkdirs();
  System.out.println(mkdirs);
}
```

### **5.** 判断资源是否存在

​	<font style='color:blue'>**exists()**</font>: 判断资源是否存在。存在返回true，不存在返回false。

​	代码示例：

```java
public static void main(String[] args) {
  File file = new File("D:/io");
  if (file.exists()) {
    System.out.println("D盘中没有io文件夹");
  }else {
    System.out.println("D盘中存在io文件夹");
  }
}
```

### **6.** 创建空文件

​	<font style='color:blue'>**createNewFile()**</font> 方法可以创建一个空文件，要求文件所在目录必须真实存在。

​	JKDK在定义这个方法时抛出了IOException（Checked异常），文件路径不正确、无法操作文件等情况下抛出此异常。返回值表示是否创建成功。

```java
public boolean createNewFile() throws IOException {
  SecurityManager security = System.getSecurityManager();
  if (security != null) security.checkWrite(path);
  if (isInvalid()) {
    throw new IOException("Invalid file path");
  }
  return fs.createFileExclusively(path);
}
```

​	示例代码：

```java
public static void main(String[] args) {
  try {
    File file = new File("D:/io/a.txt");
    boolean newFile = file.createNewFile();
    System.out.println(newFile);
  } catch (IOException e) {
    e.printStackTrace();
  }
}
```

#### **6.1** 结果一

​	设定计算机中D盘下没有io文件夹。程序运行会出现异常。

 <img src="images\4.png"/>

#### **6.2** 结果二

​	如果磁盘D盘中存在io文件夹。程序运行结果输出：true。

​	并在io文件夹中创建一个a.txt文件。文件内容为空。

#### **6.3** 代码优化

​	为了防止新建文件时因为目录不存在报异常，可以结合exists()和mkDirs()方法保证程序的稳定运行

```java
public static void main(String[] args) {
  try {
    File parent = new File("D:/io");
    if (!parent.exists()) {
      parent.mkdirs();
    }
    File file = new File(parent, "a.txt");
    boolean newFile = file.createNewFile();
    System.out.println(newFile);
  } catch (IOException e) {
    e.printStackTrace();
  }
}
```

### 7. 重命名文件

​	<font style='color:blue'>**renameTo(File)**</font> 剪切。利用剪切特性，把资源剪切后放入到原路径中，起新名字，实现重命名功能。

还可以利用此方法实现文件换目录的功能

```java
public static void main(String[] args) {
  File file = new File("D:/io/a.txt");
  boolean b = file.renameTo(new File("D:/io/b.txt"));
  System.out.println(b);
}
```

### 8. 删除文件

​	<font style='color:blue'>**delete()**</font> 可以实现删除文件。如果文件成功删除返回true。如果文件不存在或删除失败返回false。

```
//1. delete 无法有内容的文件夹
//2. delete 在删除文件的时候， 不管文件内是否有内容 都删
```

```java
public static void main(String[] args) {
  File file = new File("D:/io/a.txt");
  boolean delete = file.delete();
  System.out.println(delete);
}
```

### **9.** 判断资源是否为文件

​	<font style='color:blue'>**isFile()**</font> 判断资源是否是文件。如果是文件返回true，如果是文件夹或不存在这个资源返回false。

```java
public static void main(String[] args) {
  File file = new File("D:/io/a.txt");
  System.out.println(file.isFile());
}
```

### **1**0. 查看文件名和文件路径

​	<font style='color:blue'>**getName()**</font>  文件名。

​	<font style='color:blue'>**getAbsolutePath() **</font> 文件在磁盘中全路径。

​	<font style='color:blue'>**getPath() **</font> 创建文件时指定的路径。

```java
public static void main(String[] args) {
  File file = new File("abc/test.txt");
  System.out.println(file.getName()); //test.txt
  System.out.println(file.getAbsolutePath()); //D:\ideaws\abc\test.txt
  System.out.println(file.getPath()); //abc\test.txt
}
```

### 11. 查看目录中内容

​	<font style='color:blue'>**list()** </font>返回值为目录中资源的名称。返回值类型String[]。数组长度和资源个数相同。如果是空文件夹范围一个长度为0的数组对象。如果文件夹不存在返回null。

​	<font style='color:blue'>**listFiles()** </font>返回值为目录中资源对象。返回值类型File[]。数组长度和资源个数相同。如果是空文件夹范围一个长度为0的数组对象。如果文件夹不存在返回null。

```java
public static void main(String[] args) {
  File file = new File("D:/io");
  //获取文件夹中资源名称
  String[] list = file.list();
  for (String s : list) {
    System.out.println(s);
  }
  //获取文件中资源对象
  File[] files = file.listFiles();
  for (File file1 : files) {
    System.out.println(file1);
  }
}
```



## 二. 小节实战案例 - 使用递归打印目录结构。

### 1. 需求：

​		1. 打印D盘中io文件夹及子文件夹中内容。

​		2. 打印时格式为：资源名称 -- 文件夹 或 资源名称 -- 文件。

​		3. 打印时如果是文件夹，在下面打印出文件夹的子内容，并且子内容前面有缩进

### 2. 代码实现:

```java
public class Test {
  public static void main(String[] args) {
    //1.创建 D:/soft 对象
    File file = new File("D:/soft");
    //2.指定计数器
    int count = 0;
    test(file, count);
  }

  public static void test(File file, int count) {
    //1.获取目录中的所有文件和文件夹
    File[] files = file.listFiles();
    if (files != null) {
      for (File file1 : files) {
        for (int i = 0; i < count; i++) {
          System.out.print("\t");
        }
        System.out.println(file1.getName() + " -> " + (file1.isFile() ? "文件" : "文件夹"));
        //递归调用
        test(file1, count+1);
      }
    }
  }
}
```



## 三. IO流

### **1.** 介绍

​	流就是数据无结构化的传递。强调的是数据的传输过程。

​	流分为**输入流（<font style='color:red'>I</font>nput）**和**输出流(<font style='color:red'>O</font>utput)**，所以简称为I/O流。

​	输入流表示从一个源读取数据，输出流表示向一个目标写数据。

​	Java中I/O操作主要是指使用`java.io`包下的内容，进行输入、输出操作。**输入也叫做读取数据，输出也叫做作写出数据**。

​	输入流: 从磁盘读到内存中

 <img src="images\6.png" style="zoom:67%;" />

​	输出流 : 从内存写到磁盘中

 <img src="images\5.png" style="zoom:67%;" />

​	配合使用:

 <img src="images\7.png" style="zoom:67%;" />

​	Java中的流是对字节序列的抽象，我们可以想象有一个水管，只不过现在流动在水管中的不再是水，而是字节序列。和水流一样，Java中的流也具有一个“流动的方向”，通常可以从中读入一个字节序列的对象被称为输入流；能够向其写入一个字节序列的对象被称为输出流。

​	输入输出流是一个相对概念。主要看以谁为参照物。如果数据是从参照物出去就是输出流。如果数据是往参照物进就是输入流。

​	例如：我们一直用的控制台输入。用户在控制台输入的内容，会存储到程序中的一个变量里。出发点是控制台，目的地是程序中，流是把输入内容传递到变量的过程，这种流就是输入流。当我们使用一个输出语句打印文字时，文字的出发点是应用程序，目的地是控制台，流负责把变量对应内容运输到控制台，这种流就是输出流。

​	**对于Java程序来说，输入输出流都是相对应用程序来说的**。

​	**总结**：

​		1. 流是一个抽象概念。流是看不见的。

​		2. 流有出发点和目的地。相对出发点来说就是输出流，相对目的地来说就是输入流。

​		3. 流是数据运输的载体。

### **2.** Java中IO流分类

​	在JDK中提供了IO类的支持。这些类都在java.io包中。

​	1. 根据方向的划分: 输入流和输出流

​	2. 根据数据的单位划分: 字节流和字符流。

​	3. 根据功能的划分: 节点流和处理流(缓冲流)

​	字节流就是流中数据以字节为单位（byte）。特别适用于音频、视频、图片等二进制文件的输入、输出操作。

​	字符流就是流中数据以字符为单位。存在的意义：高效、方便的读取文本数据。此处需要注意的是字符流单位可能是一个字节，可能是多个字节。例如读取英文a，就用一个字符，读取汉字就两个字符。

### 3. Java中IO流体系图

 <img src="images\8.png" style="zoom:38%;" />

### 4. Java中IO常用类体系图

​	虽然我们作为一个Java程序员要学习JDK，但是我们不可能把JDK中所有内容都记住，所以提出上图中我们比较常用的类如下：

 <img src="images\9.png" style="zoom:40%;" />

## 四. 字节输入输出流

### **1.** 介绍

​	传输过程中，传输数据的最基本单位是字节的流。

​	FileInputStream和FileOutputStream是文件操作常用的两种流。我们可以借助这两个流实现文件读取、文件输出、文件拷贝等功能。

​	把硬盘上的文件读取到应用程序中使用FileInputStream。

​	把应用程序中文件内容输出到硬盘上使用FileOutputStream。

​	注意：

​		**1. 流使用完毕一定要关闭，释放资源。**

​		**2. 如果输出流内容来源于输入流，要先关输出流后关输入流**。

### **2.** FileInputStream API 介绍

 <img src="images\10.png" style="zoom:50%;" />

### 3. FileOutputStream API 介绍

 <img src="images\11.png" style="zoom:50%;" />

### 4. 字节输入流的使用

```java
public static void main(String[] args) {
  FileInputStream inputStream = null;
  //将D:/a.txt读取到程序中. 完成展示
  try {
    //1.创建字节输入流 FileInputStream
    inputStream = new FileInputStream("D:/a.txt");
    //2读取文件中的内容
    int read;
    while ((read = inputStream.read()) != -1) {
      //3每一次读取一个字节. 没有了返回-1
      System.out.println((char) read);
    }
  } catch (IOException e) {
    e.printStackTrace();
  } finally {
    try {
      inputStream.close();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```

### 5. 字节输出流的使用

```java
public static void main(String[] args) {
  FileOutputStream fileOutputStream = null;
  try {
    //1.创建文件字节输出流 FileOutputStream  自动创建文件
    fileOutputStream = new FileOutputStream("D:/qwe.txt");
    //2.输出内容
    String str = "efg";
    byte[] bytes = str.getBytes(); //将字符串转换为byte数组
    fileOutputStream.write(bytes);
  } catch (FileNotFoundException e) {
    e.printStackTrace();
  } catch (IOException e) {
    e.printStackTrace();
  } finally {
    try {
      fileOutputStream.close();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```

## **五.** 小节实战案例 - 使用字节流复制文件

### 1. 需求：

​		1. 在D盘根目录包含文件test.png

​		2. 把test.png复制一份，命名为test_copy.png(拷贝)

### 2. 实现思路：

​		1. 使用FileInputStream 读取test.png

​		2. 创建FileOutputStream，指定输出目的地为D:/test_copy.png

​		3. 遍历FileInputStream读取的数据，遍历的同时将读取的数据输出。

​		4. 先关闭输出流，再关闭输入流

### 3. 代码示例：

​	建议使用数组进行读取, 每一次可以读写数组长度的数据, 效率较高

```java
public static void main(String[] args) {
  FileInputStream fileInputStream = null;
  FileOutputStream fileOutputStream = null;
  try {
    //1.创建字节输入流
    fileInputStream = new FileInputStream("D:/test.png");
    //2.创建字节输出流
    fileOutputStream = new FileOutputStream("D:/test_copy.png");
    //3.读取图片  
    byte[] bytes = new byte[1024];
    //返回值为 每次读取的长度
    int lenth;
    while ((lenth = fileInputStream.read(bytes)) != -1) {
      //4.将读取到的字节数组中的内容写到本地磁盘
      //参数1: 字节数组  参数2: 从哪个下标开始写  参数3: 每次写的长度
      fileOutputStream.write(bytes, 0, lenth);
    }
  } catch (FileNotFoundException e) {
    e.printStackTrace();
  } catch (IOException e) {
    e.printStackTrace();
  } finally {
    try {
      //5.资源释放
      fileOutputStream.close();
      fileInputStream.close();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```



## 六. 字符输入输出流

### **1.** 介绍	

​	传输过程中，传输数据的最基本单位是字符的流。

​	字符编码方式不同，有时候一个字符使用的字节数也不一样，**比如ASCLL方式编码的字符，占一个字节；而UTF-8方式编码的字符，一个英文字符需要一个字节，一个中文需要三个字节。**

​	注意: 脱离具体的编码谈某个字符占几个字节是没有意义的 

​	同一个字符在不同的编码下可能占不同的字节。

就以你举的“ 字”字为例，“ 字”在 GBK 编码下占 2 字节，在 UTF-16 编码下也占 2 字节，在 UTF-8 编码下占 3 字节，在 UTF-32 编码下占 4 字节。



​	不同的字符在同一个编码下也可能占不同的字节。

 字”在 UTF-8 编码下占3字节，而“ A”在 UTF-8 编码下占 1 字节。（因为 UTF-8 是 变长编码）



​	字节数据是二进制形式的，要转成我们能识别的正常字符，需要选择正确的编码方式。我们生活中遇到的乱码问题就是字节数据没有选择正确的编码方式来显示成字符。

​	因为数据编码的不同，因而有了对字符进行高效操作的流对象，字符流本质其实就是基于字节流读取时，去查了指定的码表，而字节流直接读取数据可能会有乱码的问题（中文会乱码）

### 2. BufferedReader API介绍

 <img src="images\14.png" style="zoom:50%;" />

### 3. BufferedWriter API介绍

 <img src="images\13.png" style="zoom:50%;" />

### 4. 字符输入流的使用

```java
public static void main(String[] args) {
  FileReader fileReader = null;
  try {
    //1.创建字符输入流对象
    fileReader = new FileReader("D:/a.txt");
    //2.读取 单位: 字符
    char[] chars = new char[1024];
    int length; //每次读取的长度
    while ((length = fileReader.read(chars)) != -1) {
      System.out.println(new String(chars, 0, length));
    }
  } catch (FileNotFoundException e) {
    e.printStackTrace();
  } catch (IOException e) {
    e.printStackTrace();
  } finally {
    try {
      fileReader.close();
    } catch (Exception e) {
      e.printStackTrace();
    }
  }
}
```

### 5. 字符输出流的使用

```java
public static void main(String[] args) {
  FileWriter fileWriter = null;
  try {
    //1.创建字符输出流对象
    fileWriter = new FileWriter("/Users/cy/Desktop/a.txt");
    //2.输出
    fileWriter.write("你好啊");
  } catch (FileNotFoundException e) {
    e.printStackTrace();
  } catch (IOException e) {
    e.printStackTrace();
  } finally {
    try {
      fileWriter.close();
    } catch (Exception e) {
      e.printStackTrace();
    }
  }
}
```



## **七.** try-with-source

### **1.** 介绍

​	try-with-source 是java 7引入的。主要是为了解决因为忘记关闭资源而导致的性能问题和调用关闭方法而导致的代码结构乱的问题。

### **2.** 语法

​	<font style='color:blue'>**try(需要在finally关闭的资源对象定义,可以写多条代码){**</font>

​	<font style='color:blue'>**}catch(){**</font>

​	<font style='color:blue'>**}**</font>

**我们发现和普通的try-catch 就在try后面多个一个括号。定义到括号里面的对象都不需要程序员手动close()关闭资源。**

### 3. 使用

​	推荐使用字符流, 将a.txt中的文本内容拷贝到b.txt中

```java
public static void main(String[] args) {
  try (
    //1.创建字符输入流对象
    FileReader fileReader = new FileReader("D:/a.txt");
    //2.创建字符输出流对象
    FileWriter fileWriter = new FileWriter("D:/b.txt");)
  {
    //3.读取
    char[] chars = new char[1024];
    int length; //每次读取的长度
    while ((length = fileReader.read(chars)) != -1) {
      //4.输出
      fileWriter.write(chars, 0, length);
    }
  } catch (I nb  OException e) {
    e.printStackTrace();
  }

}
```



## **八.** 缓冲流

### **1.** 介绍



​	Java IO中BufferedXXX相关的流统称缓冲流。其本质就是在输入输出时添加缓冲区，减少磁盘IO的次数, 这样可以提高读写效率，同时也可以反复读取。

​	缓冲流称为上层流(高效流)，其底层必须有字节流或字符流。当使用完成后关闭缓冲流，字节流或字符流也会随之关闭。

​	<font style='color:red'>**可以使用flush()刷新（清空）缓冲区内容，把内容输出到目的地。**</font>

​	<font style='color:red'>**当缓存区满了以后会自动刷新。在代码中当关闭流时，底层自动调用flush()方法。缓存区的大小是 integer.max_value - 8**</font>

**缓冲流的优点及实现原理：** (面试会问)

**不带缓冲流的工作原理：**

**读取一个字节/字符，就会向用户指定的路径写出去，读一个写一个，频繁的读写增加了读写次数，降低了效率**

**带缓冲流的工作原理：**

**读取到一个字节/字符，先不输出，等凑足了缓冲的最大容量后一次性写出去，减少了读写次数，提高了效率**



### 2. 字节缓冲流

```java
/* 创建字节输入缓冲流 */
//方式一:
FileInputStream fis = new FileInputStream("D:/a.png");
BufferedInputStream bis = new BufferedInputStream(fis);
//方式二:
BufferedInputStream bis1 = new BufferedInputStream(new FileInputStream("D:/a.png"));

/* 创建字节输出缓冲流 */
//方式一:
FileOutputStream fos = new FileOutputStream("D:/b.png");
BufferedOutputStream bos = new BufferedOutputStream(fos);
//方式二:
BufferedOutputStream bos1 = new BufferedOutputStream(new FileOutputStream("D:/b.png"));
```

### 3. 字符缓冲流

```java
/* 创建字符输入缓冲流 */
//方式一:
FileReader fr = new FileReader("D:/a.txt");
BufferedReader bufferedReader = new BufferedReader(fr);
//方式二:
BufferedReader bufferedReader1 = new BufferedReader(new FileReader("D:/a.txt"));

/* 创建字符输出缓冲流 */
//方式一:
FileWriter fw = new FileWriter("D:/b.txt");
BufferedWriter bufferedWriter = new BufferedWriter(fw);
//方式二:
BufferedWriter bufferedWriter1 = new BufferedWriter(new FileWriter("D:/b.txt"));
```



## **九. ** 小节实战案例 - 把文件中内容复制到另一个文件中

### 1. 需求

​	在D盘下有a.txt文件, 包含两行内容

​		我的名字：

​				张三

​	需要从a.txt中把

​		我的名字：

​				张三

​	复制到b.txt文件中。

### 2. 实现思路：

​	创建缓冲输入, 输出流，读取a.txt, 写到b.txt

​	使用readLine(), 循环读取

### 3. 代码实现

```java
public static void main(String[] args) {
  try(
    //1.创建字符输入流缓冲区(创建字符输入流对象)
    BufferedReader br = new BufferedReader(new FileReader("D:/a.txt"));
    //2. 创建字符输出缓冲区(创建字符输出流 )
    BufferedWriter bw = new BufferedWriter(new FileWriter("D:/b.txt"));)
  {
    String str = null;
    //每次读取 一行
    while ((str = br.readLine()) != null){
      bw.write(str);
      //换行
      bw.newLine();
    }
    //手动刷新输出流缓冲区
    bw.flush();
  } catch (FileNotFoundException e) {
    e.printStackTrace();
  } catch (IOException e) {
    e.printStackTrace();
  }
}
```

### 4. 代码优化

```java
public static void main(String[] args) {
  try(
    //1.创建字符输入流缓冲区(创建字符输入流对象)
    BufferedReader br = new BufferedReader(new FileReader("D:/a.txt"));
    //2. 创建字符输出缓冲区(创建字符输出流 )
    BufferedWriter bw = new BufferedWriter(new FileWriter("D:/b.txt", true));) //新添加
  {
    String str = null;
    //每次读取 一行
    while ((str = br.readLine()) != null){
      System.out.println(str);
      //不会覆盖原始内容, 在原来内容后追加
      bw.append(str);
      //换行
      bw.newLine();
    }
    //手动刷新输出流缓冲区
    bw.flush();
  } catch (FileNotFoundException e) {
    e.printStackTrace();
  } catch (IOException e) {
    e.printStackTrace();
  }
}
```



## 十. 序列化与反序列化

### 1. 介绍

​	序列化：**把对象转换为字节数组**。在java中通过ObjectOutputStream序列化。序列化后内容必须接收序列化的结果。所以在ObjectOutputSteam构造方法需要要一个OutputStream或其子类输出流对象，这个对象就是负责接收序列化结果的。

​	反序列化：**把字节数组在转换为对象**。在java中通过ObjectInputStream反序列化，反序列化的结果在内存，需要通过输入流对象接收反序列化结果。所以在ObjectInputStream构造方法参数必须要一个InputStream对象，这个对象就负责接收反序列化结果的。

简单的说

​    序列化：将java对象转化为字节序列的过程。

​    反序列化：将字节序列转化为java对象的过程。

再简单点说：

​	序列化是指把一个Java对象变成二进制内容，本质上就是一个byte[]数组。



### **2. **为什么要把Java序列化

​	序列化后对象就是字节数组。变为字节数组后就可以把数组中内容输出到本地硬盘中，在后面学习的网络通信中，数据传输时也需要将对象转换为字节。

### 3. 如何序列化

​	让需要序列化的类实现Serializable接口，实现了这个接口代表这个类允许被序列化。

​	通过Java中ObjectOutputStream把对象进行序列化, ObjectInputStream把对象反序列化。

### 4. 序列化代码实现流程

#### 4.1 新建类

```java
public class Student implements Serializable {
  private String name;
  private int age;
	
  //...
}
```

#### 4.2 序列化

​	将对象变为字节数组

```java
public static void main(String[] args) throws IOException {
  Student zs = new Student("zs", 18);
  //序列化
  ByteArrayOutputStream baos = new ByteArrayOutputStream();
  ObjectOutputStream oos = new ObjectOutputStream(baos);
  //将对象序列化, 对象->字节数组
  oos.writeObject(zs);
  //获取对象的字节数组
  byte[] bytes = baos.toByteArray();
}
```

#### 4.3 注意事项

​	<font style='color:red'>**没有实现序列化接口**</font>

 <img src="images\15.png" style="zoom:50%;" />

### 6. 反序列化实现流程

​	把字节数组转换为对象。

```java
public static void main(String[] args) throws IOException, ClassNotFoundException {
  Student zs = new Student("zs", 18);
  /* 序列化 */
  ByteArrayOutputStream baos = new ByteArrayOutputStream();
  ObjectOutputStream oos = new ObjectOutputStream(baos);
  //将对象序列化, 对象->字节数组
  oos.writeObject(zs);
  //字节数组
  byte[] bytes = baos.toByteArray();

  /* 反序列化 */
  ByteArrayInputStream bais = new ByteArrayInputStream(bytes);
  ObjectInputStream ois = new ObjectInputStream(bais);
  Student o = (Student) ois.readObject();
}
```

### 7. 属性值不参与序列化

​	如果类中包含一些私密属性，例如: 密码等。可以通过transient关键字，禁止该属性值被序列化。

```java
public class Student implements Serializable {
  private String name;
  private transient int age;
	
  //...
}
```

### 8. 序列码(重要)

```
private static final long serialVersionUID = 123456789L;
```

​	在实现了Serializable接口的类中。如果没有显示添加序列码会由JVM生成一个。

​	程序员也可以自己显示添加一个序列码，这个序列码和类中代码有关系，如果类中内容不变序列化和反序列化是没有影响的。

​	但是在企业级项目中，难免碰见需要修改类结构的情况。例如：添加一个新的属性。当我们修改了类结构后，类中自动生成的序列码就会改变。但是要求序列化和反序列化时序列码必须相同。而由于序列码的改变，所以在反序列化时会出错。为了防止这种问题，开发中只要实现了序列化都会添加序列码。

#### 8.1 把序列化结果输出到文件中

```java
public static void main(String[] args) throws IOException, ClassNotFoundException {
  Student zs = new Student("zs", 18);
  ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("D:/a.txt"));
  oos.writeObject(zs);
}
```

#### 8.2 把文件中内容反序列化为对象

```java
public static void main(String[] args) throws IOException, ClassNotFoundException {
  ObjectInputStream ois = new ObjectInputStream(new FileInputStream("D:/a.txt"));
  Student stu = (Student) ois.readObject();
  System.out.println(stu);
}
```

#### 8.3 添加新的属性

```java
public class Student implements Serializable {
  private String name;
  private int age;
	private String addr;
  
  //...
}
```

​	运行反序列化, 异常信息如下

```java
Exception in thread "main" java.io.InvalidClassException: com.bjsxt.service.Student; local class incompatible: stream classdesc serialVersionUID = 7117420395380926872, local class serialVersionUID = -561887848319347330
	at java.base/java.io.ObjectStreamClass.initNonProxy(ObjectStreamClass.java:689)
	at java.base/java.io.ObjectInputStream.readNonProxyDesc(ObjectInputStream.java:2023)
	at java.base/java.io.ObjectInputStream.readClassDesc(ObjectInputStream.java:1873)
	at java.base/java.io.ObjectInputStream.readOrdinaryObject(ObjectInputStream.java:2180)
	at java.base/java.io.ObjectInputStream.readObject0(ObjectInputStream.java:1690)
	at java.base/java.io.ObjectInputStream.readObject(ObjectInputStream.java:499)
	at java.base/java.io.ObjectInputStream.readObject(ObjectInputStream.java:457)
	at com.bjsxt.test.Test.main(Test.java:12)
```

#### 8.4 显示指定添加序列码

​	解决办法是在类中添加serialVersionUID ，需要注意的是，等号左侧的写法是固定的。等号右侧的值随意。

```java
private static final long serialVersionUID = 123456789L;
```

#### 8.5 解决修改代码序列码不一致

```java
public class Student implements Serializable {
  private static final long serialVersionUID = 123456789L;
  private String name;
  private int age;
  
  //...
}
```

#### 8.6 再次测试结果

​	重新序列化, 反序列化



## 十一. 综合实战案例

​	完成登录注册功能。

### 1. 需求:

​	1. 只允许SystemService的实现类中包含控制台的输入输出代码。

​	2. 用户注册的数据使用序列化存储到当前项目根路径下user.txt文件中。

​	3. 用户登录需要的数据使用反序列化读取user.txt

​	4. 所有io操作必须在com.bjsxt.io包中，面向接口编程。

​	5. 用户只需有用户名和密码即可。

​	6. 用户包含登录注册两大功能。

​	7. 数据是持久化操作，多次运行程序，保留之前注册的用户信息。

### 2. 实现思路:

​	1. 抽象用户类。包含用户名和密码属性。封装。

​	2. 编写系统菜单显示。

​	3. 注册时控制台输入的信息在SystemService中完成，调用UserService实现类的注册方法，UserService实现类的注册方法调用，io包下读取user.txt的方法。

​	4. 登录时是控制台输入的信息在SystemService中完成，调用UserService实现类的登录方法，UserService实现类的登录方法调用io包下读取user.txt的方法。

### 3. 效果图

#### 3.1 先注册, 再登录

 <img src="images\16.png" style="zoom:50%;" />

#### 3.2 重启程序, 直接登录

 <img src="images\17.png" style="zoom:50%;" />

### 4. 代码实现

#### 4.1 User

```java
public class User implements Serializable {
  private static final long serialVersionUID = 123L;
  private String username;
  private String passowrd;

  //...
}
```

#### 4.2 SystemService, SystemServiceImpl

```java
public interface SystemService {
  public void show();
}

```

```java
public class SystemServiceImpl implements SystemService{
  public void show(){
    UserServiceImpl userService = new UserServiceImpl();
    Scanner sc = new Scanner(System.in);
    User user = null;
    o: while (true){
      System.out.println("\n1.登录");
      System.out.println("2.注册");
      int i = sc.nextInt();
      switch (i) {
        case 1: //登录
          System.out.println("请输入用户名");
          String username = sc.next();
          System.out.println("请输入密码");
          String pwd = sc.next();
          boolean login = userService.login(username, pwd, user);
          if (login) {
            System.out.println("登录成功");
          }else {
            System.out.println("登录失败");
            continue o;
          }
          break ;
        case 2: //注册
          System.out.println("请输入用户名");
          String uname = sc.next();
          System.out.println("请输入密码");
          String pwd1 = sc.next();
          userService.regist(uname, pwd1);
      }
    }
  }
}
```

#### 4.3 UserService, UserServiceImpl

```java
public interface UserService {
  boolean login(String username, String password, User user);
  User regist(String username, String password);
}
```

```java
public class UserServiceImpl implements UserService {
  @Override
  public boolean login(String username, String password, User user) {
    IoServiceImpl ioService = new IoServiceImpl();
    User unser = ioService.unser();
    if (unser==null) {
      return false;
    }
    if(username.equals(unser.getUsername()) && password.equals(unser.getPassowrd())){
      return true;
    }else {
      return false;
    }
  }

  @Override
  public User regist(String username, String password) {
    User user = new User(username, password);
    IoServiceImpl ioService = new IoServiceImpl();
    ioService.ser(user);
    return user;
  }
}
```

#### 4.4 IoService, IoServiceImpl

```java
public interface IoService {
  void ser(User user);
  User unser();
}
```

```java
public class IoServiceImpl implements IoService {
  @Override
  public void ser(User user) {
    ObjectOutputStream oos = null;
    try {
      oos = new ObjectOutputStream(new FileOutputStream("D:/user.txt"));
      oos.writeObject(user);
    } catch (IOException e) {
      e.printStackTrace();
    }finally {
      try {
        if (oos != null) {
          oos.close();
        }
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }

  @Override
  public User unser() {
    ObjectInputStream ois = null;
    try {
      ois = new ObjectInputStream(new FileInputStream("D:/user.txt"));
      User o = (User) ois.readObject();
      return o;
    } catch (IOException | ClassNotFoundException e) {
      //e.printStackTrace();
      return null;
    }finally {
      try {
        if (ois!=null){
          ois.close();
        }
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }
}
```

