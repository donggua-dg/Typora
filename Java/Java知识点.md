学习网址：http://c.biancheng.net/

# 面向对象

## 什么是面向对象

- 本质：<font color=red>以类的方式组织代码，以对象的方式封装数据</font>
- 抽象    对象，是具体的事物；类，是抽象的，是对对象的抽象
- 三大特性：
  - 封装
  - 继承
  - 多态

------



## 权限访问修饰符

保护对类、变量、方法和构造方法的访问，支持 4 种不同的访问权限。

- <font color=red>访问修饰符</font>

**default**（即默认，什么也不写）：在同一包内可见，不使用任何修饰符。

**private**：在同一类内可见。注意：不能修饰类（外部类）

**public**：对所有类可见。（接口中的方法默认是public）

**protected**：对同一包内的类和所有子类可见。注意：不能修饰类（外部类）。

![image-20211101192737611](E:\Typora\笔记\typora图片复制存储\image-20211101192737611.png)

- $\textcolor{red}{非访问修饰符}$

​        **static**

​        标记static的属性将会和类一起加载，因此可直接通过**类名.属性名（或方法名）** 来访问

​        没有标记static的，只有在类被实例化后才进行加载，所以需要实例化对象后才能通过**类名.属性名（或方法名）** 来访问

​        **final**

​        final 表示"最后的、最终的"含义，变量一旦赋值后，不能被重新赋值。被 final 修饰的实例变量必须显式指定初始值。

​        final 修饰符通常和 static 修饰符一起使用来创建类常量。

------



## 封装

- 该露的露，该藏的藏

我们程序设计要追求<font color=red>”高内聚，低耦合“</font>。

高内聚：就是类的内部数据操作细节自己完成，不允许外部干涉；

低耦合：仅暴露少量的方法给外部使用。

- 封装（数据的隐藏）

通常，应禁止直接访问一个对象中数据的实际表示，而应通过操作接口来访问，这称为信息隐藏。

- <font color=red>属性私有(private修饰)，提供get／set接口访问数据 </font>

  private修饰的属性（或方法）不能直接通过 **类名.属性名（或方法名）** 来访问，只能通过接口访问。

  

------



## 继承

- 继承是类和类的一种关系。除此之外，还有依赖，组合，聚合等。
- Java中类只有单继承，没有多继承，但可以多重继承（接口可以多继承）
- 继承关系的两个类，一个为子类（派生类），一个为父类（基类）
- 关键字：<font color=red>extands  和  implements</font>
  - extands：单一继承，一个子类只能拥有一个父类，所以 extends 只能继承一个类。
  -  implements：可以变相的使java具有多继承的特性，使用范围为类继承接口的情况
- 特性：
  - 拥有父类非 private 的属性、方法。
  - 可以拥有自己的属性和方法，即子类可以对父类进行扩展。
  - 可以用自己的方式实现父类的方法。
  - 父类可以通过new实例化一个子类，相反则不行。（A exands B  ；A a = new B（））
  
- 当一个类没有继承，则默认继承object（这个类在 **java.lang** 包中，所以不需要 **import**）祖先类。

- super：实现对父类成员的访问，用来引用当前对象的父类。

  - super（）        调用父类的构造器，必须在子类构造器代码的第一行，
  - super.xx           调用父类的属性
  - super.xx（）   调用父类的方法

- this：指向自己所在类的引用。

- super和this注意点：

  - super和this不能同时调用构造方法

  - super只能在继承条件下使用，this没有继承也可以使用

    

------



## 重写和重载

- 重写(Override)
  - 需要有继承关系，子类重写父类的方法，可用<font color=red>@Override注解</font>进行标记，可对父类中的方法进行检查，如果子类使用的方法父类中没有，就会报错
  - 修饰符：范围可以向上扩大但不能缩小      public>protected>default>private
  - 无法重写父类中声明为<font color=red>private final static</font>的方法
  - 子类和父类在同一个包中，那么子类可以重写父类所有方法，除了声明为 private 和 final 的方法
  - 子类和父类不在同一个包中，那么子类只能够重写父类的声明为 public 和 protected 的非 final 方法
  - 抛出的异常：范围可以缩小但不能扩大
- 重载(Overload) 
  - 在一个类里面，方法名字相同，而参数不同。返回类型可以相同也可以不同。
  - 常用：构造器的重载。
  - 被重载的方法可以改变访问修饰符
  - 方法能够在同一个类中或者在一个子类中被重载。
  - 无法以返回值类型作为重载函数的区分标准。

------



## 多态

多态是同一个行为具有多个不同表现形式或形态的能力。

多态就是同一个接口，使用不同的实例而执行不同操作，如图所示：

![img](E:\Typora\笔记\typora图片复制存储\java-polymorphism-111.png)

- 多态存在的三个必要条件

  - 继承

  - 重写

  - 父类引用指向子类对象：Parent p = new Child();

    ![img](E:\Typora\笔记\typora图片复制存储\2DAC601E-70D8-4B3C-86CC-7E4972FC2466.jpg)

- 多态实现方式

  - 重写
  - 接口
  - 抽象类和抽象方法

- 注意：多态是方法的多态，属性没有多态性

- <font color=red> instanceof </font>：判断一个对象是否为一个类（或接口、抽象类、父类）的实例，返回值为boolean型

------



## 异常

### 异常简介

[Java](http://c.biancheng.net/java/) 中的异常又称为例外，是一个在程序执行期间发生的事件，它可中断程序的执行。

- 异常产生的原因：

  - Java 内部错误发生异常，Java 虚拟机产生的异常
  - 编写的程序代码中的错误所产生的异常，例如空指针异常、数组越界异常等
  - 通过 throw 语句手动生成的异常，一般用来告知该方法的调用者一些必要信息

- 所有的异常类是从 java.lang.Exception 类继承的子类

  在 [Java](http://c.biancheng.net/java/) 代码中只有继承了 Throwable 类的实例才能被 throw 或者 catch

  ![image-20211104231555877](E:\Typora\笔记\typora图片复制存储\image-20211104231555877.png)
  
  

### Error 和 Exception

- Error（错误）：会导致程序非正常终止，无法被开发者捕获

- Exception（异常）：程序正常运行过程中可以预料到的意外情况，并且应该被开发者捕获，进行相应的处理
  - RuntimeException类及其子类：运行时异常
  
    可以捕获，也可以不处理，本质上是由程序逻辑错误引起，应避免
  
  - 编译时异常：受检异常
  
    必须进行捕获处理，否则程序无法编译通过
  
    



### 常见的 Error 和 Exception

- 运行时异常（RuntimeException）：
  - NullPropagation：空指针异常；
  - ClassCastException：类型强制转换异常
  - IllegalArgumentException：传递非法参数异常
  - IndexOutOfBoundsException：下标越界异常
  - NumberFormatException：数字格式异常
- 非运行时异常：
  - ClassNotFoundException：找不到指定 class 的异常
  - IOException：IO 操作异常
- 错误（Error）：
  - NoClassDefFoundError：找不到 class 定义异常
  - StackOverflowError：深递归导致栈被耗尽而抛出的异常
  - OutOfMemoryError：内存溢出异常



### 异常处理机制

- 拋出异常：把生成异常的对象，并把它提交给运行时系统的过程


​       捕获异常：运行时系统在方法的调用栈中查找，直到找到能够处理该类型异常的对象的过程

- 异常处理五个关键字（<font color=red>try	catch	finally	throw	throws</font>）

  - try：捕获异常
  - catch：处理异常
  - finally：不管是否抛出或捕获异常都必定执行的代码（除特殊情况）

    使用场景：执行关闭连接、关闭文件和释放线程的的操作

    特殊情况：1.调用 System.exit 函数； 2.调用 halt 函数；3.守护线程

  - throw：拋出一个具体的异常类型（<font color=red>一般在方法中使用</font>）
  - throws：声明可能会出现的所有异常类型（<font color=red>在一个方法（类）的声明处是使用</font>）

```java
try{
	//可能出现异常的代码，当代码没有出现异常，将跳过catch语块
}catch(异常类型1 e){
  //出现异常后的处理
}catch(异常类型2 e){
  //一个 try 代码块后面跟随多个 catch 代码块的情况就叫多重捕获。
}finally {
  //处理善后工作
}
```



### throws 和 throw 的区别

- throws ：声明一个方法可能抛出的所有异常信息，通常在一个方法（类）的声明处声明

- throw ：拋出一个具体的异常类型，在方法（类）内部通过 throw 声明一个具体的异常信息

- throws 可以不用自己捕获异常，可由系统自动将所有捕获的异常信息抛给上级方法；
- throw 则需要用户自己捕获相关的异常，而后再对其进行相关包装，最后将包装后的异常信息抛出。



### 自定义异常

- 内置异常类无法满足需求
- 自定义异常类需要继承 Exception 类或其子类

​       如：自定义运行时异常类需继承 RuntimeException 类或其子类

- 自定义异常类一般包含两个构造方法：
  - 一个是无参的默认构造方法
  - 另一个构造方法以字符串的形式接收一个定制的异常消息，并将该消息传递给超类的构造方法。

```java
class IntegerRangeException extends Exception {
    public IntegerRangeException() {
        super();
    }
    public IntegerRangeException(String s) {
        super(s);
    }
}
```



# 类型转换

- 自动类型转换

  - 将一个表示数据范围小的数值或者变量赋值给另一个数据范围大的变量

  如：double=10

  - 低 ------------------------------------------------------------>高
  - byte，short ，char -> int -> long -> float -> double
  - 取值范围

| 基本类型 | 字节数 | 位数  | 最大值                 | 最小值     |
| -------- | ------ | ----- | ---------------------- | ---------- |
| byte     | 1byte  | 8bit  | 2^7-1                  | -2^7       |
| short    | 2byte  | 16bit | 2^15-1                 | -2^15      |
| int      | 4byte  | 32bit | 2^31-1                 | -2^31      |
| long     | 8byte  | 64bit | 2^63-1                 | -2^63      |
| float    | 4byte  | 32bit | 3.4028235E38           | 1.4E - 45  |
| double   | 8byte  | 64bit | 1.7976931348623157E308 | 4.9E - 324 |
| char     | 2byte  | 16bit | 2^16 - 1               | 0          |

- 强制类型转换
  - 数值范围大的赋值给数值范围小的
  - 如int k = （int）88.88

- 运算中，不同类型的数据先转化为同一类型，然后进行运算。



------



# Arrays类

- 数组的工具类java.util.Arrays

- 由于数组对象本身并没有什么方法可以供我们调用，但API中提供了一个工具类Arrays供我们使用，从而可以对数据对象进行一些基本的操作，减少重复造轮子

  打印数组元素例子：

  快捷方法：

  ```java
  public class Array01 {
      public static void main(String[] args) {
          int[] a = {1,2,3,4,5,90,25,15631,4512};
          System.out.println(Arrays.toString(a));
      }
  }
  ```

  不使用提供的方法：（需要自己写打印元素的方法，需要自己造轮子）

  ```java
  public class Array01 {
      public static void main(String[] args) {
          int[] a = {1,2,3,4,5,90,25,15631,4512};
         printArray(a);
      }
      public static void printArray(int[] a){
          for(int i= 0;i<a.length;i++){
              System.out.println(a[i]+",");
          }
      }
  }
  ```

  

- Arrays类中的方法都是static修饰的静态方法，在使用的时候可以直接使用类名进行调用，而"不用“使用对象来调用（注意：是”不用“而不是”不能"）

  
  
- 具有以下常用功能：

  - 给数组赋值：通过fill方法
  - 对数组排序：通过sort方法按升序。
  - 比较数组：通过equals方法比较数组中元素值是否相等。
  - 查找数组元素：通过binarySearch方法能对排序好的数组进行二分查找法操作。

- **可查看JDK帮助文档查看所有方法**



------



# 形参与实参

## 形参

在函数定义中出现的参数可以看做是一个占位符，它没有数据，只能等到函数被调用时接收传递进来的数据，所以称为**形式参数**，简称**形参**。

## 实参

函数被调用时给出的参数包含了实实在在的数据，会被函数内部的代码使用，所以称为**实际参数**，简称**实参**。

形参和实参的功能是传递数据，发生函数调用时，实参的值会传递给形参。

## 形参和实参的区别和联系

- 形参被调用时才会分配内存，调用结束立刻释放内存，只在函数内部有效，不能在函数外部使用。
- 实参可以是常量、变量、表达式、函数等，无论实参是何种类型的数据，在进行函数调用时，它们都必须有确定的值，以便把这些值传送给形参。
- 实参和形参在数量上、类型上、顺序上必须严格一致，否则会发生“类型不匹配”的错误。当然，如果能够进行自动类型转换，或者进行了强制类型转换，那么实参类型也可以不同于形参类型。
- 函数调用中发生的数据传递是单向的，只能把实参的值传递给形参，而不能把形参的值反向地传递给实参；换句话说，一旦完成数据的传递，实参和形参就再也没有瓜葛了，所以，在函数调用过程中，形参的值发生改变并不会影响实参。



# 逻辑运算符

- &：逻辑运算符	&&：短路运算符

- &和&&的区别
  - 本质上都是逻辑与
  - && ：当前面的为false时，不会在继续进行判断，直接输出false
  - &：两边都会进行判断



# 三元运算符

- 表达式？表达式1：表达式2

表达式为true，执行1，否则执行2





# 数组

## 数组定义

- 格式1:	数据类型[ ] ：推荐使用

如：int[ ] arr

- 格式2： 数据类型 变量名[ ] 

如：int arr[ ]



## 数组初始化

- 使用 new 指定数组大小后进行初始化

```java
int[] number = new int[5];
```

- 使用 new 指定数组元素的值

```java
int[] number = new int[]{1, 2, 3, 5, 8};
```

- 直接指定数组元素的值

```java
int[] number = {1,2,3,5,8};
```



# String

## String和int的相互转换

- String转换为int
  - Integer.parseInt(str)
  - Integer.valueOf(str).intValue()
- int转换为String
  - String s = String.valueOf(i);
  - String s = Integer.toString(i);   可以把一个引用类型转换为 String 字符串类型
  - String s = "" + i;



## 字符串拼接

- 使用连接运算符“+”

```java
String str="Welcome to"+"Beijing"+"欢迎来到"+"北京。"+"北京我的故乡。";
```

- 使用 concat() 方法

```java
字符串 1.concat(字符串 2);
```



## 获取字符串长度

- str.length（）



## 字符串大小写转换

- toLowerCase()：

```java
字符串名.toLowerCase()    // 将字符串中的字母全部转换为小写，非字母不受影响
```

- toUpperCase() ：

```java
字符串名.toUpperCase()    // 将字符串中的字母全部转换为大写，非字母不受影响
```



## 去除字符串空格（trim）

```java
字符串名.trim()
```



## 字符串截取（subString）

- 按字符截取，而不是按字节

- substring(int beginIndex) ：提取从索引位置开始至结尾处的字符串部分

```java
String str = "我爱 Java 编程";
String result = str.substring(3);
System.out.println(result);    // 输出：Java 编程
```

- substring(int beginIndex，int endIndex) ：从索引位置开始到索引结束的前一个



## 分割字符串（spilt）

- str.split(String sign)

```java
 String Colors = "Red,Black,White,Yellow,Blue";
    String[] arr1 = Colors.split(","); // 不限制元素个数
输出：Red
     Black
     White
     Yellow
     Blue
```

- str.split(String sign,int limit)：limit为限制的个数

```java
str.split(String sign,int limit)
输出：Red
		 Black
	 	 White,Yellow,Blue

  
```



## 字符串替换（replace）

```java
字符串.replace(String oldChar, String newChar)
```

- replaceFirst() 方法

```java
字符串.replaceFirst(String regex, String replacement)
//用于将目标字符串中匹配某正则表达式的第一个子字符串替换成新的字符串
String words = "hello java，hello php";
String newStr = words.replaceFirst("hello","你好 ");
System.out.println(newStr);    // 输出：你好 java,hello php
```

```java
replaceAll() 方法
//将目标字符串中匹配某正则表达式的所有子字符串替换成新的字符串
  String words = "hello java，hello php";
String newStr = words.replaceAll("hello","你好 ");
System.out.println(newStr);    // 输出：你好 java,你好 php
```



## 字符串比较

- equals() 方法

逐个地比较两个字符串的每个字符是否相同。如果两个字符串具有相同的字符和长度，它返回 true，否则返回 false，大小写也会检查。

- qualsIgnoreCase() 方法

和equals作用相同，但不区分大小写。

- compareTo() 方法

```java
str.compareTo(String otherstr);
//用于按字典顺序比较两个字符串的大小，该比较是基于字符串各个字符的 Unicode 值
```



## 字符串查找

- indexOf() 方法

```java
str.indexOf(value)
str.indexOf(value,int fromIndex)
//用于返回字符（串）在指定字符串中首次出现的索引位置，如果能找到，则返回索引值，否则返回 -1
```

```java
String s = "Hello Java";
int size = s.indexOf('v');    // size的结果为8
```

-  lastlndexOf() 方法

```java
str.lastIndexOf(value)
str.lastlndexOf(value, int fromIndex)
//用于返回字符（串）在指定字符串中最后一次出现的索引位置，如果能找到则返回索引值，否则返回 -1
```

- 根据索引查找

​	字符串本质上是字符数组，因此它也有索引，索引从零开始。

```java
字符串名.charAt(索引值)
//charAt() 方法可以在字符串内根据指定的索引查找字符
  String words = "today,monday,sunday";
System.out.println(words.charAt(0));    // 结果：t
System.out.println(words.charAt(1));    // 结果：o
System.out.println(words.charAt(8));    // 结果：n
```



# IO
