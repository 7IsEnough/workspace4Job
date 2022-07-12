# 一、JAVA基础

1. **JDK 和 JRE 有什么区别？**

   - JDK：Java Development Kit 的简称，Java 开发工具包，提供了 Java 的开发环境和运行环境
   - JRE：Java RunTime Environment的简称，java 运行环境，为 java 的运行提供了所需环境

   具体来说，JDK 包含了 JRE ，同时还包含了编辑 java 源码的编译器 javac ，还包含了很多 java 程序调试和分析的工具。

   简单来说，如果你需要运行 java 程序，只需安装 JRE 就可以了，如果你需要编写 java 程序，需要安装 JDK 。

2. **== 和 equals 的区别是什么**

   **== 解读**

   对于基本类型和引用类型 == 的作用效果是不同的，如下所示：

   - 基本类型：比较的是值是否相同
   - 引用类型：比较的是引用是否相同

   代码示例：

   ```java
   String x = "string";
   String y = "string";
   String z = new String("string");
   System.out.println(x == y); // true
   System.out.println(x == z); // false
   System.out.println(x.equals(y)); // true
   System.out.println(x.equals(z)); // true
   ```

   代码解读：因为 x 和 y 指向同一个引用，所以 == 也是 true ，而 new String() 方法则重写开辟了内存空间，所以 == 结果为 false ，

   而 equals 比较的一直是值，所以结果都为 true 。

   **equals 解读**

   equals 本质上就是 == ，只不过 String 和 Integer 等重写了 equals 方法，把它变成了值比较。

   首先来看默认情况下 equals 比较一个有相同值的对象，代码如下：

   ```java
   class Cat {
   
       private String name;
   
       public Cat(String name) {
           this.name = name;
       }
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   }
   
   Cat c1 = new Cat("王磊");
   Cat c2 = new Cat("王磊");
   System.out.println(c1.equals(c2)); // false
   ```

   输出结果为 false ？看 equals 的源码如下

   ```java
   public boolean equals(Object obj) {
       return (this == obj);
   }
   ```

   原来 equals 的本质上就是 == 。

   那为什么两个相同值的 String 对象返回的是 true ？

   ```java
   String s1 = new String("老王");
   String s2 = new String("老王");
   System.out.println(s1.equals(s2)); // true
   ```

   来看一下 String 的 equals 方法，代码如下：

   ```java
   
   public boolean equals(Object anObject) {
       if (this == anObject) {
           return true;
       }
       if (anObject instanceof String) {
           String anotherString = (String)anObject;
           int n = value.length;
           if (n == anotherString.value.length) {
               char v1[] = value;
               char v2[] = anotherString.value;
               int i = 0;
               while (n-- != 0) {
                   if (v1[i] != v2[i])
                       return false;
                   i++;
               }
               return true;
           }
       }
       return false;
   }
   ```

   原来是 String 重写了 Object 的 equals 方法，把引用比较改成了值比较

   **总结：**== 对于基本类型比较是值比较，对于引用类型来说是比较的引用；

   equals 默认情况下是引用比较，只是很多类重写了 equals 方法，比如 String 、Integer 等把它变成了值比较，所以一般情况下 equals 比较的是值是否相等。

3. **两个对象的 hashCode() 相同，则 equals() 也一定为true，对吗？**

   不对

   代码示例：

   ```java
   String str1 = "通话";
   String str2 = "重地";
   System.out.println(String.format("str1：%d | str2：%d",  str1.hashCode(),str2.hashCode()));
   System.out.println(str1.equals(str2));
   ```

   执行的结果：

   str1: 1179395 | str2: 1179395

   false

   hashCode() 是 Java 的本地方法，通过对象的内存地址（不止内存地址）计算出一个整数值，少数情况下，内存地址不同，这个整数值也有可能是相同的。

   很显然 “通话” 和 “重地” 的 hashCode() 相同，然而 equals() 则为 false，因为在散列表中，hashCode() 相等即两个键值对的哈希值相等，然而哈希值相等，并不一定能得出键值对相等。

4. **final 在 java 中有什么作用？**
   - final 修饰的类为最终类，该类不能被继承。
   - final 修饰的方法不能被重写。
   - final 修饰的变量叫常量，常量必须初始化，初始化之后的值就不能被修改。

5. **java 中的 Math.round(-1.5) 等于多少？**

   等于 -1，因为在数轴上取值时，中间值 (0.5) 向右取整，所以正 0.5 是往上取整，负 0.5 是直接舍弃。

6. **String 属于基础的数据类型吗？**

   不属于，基础类型为以下8种：byte、boolean、char、short、int、float、long、double，而 String 属于对象。

7. **java 中操作字符串都有哪些类？它们之间有什么区别？**

   String、StringBuilder、StringBuffer

   这3个类的区别在于 String 声明的是不可变的对象，每次操作都会生成新的 String 对象，然后将指针指向新的 String 对象，而 StringBuffer、StringBuilder 可以在原有对象的基础上进行操作，所以在经常改变字符串内容的情况下最好不要用 String。

   StringBuffer 与 StringBuilder 最大的区别在于，StringBuffer 是线程安全的，而 StringBuilder 是非线程安全的，但 StringBuilder 的性能却高与StringBuffer，所以在单线程环境下推荐使用 StringBuilder，多线程环境下推荐使用 StringBuffer。

8. **String str = "i" 与 String str = new String("i") 一样吗？**

   不一样，因为内存的分配方式不一样。String str = "i" 的方式，java 虚拟机会将其分配到常量池中；而  String str = new String("i") 则会被分配到堆内存中。

9. **如何将字符串反转**

   使用 StringBuilder 或者 StringBuffer 的 reverse() 方法。

   示例代码：

   ```java
   // StringBuffer reverse
   StringBuffer stringBuffer = new StringBuffer();
   stringBuffer.append("abcdefg");
   System.out.println(stringBuffer.reverse()); // gfedcba
   // StringBuilder reverse
   StringBuilder stringBuilder = new StringBuilder();
   stringBuilder.append("abcdefg");
   System.out.println(stringBuilder.reverse()); // gfedcba
   ```

10. **String 类的常用方法有哪些？**
    - indexOf()：返回指定字符的索引。
    - charAt()：返回指定索引处的字符。
    - replace()：字符串替换。
    - trim()：去除字符串两端空白。
    - split()：分割字符串，返回一个分割后的字符串数组。
    - getBytes()：返回字符串的 byte 类型数组。
    - length()：返回字符串长度。
    - toLowerCase()：将字符串转为小写字母。
    - toUpperCase()：将字符串转为大写字母。
    - subString()：截取字符串。
    - equals()：字符串比较。

11. **抽象类必须有抽象方法吗？**

    不需要，示例代码：

    ```java
    abstract class Cat {
        public static void sayHi() {
            System.out.println("hi~");
        }
    }
    ```

    上面代码，抽象类并没有抽象方法但完全可以正常运行。

12. **普通类和抽象类有哪些区别？**
    - 普通类不能含有抽象方法，抽象类可以包含抽象方法。
    - 抽象类不能直接实例化，普通类可以直接实例化。

13. **抽象类能使用 final 修饰吗？**

    不能，定义抽象类就是让其他类继承的，如果定义为 final 该类就不能被继承，这样彼此就会产生矛盾，所以 final 不能修饰抽象类，如下图所示，编译器也会提示错误信息：

    ![](data:image/png;base64,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)

14. **接口和抽象类有什么区别？**
    - 实现：抽象类的子类使用 extends 来继承；接口必须使用 implements 来实现接口。
    - 构造函数：抽象类可以有构造函数；接口不能有。
    - main 方法：抽象类可以有 main 方法，并且我们能运行它；接口不能有 main() 方法。
    - 实现数量：类可以实现很多个接口；但是只能继承一个抽象类。
    - 访问修饰符：接口中的方法默认使用 public 修饰；抽象类中的方法可以是任意访问修饰符。

15. **java 中 IO 流分为几种？**
    - 按功能来分：输入流(input)、输出流(output)。
    - 按类型来分：字节流和字符流
    - 字节流和字符流的区别是：字节流按8位传输以字节为单位输入输出数据，字符流按16位传输以字符为单位输入输出数据。
16. **BIO、NIO、AIO 有什么区别？**
    - BIO：Black IO 同步阻塞式 IO，就是我们平常使用的传统 IO，它的特点是模式简单使用方便，并发处理能力低。
    - NIO：New IO 同步非阻塞 IO，是传统 IO 的升级，客户端和服务器端通过 Channel (通道) 通讯，实现了多路复用。
    - AIO：Asynchronous IO 是 NIO 的升级，也叫 NIO2，实现了异步非堵塞 IO，异步 IO 的