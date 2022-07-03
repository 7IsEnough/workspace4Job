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