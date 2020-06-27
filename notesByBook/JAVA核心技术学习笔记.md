# Core Java Study Notes

![JAVA核心技术](JAVA核心技术学习笔记.assets/JAVA核心技术.jpg)

## 第一章 Java程序设计概述

### 1.2、JAVA"白皮书"的关键术语

1. 简单性
2. 面向对象
3. 分布式
4. 健壮性
5. 安全性
6. 体系结构中立
7. 可移植性
8. 解释型
9. 高性能
10. 多线程
11. 动态性

## 第二章 Java程序设计环境

###### 工具包术语

- JDK: Java开发工具包
- JRE: Java运行环境
- Server JRE: 服务器JRE
- SE: 标准版
- EE: 企业版
- ME: 微型版



###### :star2:**环境变量配置**

JAVA_HOME c:Java\jdk-11.0.2

Path后面加%JAVA_HOME%\bin;



######  命令行

win+r cmd 

javac Welcome.java //将java文件编译为class文件

java Welcome //执行

###### JShell

## 第三章 Java的基本程序设计结构

### 3.1、一个简单的Java应用程序

```java
public class FirstSample{
	public static void main(String[] args){
        System.out.plainln("Hello,World!");
    }
}
```

### 3.2、注释

//单行注释

/* */多行注释

/** */自动生成文档注释 ==4.9详细讲解==

### 3.3、:star2:基本数据类型

4种整型、2种浮点型、1种字符类型、1种boolean型

| 类型    | 存储空间 | 取值范围                                                     |
| ------- | -------- | ------------------------------------------------------------ |
| int     | 4b       | -2147483648~2147483647                                       |
| short   | 2b       | -32768~32767                                                 |
| long    | 8b       | -9223372036854775808~9223372036854775807                     |
| byte    | 1b       | -128~127                                                     |
| float   | 4b       | 大约&plusmn;3.40282347E&plusmn;38F(有效位数为6~7位)          |
| double  | 8b       | 大约&plusmn;1.79769313486231570E&plusmn;308F(有效位数为15位) |
| char    | 2b       | UTF-16编码中的一个**代码单元**                               |
| boolean | 1b       |                                                              |

| 转义序列 | 名称   | Unicode值 |
| -------- | ------ | --------- |
| \b       | 退格   | \u0008    |
| \t       | 制表   | \u0009    |
| \n       | 换行   | \u000a    |
| \r       | 回车   | \u000d    |
| \\"      | 双引号 | \u0022    |
| \\'      | 单引号 | \u0027    |
| \\\      | 反斜杠 | \u005c    |



### 3.4、变量与常量

```java
int i , j; //不建议声明变量时写在一行，降低可读性
```

变量声明尽可能靠近变量第一次使用的地方

#### 3.4.3、常量

关键词为final表示这个变量只能被赋值一次，常量名使用全大写

````java
final double CM_PER_INCH=2.54;
````

可以在一个类的多个方法中使用的常量，类常量

```java
public static final double CM_PER_INCH=2.54;
```

#### 3.4.4、枚举类型

```java
enum Size {SMALL,MEDIUM,LARGE,EXTRA_LARGE};

Size s = Size.MEDIUM;
```



### 3.5、运算符

\+ - * / % 加减乘除和整数求余

#### 3.5.2、数学函数

Math

sqrt(double a) 平方根

pow(double a,double b) a的b次方

floodMod

#### 3.5.3、类型转换

![类型转换关系图](JAVA核心技术学习笔记.assets/类型转换关系图.png)

#### 3.5.6、自增与自减运算

++n与n++

```java
int n = 7;
int m = 7;
int a = 2*++m; 
int b = 2*n++;
```

int a = 2*++m; ==等价于 int a = 2\*(m+1)==

int b = 2*n++; ==等价于 int b = 2\*n+1==

#### 3.5.7、运算符

![逻辑运算符](JAVA核心技术学习笔记.assets/逻辑运算符.png)

& 与 && 的区别：& 和 | 左右两边的式子一定会执行（比较笨），&& 和 || 只要左边的式子能得出结果，右边的式子就不会执行（比较聪明）。

三元运算符

condition ? expression<sub>1</sub> : expression<sub>2</sub>

condition 为true 值为expression<sub>1</sub>反之为expression<sub>2</sub>

#### 3.5.8、位运算符

&:	1 0010 & 1 0000 = 1 0000 两个操作数中位都为1，结果才为1，否则结果为0

|:	1 0010 & 1 0000 = 1 0010 两个位只要有一个为1，那么结果就是1，否则就为0

^:	1111 ^ 0010 = 1101 如果位为0，结果是1，如果位为1，结果是0

~:	~1101 = 0010 两个操作数的位中，相同则结果为0，不同则结果为1

\> \>	 <<

运算符优先级

![](JAVA核心技术学习笔记.assets/运算符优先级.jpg)

### 3.6、字符串

```java
String str1 = "abc";
String str2 = "abc";
String str3 = new String("abc");
```

![](JAVA核心技术学习笔记.assets/字符串的常量池.png)

#### 3.6.2、拼接

字符串间插入

```java
String str = "Hello";
System.out.println(String.join(" /","S","M","L","XL") );
```

输出结果为:S /M /L /XL



JAVA11新增，复制拼接字符串

```java
String str = "Hello";
System.out.println(str.repeat(4));
```

输出结果为：HelloHelloHelloHello

#### 3.6.3、不可变字符串

不可变字符串优点：编译器可以让字符串共享

缺点效率低

#### 3.6.4、字符串是否相等

```java
String str1 = "Hello";
String str2 = "Hello";
String str3 = "hello";
System.out.println(str1.equals(str2));//true
System.out.println(str1.equalsIgnoreCase(str3));//true，不校验大小写区分
System.out.println(str1+""==str2);//false
System.out.println(str1.comparaTo(str2));//0,效率不如equals
```

==str1+""\=\=str2判断这两个字符串是否存在同一个位置==

#### 3.6.6、码点与代码单元

代码点（Code Point）：在 Unicode 代码空间中的一个值，取值 0x0 至 0x10FFFF，代表一个字符。

代码单元（Code Unit）：在具体编码形式中的最小单位。比如 UTF-16 中一个 code unit 为 16 bits，UTF-8 中一个 code unit 为 8 bits。一个 code point 可能由一个或多个 code unit(s) 表示。在 U+10000 之前的 code point 可以由一个 UTF-16 code unit 表示，U+10000 及之后的 code point 要由两个 UTF-16 code units 表示

#### 3.6.7、String API

```java
String greeting = "Hello World!";
greeting.charAt(3);//返回指定位置的代码单元
int i = greeting.codePointAt(4);//返回从给定位置开始的码点
greeting.offsetByCodePoints(1,4);//返回从1码点开始，4个码点之后的码点索引
"sd".compareTo("sd");//按字典顺序比较两个字符串,如果两个字符串内容相同返回0
greeting.codePoints().toArray();//将这个字符串作为一个流返回
"".isEmpty();//如果字符串为空返回true
"  ".isBlank();//如果字符串为空或空格组成返回true
greeting.startsWith("H");//如果字符串以H为开头返回true
greeting.endsWith("o");//如果字符串以o为结尾返回true

//返回与字符串或码点匹配的第一个字串开始的位置
greeting.indexOf("e");
greeting.indexOf("l",5);
greeting.indexOf(111);
greeting.indexOf(111,6);

//返回与字符串或码点匹配的末尾字串开始的位置
greeting.lastIndexOf("e");
greeting.lastIndexOf("l",5);
greeting.lastIndexOf(111);
greeting.lastIndexOf(111,6);

greeting.length();
greeting.codePointCount(1,5);//返回1和4之间的码点个数
greeting.replace("lo","a");//返回新字符串，这个字符串替换lo
greeting.substring(3);
greeting.substring(3,7);//截取字符串
greeting.toLowerCase();//返回一个原字符串全小写字符串
greeting.toUpperCase();//返回一个原字符串全大写字符串
"   Hello   ".trim();//去除收尾空格
"   Hello   ".strip();//去除收尾空格，包括Unicode的空格
```

#### 3.6.9、构建字符串

StringBuilder效率高但线程不安全

StringBuffer效率低线程安全

```java
StringBuilder builder = new StringBuilder();//构造空的构建字符串
builder.append("Hello World");//追加一个字符串并返回一个this
builder.append('c');//追加一个字节并返回一个this
builder.appendCodePoint(97);//追加一个码点并返回一个this
builder.setCharAt(0,'h');//将第0位设置为h
builder.insert(6,'a');//在第6位插入一个字符串并返回this
builder.delete(6,7);//删除偏移量6到6-1的代码单元并返回this
String str = builder.toString();//返回一个与构建器或缓冲器内容相同的字符串
```

### 3.7、输入与输出

```java
Scanner in = new Scanner(System.in);
System.out.println("What is your name");
String name = in.nextLine();
System.out.println("How old are you");
int age = in.nextInt();
System.out.println("Hello, "+name+". Next year , you,ll be" + (age+1));
```

#### 3.7.2、格式化输出

```java
System.out.printf("%+,.2f",-10000.0/3.0);
String name = "William ";
int age = 28;
String message = String.format("Hello,%s. Next year you'll be %d",name,age);
System.out.printf(message);
System.out.printf("%tc", new Date());
System.out.printf("%1$s %2$tB %2$te %2$tY","Due date:",new Date());
System.out.printf("%s %tB %<te %<tY","Due date:",new Date());
```

#### 3.7.3、文件输入与输出

```java
Scanner in = new Scanner(Path.of("C:\\myfile.txt"), StandardCharsets.UTF_8);
PrintWriter out = new PrintWriter("myfile.txt",StandardCharsets.UTF_8);
```



### 3.8、控制流程

#### 3.8.1、块作用域

#### 3.8.2、条件语句

if(choice){}

if(){}else{}

if(){}else if(){}else{}

#### 3.8.3、循环

while(){}

do{}while()

#### 3.8.4、确定循环

for(int i=0;i<=10;i++){}

#### 3.8.5、多重选择: switch

```java
switch(choice){

	case 1:
        ...
        break;
    case 2:
        ...
        break;
    default:
        ...
        break;    
}
```

case标签可以是：

- 类型为char、byte、short或int的常量表达式
- 枚举常量
- 从==Java7==开始，case标签还可以是字符串字面量

#### 3.8.6、中断控制流程语句

break：不执行代码块后面代码，直接退出循环语句

read_data:while(){break read_data;}：带标签的break，用于跳出多重嵌套的循环语句

continue：不执行代码块后面代码，回到循环首部再执行循环

### 3.9、大数

BigInteger：实现任意精度的整数运算

BigDecimal：实现任意精度的浮点数运算

BigInteger方法：

- BigInteger add(BigInteger  other)：求和
- BigInteger subtract(BigInteger  other)：求差
- BigInteger multiply(BigInteger  other)：求积
- BigInteger divide(BigInteger  other)：求商
- BigInteger mod(BigInteger  other)：求余
- BigInteger sqrt(BigInteger  other)：JDK9 平方根
- int compareTo(BigInteger  other)：如果两个大数相等则返回0，小于返回负数，大于返回正数
- static BigInteger valueOf(long x)：返回值等于X的大整数

BigDecimal方法

- BigDecimal add(BigDecimalother)：求和
- BigDecimal subtract(BigDecimalother)：求差
- BigDecimal multiply(BigDecimalother)：求积
- BigDecimal divide(BigDecimalother)：求商 如果商是一个无限循环小数则会抛出一个异常
- BigDecimal divide(BigDecimalother,RoundingMode mode)：RoundingMode为枚举，求上根据mode舍入
- int compareTo(BigInteger  other)：如果两个大数相等则返回0，小于返回负数，大于返回正数
- static BigInteger valueOf(long x)：返回值等于x的大整数
- static BigInteger valueOf(long x,int scale)：返回值等于x/10<sup>scale</sup>的大整数

### 3.10、数组

#### 3.10.1、声明数组

一旦创建数组就不能再改变它的大小

int[] a;	声明数组

int[] a = new int[100];	初始化数组

int[] a = {2,3,4,5,1};	初始化数组简写形式

new int[]{2,3,4,5,1};	匿名数组

int[] a = new int[]{{2,3,4,5,1}} 	初始化数组并直接设置数组大小

int[] b = {2,3,4,5,1};	与上面等价

#### 3.10.2、访问数组元素

创建一个数组时，所有元素都初始化为0。boolean数组的元素会初始化为false，对象数组的元素则初始化为一个特殊值null

#### 3.10.3、for each 循环

增强for循环语句

for(variable : collection) statement

#### 3.10.4、数组拷贝

```java
        int[] a = {1,2,3};
        int[] b = {1,2,3};

        a = b;
        System.out.println(a == b);
        System.out.println(a);
        System.out.println(b);

        int[] a1 = {1,2,3};
        int[] b1 = {1,2,3};

        a1 = Arrays.copyOf(b1,b1.length);
        System.arraycopy(b1, 0, a1, 0, b1.length);
        System.out.println(a1 == b1);
        System.out.println(a1);
        System.out.println(b1);
```

a  =  b指向相同的内存地址

a1 = b1 指向不同的内存地址

#### 3.10.5、命令行参数

3.10.6、数组排序

viod Arrays.sort(xxx[] a): 源码为双枢轴快速排序，不改变内存路径

Arrays API

static String toString(xxx[] a)：返回包含a中元素的一个字符串，a只能是基本数据类型数组

static xxx[] copyOf(xxx[] a,int end)：返回与a类型相同数组，范围为0~end，如果end超过a.length，则会填充默认值

static xxx[] copyOfRange(xxx[] a,int start,int end)：返回与a类型相同数组，范围为start~end，如果end超过a.length，则会填充默认值

static void sort(xxx[] a)：快速排序

static int binarySearch(xxx[] a,xxx v)：

static int binarySearch(xxx[] a,int start,int end,xxx v)：使用二分法查找在有序数组a中查找值v，如果找到v则返回相应下标，否则返回一个负数r，-r-1是v应插入的位置

static void full(xxx[] a,xxx v)：将数组的所有元素都设置为v

static boolean equals(xxx[] a,xxx[] b)：如果两个数组大小相同，并且下标相同的元素都对应相等，返回true

#### 3.10.7、多维数组

```java
double[][] balances;
```

#### 3.10.8、不规则数组

## 第四章 对象与类

### 4.1、面向对象程序设计概述

面向对象程序设计(object-oriented programming,OOP)

#### 4.1.1、类

类：构造对象的模板或蓝图

创建类的实例：由类构造的对象的过程为创建类的实例

封装(或称为数据隐藏)：将数据和行为组合在一个包中，并对对象的使用者隐藏具体实现方式

实例字段：对象中的数据称为实例字段

方法：操作数据的过程为方法

对象的状态：特定对象都有一组特定的实例字段值，这些值的集合就是这个对象的当前状态

无论何时，只要在对象上调用一个方法，他的状态就有可能发生改变

继承：通过扩展一个类来建立另外一个类的过程称为继承

#### 4.1.2 、对象

对象的三个主要特征：

- 对象的行为：可以对对象完成那些操作，或者可以对对象应用哪些方法
- 对象的状态：当调用那些方法时，对象会如何响应
- 对象的标识：如何区分具有相同行为与状态的不同对象

#### 4.1.3、识别类

#### 4.1.4、类之间的关系

常见关系有：

- 依赖（“uses-a”）
- 聚合（“has-a”）
- 继承（“is-a”）

### 4.2、使用预定类

#### 4.2.1、对象与对象变量

想要使用对象，首先必须构造对象，并指定其初始状态，然后对对象应用方法

#### 4.2.2、Java类库中的localDate类

- static LocalDate now()：构造一个表示当前日期的对象
- static LocalDate of(int year,int month,int day)：构造一个表示给定日期的对象
- int getYear()
- int getMonthValue()
- int getDayofMonth()：得到当前日期的年、月、日
- DayOfWeek getDayOfWeek：得到当前日期是星期几，作为一个DayOfWeek的实例返回，调用getValue来得到1~7之间的一个数，表示这是星期几，1表示星期一，7表示星期日
- LocalDate plusDays(int n)
- LocalDate miusDays(int n)：生成当前日期之后或之前n天的日期

#### 4.2.3、更改器方法与访问器方法

更改器方法：改变对象的状态的方法，例如set方法

访问器方法：只访问对象而不修改对象的方法，例如get方法

### 4.3、用户自定义类

#### 4.3.1、Employee类

#### 4.3.2、多个源文件的使用

javac Employee*.java

javac EmployeeTest.java：如果Employee被调用，编译器会自动编译Employee，反之则不会编译

#### 4.3.3、剖析Employee类

==强烈建议将实例字段标记为private==

#### 4.3.4、从构造器开始

在构造Employee类的对象时，构造器会运行，从而将实例字段初始化为所希望的初始状态

构造器：

- 构造器与类名相同
- 每个类有一个或多个参数
- 构造器可以有0个、1个或多个参数
- 构造器没有返回值
- 构造器总伴随着new操作符一起调用

#### 4.3.5、用var声明局部变量

在==Java10==中

```java
Emloyee harry = new Employee("Herry Hacker",50000,1989,10,1);
var harry = new Employee("Herry Hacker",50000,1989,10,1);
```

var关键字只能用于方法中的局部变量。参数和字段的类型必须声明

#### 4.3.6、使用null引用

==Java9==中Objects类提供的null判断方法：

- static <T\> T requireNonNull(T obj,String message)：检查指定的对象引用是否为null ，如果是，则抛出自定义的NullPointerException。
- static <T\>T requireNotNullElse(T obj,T defaultObj)：如果它是非 null ，则返回第一个参数，否则返回非 null第二个参数。 

#### 4.3.7、隐式参数与显式参数

```java
public void raiseSalary(double byPercent){
    double raise = salary * byPercent / 100;
    salary += raise;
}
```

显式参数：又称为方法调用的目标或接收者，位于方法名后面的括号中，上面byPercent就是显式参数

隐式参数：是出现方法名前的对象，上面salary就是显式参数

#### 4.3.8、封装的优点

```java
public String getName(){
    return name;
}
public double getSalary(){
    return salary;
}
public LocalDate getHireDay(){
    return hireDay
}
```

字段访问器：只返回实例字段值的方法

想要获取或设置实例字段的值，需要提供三项内容：

- 一个私有的数据字段
- 一个公共的字段访问器方法
- 一个公共的字段修改器方法

==不要编写返回可变对象引用的访问器方法==

```java
class Employee{
    private Date hireDay;
    public Date getHireDay(){
        return hireDay;
    }
}
```

由于Date是可变的，harry和d引用的是同一对象，所以这样写破坏了封装性

```java
Employee2 harry = new Employee2();
Date d = harry.getHireDay();
double tenYears = 10*365.25*24*60*60*1000;
Date date = d.setTime(d.getTime() - (long)tenYears);
```

直接导致在对d做修改时会同时修改harry的值

```java
class Employee{
    private Date hireDay;
    public Date getHireDay(){
        return (Date)hireDay.clone();
    }
}
```

解决方案为克隆，对象克隆是指存放在另一个新位置上的对象副本

#### 4.3.9、基于类的访问权限

一个方法可以访问所属类的所有对象的私有数据

#### 4.3.10、私有方法

只要方法是私有的，类的设计者就可以确信它不会再别处使用，所以可以将其删除，而不影响其他代码

#### 4.3.11、final实例字段

final修饰的字段必须在构造对象时初始化

final修饰符常用于修饰基本类型或者不可变类的字段

```java
class Employee{
    private final String name;
}
```



### 4.4静态方法

#### 4.4.1、静态字段

静态字段又称为类字段，如果将一个字段定义为static，每个类只有一个这样的字段

```java
class Employee{
    private static int nextId = 1;
    private int id;
}
```

#### 4.4.2、静态常量

```java
public static final double PI = 3.141592653589323846
```

由于每个类对象都可以修改公共字段，所以，最好不要有公共字段。然而，公共常量(final)可以声明为公共，因为它不能被重新赋值

#### 4.4.3、静态方法

静态方法是不在对象上执行的方法

```java
public static int getNextId(){
    retrun nextId;
}
int n = Employee.getNextId();
```

可以使用Employee对象调用静态方法，但不建议这样做，因为会与非静态方法混淆

两种情况下可以使用静态方法

- 方法不需要访问对象状态，因为它需要的所有参数都是通过显示参数提供(例如：Math.pow)
- 方法只需要访问类的静态字段(例如：Employee.getNextId)

#### 4.4.4、工厂方法

#### 4.4.5、main方法

static <T\> void requireNonNull(T obj)

static <T\> void requireNonNull(T obj,String message)

static <T\> void requireNonNull(T obj,Supplier<String\> messageSupplier)：java8

如果obj为null，这些方法会抛出一个NullPointerException异常而没有消息或者给定的消息

static <T\> T requireNonNullElse(T obj,T defaultObj)

static <T\> T requireNonNullElseGet(T obj,Supplier<T\> defaultSupplier)

如果obj不为null则返回obj，或者如果obj为null则返回默认对象

### 4.5、方法参数

按值调用：表示方法接受的是调用者提供的值

按引用调用：表示方法接收的是调用者提供的变量地址

==Java程序设计语言总是采用按值调用==

Java中对方法参数能做什么和不能做什么：

- 方法不能修改基本数据类型的参数(即数值型或布尔值)
- 方法可以改变对象参数的状态
- 方法不能让一个对象参数引用一个新的对象

### 4.6、对象构造

#### 4.6.1、重载

重载：多个方法，有相同的名字、不同的参数。

#### 4.6.2、默认字段初始化

如果构造器中没有显式地为字段设置初始值，那么就会被自动地赋为默认值：数值为0、布尔值为false、对象引用为null，即使想要的值为默认值也应初始化字段

#### 4.6.3、无参数的构造器

public Employee(){

​	name = "";

​	salary = 0;

​	hireDay = LocalDate.now();

}

如果写一个类时没有编写构造器，就会为你提供一个无参数构造器。这个构造器将所有的实例字段设置为默认值。

当类没有其他任何构造器时，会有一个默认的无参构造器。如果你已经写了一个有参构造器，那么如果你想要构造一个无参对象，则必须写一个无参构造器

#### 4.6.4、显式字段初始化

一般常量static final 声明字段在声明时直接初始化

其他字段建议在构造器中初始化

比如类Demo有个Map类型成员变量，如果直接赋值，那么就必须要指定这个Map是何种Map,而通过构造函数赋值，这个就不确定了，有可能是各种Map的实现。所以，通过构造函数与set方法赋值，能够使程序更加的灵活，也能够体现多态的面向对象的特征。

==有点看不懂回头再看==<https://coolshell.cn/articles/1106.html>

#### 4.6.5、参数名

#### 4.6.6、调用另一个构造器

如果构造器的第一个语句形如this(...)，这个构造器将调用同一个类的另一个构造器

```java
public Employee(double s){
    this("Employee #" + nextId, s);
    nextId++;
}
```

#### 4.6.7、初始化块

初始化数据三种方法：

- 在构造器中设置值
- 在声明中赋值
- 在初始化块中赋值

初始化块：只要构造这个类的对象，这些块就会被执行

在一个类声明中，可以包含任意多个代码块

为了避免字段重复定义，建议总是将初始化块放在字段定义之后

Random()：构造一个新的随机数生成器

int nextInt(int i)：返回一个0~n-1之间的随机数

#### 4.6.8、对象析构与finalize方法

### 4.7、包

#### 4.7.1、包名

使用包的主要原因是确保类名的唯一性

#### 4.7.2、类的导入

一个类可以使用所属包中的所有类，以及其他包中的公共类(public class)

两种方式访问一个包中的公共类：

- 完全限定名：java.time.LocalDate today = java.time.LocalDate.now();
- import语句

#### 4.7.3、静态导入

静态导入允许导入静态方法和静态字段，而不只是类

```java
import static java.lang.StrictMath.random;
random();
```

#### 4.7.4、在包中增加类

想要将类放入包中，就必须将包的名字放在源文件的开头，即放在定义这个包中各个类的代码之前

如果没有在源文件中放置package语句，这个源文件中的类就属于无名包

编译器不检查目录结构，虚拟机需要通过目录找到类

#### 4.7.5、包访问

public：任意类使用

private：只能由它们定义的类使用

(defaul)：同一个包下的类使用

 protected：子类使用

#### 4.7.6、类路径

#### 4.7.7、设置类路径

### 4.8、JAR文件

#### 4.8.1、创建JAR文件

#### 4.8.2、清单文件

#### 4.8.3、可执行JAR文件

#### 4.8.4、多版本JAR文件

#### 4.8.5、关于命令行选项的说明

### 4.9、文档注释

#### 4.9.1、注释的插入

#### 4.9.2、类注释

#### 4.9.3、方法注释

- @param variable description：参数
- @return description：返回
- @throws class description：可能抛出的异常

#### 4.9.4、字段注释

#### 4.9.5、通用注释

- @since text：始于，text可以是引入这个特性的版本的任何描述，@since 1.7.1
- @author name：作者
- @version text：版本
- @see reference：参见
- @Link：链接
- @index：搜索框条目

#### 4.9.6、包注释

#### 4.9.7、注释抽取

### 4.10、类设计技巧

1. 一定要保证数据私有

2. 一定要对数据进行初始化

3. 不要在类中使用过多的基本数据类型

4. 不是所有的字段都需要单独的字段访问器和字段更改器

5. 分解有过多职责的类

6. 类名和方法名要能够体现它们的职责：

   类名应当是一个名词(Order)，或者是前面有形容词修饰的名词(RushOrder)，或者是有动名词(有"-ing"后缀)修饰的名词

   访问器方法用小写的get开头

   更改器方法用小写的set开头

7. 优先使用不可变得类

   如果多个线程试图同时更新一个对象，就会发生并发更改

## 第五章、继承

继承的基本思想是，可以基于已有的类创建新的类

反射是指在程序运行期间更多地了解类及其属性的能力

### 5.1、类、超类、和子类

#### 5.1.1、定义子类

关键字**extends**表明正在构造的新类派生于一个已存在的类

这个已存在的类称为***超类、基类或父类***

新类称为***子类或孩子类***

子类中没有显式地定义超类的方法，但是可以对子类的对象使用这些方法，这是因为子类类自动地继承了超类中的方法

应该将最一般的方法放在超类中，而将更特殊的方式放在子类中

#### 5.1.2、覆盖方法

超类的方法并不一定适用于子类，为此需要一个新的方法***覆盖***超类中的方法

子类已经覆盖了超类的方法，但是又需要调用超类的方法，这时候需要使用super关键字

```java
super.getSalary;
```

在子类中可以增加字段、增加方法或覆盖超类的方法，不过继承绝对不会删除任何字段或方法

#### 5.1.3、子类构造器

多态：一个对象变量可以指示多种实际类型的现象称为多态

动态绑定：在运行时能够自动选择适当的方法称为动态绑定

#### 5.1.4、继承层次

继承层次：由一个公共超类派生出来的集合称为继承层次

继承链：从某个特定的类到其祖先的路径称为该类的继承链

#### 5.1.5、多态

替换原则：程序中出现超类对象的任何地方都可以使用子类的对象替换

对象变量是多态的，一个超类的变量既可以引用一个超类的对象，也可以引用任何一个子类的对象

#### 5.1.6、理解方法调用

假设要调用.f(args)，x声明为C类的一个对象，调用的详细描述：

1. 编译器查看对象的声明类型和方法名

2. 编译器确定方法调用中提供的参数类型。如果在所有名为f的方法中存在一个与所提参数类型完全匹配的方法，就选这个方法，这个过程称为***重载解析***

   签名：方法的名字和参数列表称为方法的签名

   子类方法覆盖超类方法并修改返回类型，则称为这两个方法有***可协变***的返回类型

3. 如果是private方法、static方法、final方法或者构造器，那么编译器将可以准确地知道应该调用哪个方法。这个称为***静态绑定***

4. 程序运行并且采用动态绑定调用方法时，虚拟机必须调用与x所引用对象的实际类型对应的那个方法

调用e.getSalary()的解析过程为：

1. 首先，虚拟机获取e的实际类型的方法表
2. 之后，虚拟机查找定义了getSalary签名的类
3. 最后，虚拟机调用这个方法

动态绑定重要特性：无序对现有的代码进行修改就可以对程序进行扩展

在覆盖方法的时候，子类的方法不能低于超类方法的可见性。

#### 5.1.7、阻止继承：final类和方法

不允许扩展的类被称为final类

类中的某个方法也可以被声明为final。如果这样做，子类就不能覆盖这个方法，final类中的所有方法自动地成为final方法，但字段不会被声明为final

如果一个方法没有被覆盖并且很短，编译器就能够对它进行优化处理，这个过程称为***内联***，比如说e.getName()，编译器会将其替换为e.name

#### 5.1.8、强制类型转换

将一个类型强制转换成另一个类型的过程称为强制类型转换

强制类型转换的作用是，要在暂时忽视对象的实际类型之后使用对象的全部功能

instanceof关键字：判断一个对象是否为这个类的实例

```java
Student s = new Student;
s instanceof Student //true
```

强制类型转换使用注意事项：

- 只能在继承层次内进行强制类型转换
- 在将超类强制类型转换为子类之前，应该使用instanceof进行检查

注意：

```java
s = null;
s instanceof Student//false
```

如果s为null，判断不会发生异常，因为null没有引用任何对象

尽量少使用强制类型转换和instanceof关键字

#### 5.1.9、抽象类

父类中的方法，被它的子类们重写，子类各自的实现都不尽相同。那么父类的方法声明和方法主体，只有声明还有意义，而方法主体则没有存在的意义了。我们把没有方法主体的方法称为抽象方法，用abstract关键字声明抽象类。Java语法规定，包含抽象方法的类就是抽象类。

定义：

1. 抽象方法：没有方法体的方法。
2. 抽象类：包含抽象方法的类。

```java
public abstract class Person{
    public abstract String getDescription();
}
```

有抽象方法的类必须声明为抽象类，但是即便不含抽象方法的类，也可以声明抽象类

超类为抽象类的子类，声明时必须重写父类方法

抽象类不能实例化

#### 5.1.10、受保护访问

1. private：仅对本类可见
2. public：对外部完全可见
3. protected：对本包和所有子类可见
4. 默认：对本包可用

### 5.2、Object：所有类的超类

Object类是Java所有类的始祖

如果没有明确地指出超类，Object就被认为是这个类的超类

#### 5.2.1、Object类型的变量

Java当中，只有基本数据类型不是对象

所有数组类型，不管对象数组还是基本数据类型的数组都扩展了Object类

#### 5.2.2、equals方法

Object类中的equals方法用于检测一个对象是否等于另一个对象

#### 5.2.3、相等测试与继承

Java语言规范要求equals方法具有的特性：

1. 自反性：对于任何非空引用x，x.equals(x)应该返回true
2. 对称性：对于任何引用x和y，当且仅当y.equals(x)返回true时，x.equals(y)返回true
3. 传递性：对于任何引用x，y和z，如果x.equals(y)返回true，y.equals(z)返回true，x.equals(z)也应该返回true。
4. 一致性：如果x和y引用的对象没有发生变化，反复调用x.equals(y)应该返回同样的结果。
5. 对于任意非空引用x，x.equals(null)应该返回false

```JAVA
public class Empolyee{
    public boolean equals(Object otherObject){
        //a quick test to see if the Objects are identical
        if(this == otherObject) return true;
        //must return false if the explicit paramenter is null
        if(otherObject == null) return false;
        //if the classes don`t match,they can`t equal
        if(getClass() != otherObject.getClass())
            return false;
        //now we know otherObject is a non-null Employee
        Employee other = (Employee)otherObject;
        //test whether the fields have identical values
        return name.equals(other.name)
            && salary == other.salary 
            && hireDay.equals(other.hireDay);
    }
}
```

编写一个完美的equals方法的建议：

1. 显式参数命名为otherObject，稍后需要将他强制转换成另一个名为other的变量

2. 检测this与otherObject是否相等：意思是检测otherObject是否为该对象本身

   if(this == otherObject) return true;

3. 检测otherObject是否为null

   if(otherObject == null) return false;

4. 比较this与otherObject的类：

   - 如果equals的语义可以在子类中改变，就使用getClass检测

     if(getClass() != otherObject.getClass()) return false;

   - 如果所有子类都有相同的相等性语义，可以使用instanceof检测

     if(!(otherObject instanceof ClassName)) return false;

5. 将otherObject强制转换为相应类类型的变量

   Employee other = (Employee)otherObject;

6. 根据相等性概念的要求改比较字段。使用 == 比较基本类型字段。使用Objects.equals比较对象字段。

   如果所有字段都匹配，就返回true，否则返回false

   return name.equals(other.name)
               && salary == other.salary 
               && hireDay.equals(other.hireDay);

   如果在子类中重新定义equals，就要在其中包含一个super.equals(other)调用



可以使用@Override标记要覆盖超类方法的那些子类方法，如果签名不一致就会报错

java.util.Arrays:

- static boolean equals(xxx[] a,xxx[] b) ==5==：如果两个数组长度相同，并且在对应的位置上数据元素也相同，将返回true

java.util.Objects

- static boolean equals(Object a,Object b) ==7== ：如果a和b都为null，返回true；如果只有其中之一为null，返回false；否则返回a.equals(b)

#### 5.2.4、hashCode方法

散列码(hash code)是由对象导出的一个整型值

String类使用计算散列码：

```java
int hash = 0;
for(int i = 0;i < lenth(); i++){
    hash =31 * hash + charAt(i);
}
```

如果重新定义了equals方法，就必须为用户可能插入散列表的对象重新定义hashcode方法

equals与hashCode的定义必须相容：如果x.equals(y) 返回true，那么x.hashCode()就必须与y.hashCode()返回相同的值

java.lang.Object:

- int hashCode()：返回对象的散列码。散列码可以是任意的整数，包括正数或负数。两个相等的对象要求返回相等的散列码

java.util.Objects: ==7==

- static int hash(Object... objects)：返回一个散列码，由提供的所对象的散列码组合而得到
- static int hashCode(Object a)：如果a为null返回0，否则返回a.hashCode()

java.lang.(Integer|Long|Short|Byte|Double|Float|Character|Boolean)

- static int hashCode(xxx value)：==8==返回给定制的散列码。这里xxx是对应给定包装类型的基本类型。

java.util.Arrays

- static int hashCode(xxx[] a)：==5==计算数组a的散列码。组成这个数组的元素类型xxx可以是Object、int、long、short、char、byte、boolean、float或double

#### 5.2.5、toString方法

toString方法：返回表示对象值的一个字符串

java.lang.Object:

- Class getClass()：返回包含对象信息的类对象
- boolean equals(Object otherObject)：比较两个对象是否相等，如果两个对象指向同一块存储取余，返回true，否则返回false，要在自定义的类中覆盖这个方法
- String toString()：返回表示该对象值的字符串，要在自定义的类中覆盖这个方法

java.lang.Class：

- String getName()：返回这个类的名字
- Class getSuperClass()：以Class对象的形式返回这个类的超类

### 5.3、泛型数组列表

ArrayList是一个有类型参数的泛型类

#### 5.3.1、声明数组列表

java.util.ArrayList<E\> 

- ArrayList<E\>()：构造一个空数组列表
- ArrayList<E\>(int initialCapacity)
- boolean add(E obj)：一旦加了一个元素，数组列表就会将容量定为10
- int size()
- void ensureCapacity(int capacity)：确保数组列表在不重新分配内部存储数组的情况下有足够的容量存储给定数量的元素
- void trimTosize()：可将数组列表的存储容量削减到当前大小

#### 5.3.2、访问数组列表元素

java.util.Arrays<E\>

- E set(int index,E obj)
- E get(int index)
- void add(int index,E obj)
- E remove(int index)

#### 5.3.3、类型化与原始数组列表的兼容性

声明ArrayList时，如果没有声明类型参数，则默认为Object

```java
public class EmployeeDB{
    public void update(ArrayList list){}
    public ArrayList find(String query){}
}
```

```java
ArrayList<EmployeeDB> staff = ...;
employee.update(staff);
```

这时不需要将staff强制转换，但是依然是有风险的，update方法有可能无法处理EmployeeDB类型

```java
ArrayList<Employee> result = employeeDB.find(query);
```

这时返回的数组列表的类型参数是未知的，如果返回类型无法强转为Employee则会报java.lang.ClassCastException强制转换异常错误

```java
@SuppressWarnings("unchecked")
```

用上述注解，可以标记接受强制类型转换的变量，暂时忽略编译器的警告

### 5.4、对象包装器与自动装箱

包装器：将基本类型转换为对象的类称为包装器

ArrayList的类型参数不允许是基本数据类型

ArrayList<Integer\>效率远低于int[]，所以除非必须要ArrayList，否则用int[]

自动装箱：调用list.add(3)编译器会自动变换为list.add(Integer.valueOf(3))，这种变换称为自动装箱

自动拆箱：int n = list.get(i) ==> int n = list,get(i).intValue();

尽量避免频繁自动装拆箱，虽然自动拆装箱是编译器的工作，但是频繁的创建对象会占用虚拟机内存

java.lang.Integer：

- int inValue
- static String toString(int i)
- static String toString(int i,int radix)：返回数值i基于radix参数指定进制的表示
- static int parseInt(String s)
- static int parseInt(String s,int radix)：返回字符串s表示整数，采用radix参数指定的进制
- static Integer valueOf(String s)
- static Integer valueOf(String,int radix)

java.text.NumberFormat

- Number parse(String s)：假设s表示的是一个数值，返回数字值

### 5.5、参数数量可变的方法

变参方法：可以提供参数数量可变的方法

```java
public static double max(double... values){
    double largest = Double.NeGTIVE_INFNITY;
    for(double v:values){
        if(v>largest){
            largest = v;
        }
    }
    return largest;
}
```

### 5.6、枚举类

```java
public enum Size{SMALL,MEDIUM,LARGE,EXTRA_LARGE}
```

可以为枚举类型增加构造器、方法和字段

枚举的构造器总是私有的，可以省略private修饰符

### 5.7、==反射==

反射：能够分享类能力的程序称为反射

反射使用场景：

- 在运行时分析类的能力
- 在运行时检查对象
- 实现泛型数组操作代码
- 利用Method对象

#### 5.7.1、Class类

Java运行时系统始终为所有对象维护一个运行时的类型标识，保存这些类型标识的类名为Class

获得Class类对象的三种方法：

```java
var generator = new Random();
Class c1 = generator.getClass();

String className = "java.util.Random";
Class c2 = Class.forName(className);

Class c4 = Random.class;
```

虚拟机为每个类型管理一个唯一的Class对象。因此，可以利用 == 运算符实现两个类对象的比较

```java
var c1 = Class.forName("java.util.Random");
Object obj = c1.getConstructor().newInstance();
```

Class类型对象，调用getConstructor方法获得Constructor类型对象，再调用newInstance方法来构造一个实例

java.lang.Class

- string getName()：返回这个类的类名及包名

- static Class forName(String className)：返回一个Class对象，表示名为className的类
- Constructor getConstructor(Class... parameterType)：生成一个对象，描述有指定参数类型的构造器

java.lang.reflect.Constructor

- Object newInstance(Object... params)：将params传递到构造器，来构造这个构造器声明类的一个新实例

java.lang.Throwable

- void printStackTrace()：将Throwable对象和堆栈轨迹打印到标准错误流

#### 5.7.2、声明异常入门

处理器可以捕获异常并进行处理

检查型异常：建议其将会检查你是否知道这个异常并做好准备来处理后果

非检查型异常：编译器并不期望你为这些异常提供处理器

#### 5.7.3、资源

java.lang.Class

- URL getResource(String name)

- InputStream getResourceAsStream(String name)

  找到与类位于同一位置的资源，返回一个可以用来加载资源的URL或输入流

#### 5.7.4、利用反射分析类的能力

java.lang.Class：

- Field[] getFields()：返回一个包含Field对象的数组，这些对象对应这个类或其他超类的公共字段

- Field[] getDeclaredFields()：只返回当前类的公共字段，不返回超类的公共字段

- Method[] getMethods()：返回包含Method对象的数组，包含该类及继承自超类的所有公共方法

- Method[] getDeclaredMethods()

- Constructor[] getConstructors()：返回包含Constructor对象的数组，包含该类及继承自超类的所有公共构造器

- Constructor[] getDeclaredConstructors();

- String getPackageName()：==9==得到包含这个类型的包的包名，如果这个类型是一个数组类型，则返回元素类型所属的包，或者如果这个类型是一个基本数据类型，则返回“java.lang”

java.lang.reflect.Field

java.lang.reflect.Method

java.lang.reflect.Constructor

- Class getDeclaringClass()：返回一个Class对象，表示定义了这个构造器、方法或字段的类

- Class getExceptionType()(在Constructor和Method classes类中)

  返回一个Class对象数组，其中各个对象表示这个方法所抛出的异常的类型

- int getModifiers()：返回一个整数，描述这个构造器、方法或字段的修饰符。使用Modifier类中的方法来分析这个返回值

- String getName()

- Class[] getParameterTypes()(在Constructor和Method classes类中)

  返回一个Class对象数组，其中各个对象表示参数的类型

- Class getReturnType()(在Method类中)

java.lang.reflect.Modifier

- static String toString(int modifiers)：返回一个字符串，包含对应modifiers中位设置的修饰符

- static boolean isAbstract(int modifiers)
- static boolean isAbstract(int modifiers)
- static boolean isFinal(int modifiers)
- static boolean isInterface(int modifiers)
- static boolean isNative(int modifiers)
- static boolean isPrivate(int modifiers)
- static boolean isProtected(int modifiers)
- static boolean isPublic(int modifiers)
- static boolean isStatic(int modifiers)
- static boolean isStrict(int modifiers)
- static boolean isSynchronized(int modifiers)
- static boolean isVolatile(int modifiers)

这些方法将检测modifiers值中与方法明中修饰符对应的二进制位

#### 5.7.5、使用反射在运行时分析对象

利用反射机制可以查看在编译时还不知道的对象字段

java.lang.reflect.AccessibleObject

- void setAccessible(boolean flag)：设置或取消这个可访问对象的可访问标志，如果拒绝访问则抛出一个IllegalAccessException异常
- boolean trySetAccessible()：==9== 为这个可访问的对象设置可访问标志，如果拒绝则返回false
- boolean isAccessible()：得到这个可访问对象的可访问标志值
- static void setAccessible(AccessibeObject[] array,boolean flag)：设置一个对象数组的可访问标志

java.lang.Class

- Field getField(String name)：得到指定名的公共字段
- Field[] getFields()：所有这些字段的一个数组
- Field getDeclaredField(String name)
- Fields getDeclaredFields()

java.lang.reflect.Field

- Object get(Object obj)：返回obj对象中用这个Field对象描述的字段的值
- void set(Object obj,Object newValue)：将Obj对象中这个Field对象描述的字段设置为一个新值

#### 5.7.6、使用反射编写泛型数组代码

java.lang.reflect.Array

- static Object get(Object array,int index)

- static xxx getXxx(Object array,int index)

  (xxx 是boolean、byte、char、double、float、int、long、short中的一种基本类型)这些方法将返回存储给定数组中给定索引位置上的值

- static void set(Object array,int index,xxx newValue)

- static void setXxx(Object array,int index,xxx newValue)

#### 5.7.7、调用任意方法和构造器

java.lang.reflect.Method

- public Object invoke(Object implicitParamenter,Object[] explicitParamenter)

  调用这个对象描述的方法，传入给定参数，并返回方法的返回值。对于静态方法，传入null作为隐式参数。使用包装器传递基本类型值。基本类型的返回值必须是未包装的

### 5.8、继承的设计技巧

1. 将公共操作和字段放在超类中
2. 不要使用受保护的字段
3. 使用继承实现“is-a”关系
4. 除非所有继承的方法都有意义，否则不要使用继承
5. 在覆盖方法时，不要改变预期的行为
6. 使用多台，而不要使用类型信息
7. 不要滥用反射

## 第六章、接口、lambda表达式与内部类

### 6.1、接口

接口：接口用来描述类应该做什么，而不是指定他们应该具体做什么

一个类可以实现一个或多个接口

#### 6.1.1、接口的概念

```java
class Employee implements Comparable<Employee>{
    public int compareTo(Employee other){
        return Double.compare(salary,other.salary);
    }
}

Arrays.sort(new Employee[3]);
```

想要调用Arrays.sort方法，显式参数类型必须实现接口Comparable，因为Arrays.sort必须调用compareTo方法

接口不是类，而是对希望符合这个接口的类的一组需求

接口中的所有方法都自动是public方法，因此，在接口中声明方法时，不必提供关键字public

接口绝不会有实例字段

让类实现接口步骤：

1. 将类声明为实现给定的接口，关键字为implements
2. 对接口中的所有方法提供定义

实现接口时，必须把方法声明为public

实现接口的主要原因是，Java是一种强类型语言，在调用方法的时候，编译器要能检查这个方法确实存在

java.lang.Comparable<T\>

- int copareTo(T other)：用这个对象与other进行比较，小于other返回负整数，等于返回0，大于返回正整数

java.util.Arrays

- static void sort(Object[] a)：对数组a中的元素排序，a中元素必须实现Comparable接口，并且元素之间必须可比较

java.Iang.Integer

- static int compare(int x,int y)：x<y返回负整数，x=y返回0，x>y返回正整数

java.lang.Double

- static int compate(double x,double y)

#### 6.1.2、接口的属性

接口不是类，不能通过new实例化接口

接口变量必须应用实现了这个接口的类对象

可以使用instanceof检查一个对象是否实现了某个特定的接口

接口中不能包含实例字段，但可以包含常量

接口中的方法都自动被设置为public一样，接口中的字段总是public static final

#### 6.1.3、接口与抽象类

每个类只能扩展一个类，但是每个类可以实现多个接口

#### 6.1.4、静态和私有方法

允许在接口中增加静态方法

#### 6.1.5、默认方法

可以为接口方法提供一个默认实现，必须用default修饰符标记这样一个方法

```java
public interface Iterator<E>{
    boolean hashNext();
    E next();
    defualt void remove(){
        throw new UnsupportedOperationException("remove");
    }
}
```

默认方法的一个重要用法是“接口演化”，如果接口增加了一个方法，只能通过默认方法来扩展，否则不能保证“源代码兼容”

#### 6.1.6、解决默认方法冲突

1. 如果这个类的超类与接口定义了同样签名的方法，超类优先

   

2. 如果这个类的两个接口中，其中一个接口定义了默认方法，另一个接口定义了同样签名的方法（不管这个方法是不是默认方法），这个类必须覆盖这个方法。但是当这两个接口都没有为共享方法提供默认实现，就不会引起冲突

#### 6.1.7、接口与回调

回调是一种常见的设计模式。在这种模式中，可以指定某个特定事件发生时应该采取的动作

#### 6.1.8、Comparator接口

#### 6.1.9、==对象克隆==

```java
var original = new Employee("John Public",50000);
Employee copy = original;
copy.raiseSalary(10);
```

当original对象建立copy副本后，copy修改salary值，同时original的salary的值也会改变，这时就需要clone

```java
Employee copy = original.clone();
copy.raiseSalary(10);
```

默认的克隆操作是“浅拷贝”，并没有克隆对象中引用的其他对象。如果引用的对象是可变的，那么浅克隆出的对象所引用的对象还是会一同改变，如Date类的对象

如果原对象的浅克隆对象共享的子对象是不可变的，那么这种共享就是安全的，如String对象

浅拷贝同时克隆所有可变的引用对象就是深拷贝

对于每一个类，需要确定：

1. 默认的clone方法是否满足需求
2. 是否可以在可变的子对象上调用clone来修补默认的clone方法
3. 是否不该使用克隆

第三项为默认选项，如果选择一二项，类必须：

1. 实现Cloneable接口
2. 重新定义clone方法，并指定public访问修饰符

Object类中的clone方法声明为protected

Cloneable接口的出现与接口的正常使用，这个接口的作用是，当一个对象请求克隆，但是没有实现这个接口，那么就会生成一个检查型异常

Cloneable是一个少有的**标记接口**(或称为**记号接口**)，标记接口不包含任何方法，它的唯一作用就是允许在类型查询中使用instanceof

### 6.2、lambda表达式

lambda表达式是一个可传递的代码块，可以在一个执行一次或多次

#### 6.2.1、为什么引入lambda表达式

在引入lambda表达式之前，由于java是一种面向对象语言，传递一段代码块并不容易，必须构造一个对象，然后引用这个对象实现的方法来传递，所以引入lambda表达式来处理代码块

#### 6.2.2、lambda表达式的语法

1. 有参表达式：(参数) -> {表达式}

```java
(String first,String second) -> {first.length() - second.length()}
```

2. 无参表达式：() -> {表达式}

```java
() -> {
    for (int i = 100; i>=0; i++){
        System.out.println(i);
    }
}
```

3. 可推导出参数类型，可省略参数类型

```java
Comparator comp = (first , second) -> {
    first.length() - second.length();
}
```

4. 只有一个参数，且参数类型可推导，可以省略空格

```java
ActionListener listener = event -> 
    System.out.println("The time is " + INstant.ofEpochMilli(event.getWhen()));
```

5. 无序指定lambda表达式的返回类型

```java
(String first,String second) -> {first.length() - second.length()}
```

#### 6.2.3、函数式接口

函数式接口：对于只有一个抽象方法的接口，需要这种接口的对象是，就可以提供一个lambda表达式

```java
public class Test {
    static IntCall intcall;
    public static void main(String[] args) {
        intcall = i -> {
            if(i<10){
                return i+intcall.call(i+1);
            }else{
                return i;
            }
        };
        int i2 = intcall.call(1);
    }
}
interface IntCall{
    int call(int x);
}
```

在java中，对lambda表达式所能做的也只是转换为函数式接口

#### 6.2.4、方法引用

```java
var timer = new Timer(1000,event -> System.out.println(event));
//lambda简写为
var timer = new Timer(1000,System.out::println);
```

方法引用指示编译器生成一个函数式接口的实例，覆盖这个接口的抽象方法来调用给定的方法

用 :: 运算符分割方法名与对象或类名：

1. object :: instanceMethod
2. Class :: instanceMethod
3. Class :: staticMethod

只有当lambda表达式的体只调用一个方法而不做其他操作时，才能把lambda表达式重写为方法引用

#### 6.2.5、构造器引用

构造器引用与方法引用很类似，只不过方法名为new。例如，Person::new是Person构造器的一个引用

```java
Arraylist<String> names = ...;
Stream<Person> stream = names.stream().map(Person::new);
List<Person> people =stream.collect(Collectors.toList());
```

Java有一个限制，无法构造泛型类型T的数组。数组构造器引用对于克服这个限制很有用。表达式new T[n]会产生错误，因为这会改为new Object[n]

```java
Person[] people = stream.toArray(Person[]::new);
```

#### 6.2.6、变量作用域

lambda表达式中可以捕获外围作用域中的变量的值，但要确保所捕获的值是明确定义的。在lambda表达式中，只能引用值不会改变的变量。

下面做法为不合法的，因为start是会改变的变量：

```java
public static void countDown(int start,int delay){
    ActionListener listener = event -> {
        start--;
        System.out.println(start);
    };
    new Timer(delay,listener).start();
}
```

如果在lambda表达式中引用一个变量，而这个变量可能在外部改变，这也是不合法的

下面做法是不合法的，因为i在外部发生了改变：

```java
public static void repeat(String text,int count){
    for(int i = 1; i <= count; i++){
        ActionListener listener = event -> {
            System.out.println(i + ":" + text);
        };
        new Timer(1000,listener.start());
    }
}
```

lambda表达式中捕获的变量必须实际上是事实最终变量，事实最终变量是指，这个变量初始化之后就不会再为它赋新值。

lambda表达式的体育嵌套块有相同的作用域

#### 6.2.7、处理lambda表达式

使用lambda表达式的重点是延迟执行

这样做的原因有：

- 在一个单独的线程中运行代码
- 多次运行代码
- 在算法的适当位置运行代码
- 发生某种情况是执行代码
- 只在必要时才运行代码

如果设计你自己的接口，其中只有一个抽象方法，可以用@FunctionalInterface注解来标记这个接口。这样做有两个优点。如果你无意中增加了另一个抽象方法，编译器会产生一个错误消息。另外javadoc页会指出你的接口是一个函数式接口

#### 6.2.8、再谈Comparator

### 6.3、内部类

内部类是定义在另一个类中的类

使用内部类的两个原因：

- 内部类可以对同一个包中的其他类隐藏
- 内部类方法可以访问定义这个类的作用域中的数据，包括原本私有的数据

#### 6.3.1、使用内部类访问对象的状态

一个内部类方法可以访问自身的数据，也可以访问创建它的外围对象的数据字段

内部类的对象总有一个隐式引用，指向创建它的外部类对象

类的可声明权限修饰符

外部类：public、(defualt)

内部类：public、protected、(defualt)、private

#### 6.3.2、内部类的特殊语法规则

使用外围类引用：OuterClass.this

外围类的作用域之外引用内部类：OuterClass.InnerClass

内部类不能有static方法

#### 6.3.3、内部类是否有用、必要和安全

内部类是一个编译器现象，与虚拟机无关。编译器将会把内部类转换为常规的类文件，用$分隔外部类名与内部类名，而虚拟机则对此一无所知

内部类可以访问外围类的私有数据，但外部类则不能访问内部类的的私有数据

如果内部类访问了私有数据字段，就有可能通过外围类所在包中增加的其他类访问那些字段

#### 6.3.4、局部内部类

声明局部类是不能有访问说明符。局部类的作用域被限定在声明这个局部类的块中

局部类有个很大的优势，即对外部世界完全隐蔽

#### 6.3.5、由外部方法访问变量

局部类还有一个优点。它们不仅能够访问外部类的字段，还可以访问局部变量。不过局部变量必须是**事实最终变量**

#### 6.3.6、匿名内部类

假如只想创建这个类的对象，甚至不需要为类指定名字。这样一个类被称为匿名内部类

```java
new SuperType(construction parameters){
    inner class methods and data
}
```

SuperType如果是接口，匿名内部类就要实现这个接口；如果是类，匿名内部类就要扩展这个类

```java
var count = new Person("Dracula"){{initalization}};
```

尽管匿名类不能有构造器，但可以提供一个对象初始化块

外层括号建立了ArrayList的一个匿名子类。内层括号则是一个对象初始化块

#### 6.3.7、静态内部类

使用内部类只是为了把一个隐藏在另一个类的内部，并不需要内部类有外围类对象的一个引用。为此，可以将内部类声明为static，这样就不会生成那个引用。

只要内部类不需要方位外围类对象，就应该使用静态内部类。有些程序员用嵌套类表示静态内部类

与常规类不同，静态内部类可以有静态字段可方法

在接口中声明的内部类自动是static和public

### 6.4、==服务加载器==

### 6.5、==代理==

## 第七章、异常、断言和日志

当发生异常时，应做到：

1. 向用户通知错误
2. 保存所有的工作
3. 允许用户妥善地退出程序

Java使用了一种称为异常处理的错误捕获机制

### 7.1、处理错误

如果由于出现错误而使得某些操作没有完成，程序应：

- 返回到一种安全状态，并能够让用户执行其他命令
- 允许用户保存所有工作结果，并以妥善的方式终止程序

可能会出现的错误种类：

1. 用户输入错误
2. 设备错误
3. 物理限制
4. 代码错误

在Java中，如果某个方法不能够采用正常的途径完成它的任务，可以通过另一个路径退出方法。在这种情况下，方法并不返回任何值，而是抛出一个封装了错误信息的对象。需要注意的是，这个方法将会立即退出，并不返回正常值或任何值。此外，也不会从调用这个方法的代码中继续执行，取而代之的是，异常处理机制开始搜索能够处理这种异常状况的异常处理器

#### 7.1.1、异常分类

![img](JAVA核心技术学习笔记.assets/v2-3733e690763f0fe1f7614ed658b7b2d8_720w.png)

所有异常对象都是派生与Throwable类的一个类实例

Throwable下有两个分支：

- Error：Error类层次结构描述了Java运行时系统的内部错误和资源耗尽错误，这种错误对象不应被抛出，只能通知用户
- Exception：Exception下又分解为两个分支：
  - RuntimeException：
    - 错误的强制类型转换异常
    - 数组访问越界
    - 访问空指针异常
  - 非RuntimeException的异常，如IOException
    - 试图超越文件末尾继续读取数据
    - 试图打开一个不存在的文件
    - 试图根据给定的字符串查找Class对象，而这个字符串表示的类并不存在

Error类或RuntimeException类的所有异常称为非检查型异常

所有其他的异常称为检查型异常

#### 7.1.2、声明检查型异常

方法不仅需要告诉编译器需要返回什么值，还要告诉编译器有可能发生什么样的错误

要在方法的首部指出这个方法可能抛出的一个异常，所以要修改方法首部，以反映这个方法可能抛出的检查型异常

有以下四种情况会抛出异常：

- 调用了一个抛出检查型异常的方法
- 检测到一个错误，并且利用throw语句抛出一个检查型异常
- 程序出现错误
- Java虚拟机或运行时库出现内部错误

有些Java方法包含在对外提供的类中，对于这些方法应该通过方法受不得异常规范声明这个方法的异常

```java
class MyAniation{
    public Image loadImage(String s) throws IOException{
        
    }
}
```

需要抛出多个检查型异常类型，可以将配个异常类型之间用逗号隔开

```java
class MyAniation{
    public Image loadImage(String s) throws FileNotfoundException,EOFException{
        
    }
}
```

==不要声明java的内部错误，即从Error继承的异常，也不要声明从RuntimeException继承的那些非检查型异常==

总结：一个方法必须声明所有可能抛出的检查型异常，而非检查型异常要么在你的控制之外(Error)，要么是由从一开始就应该避免的情况所到会的(RuntimeException)。如果方法没有声明所有可能发生的检查型异常，编译器就会发出一个错误消息



如果在子类中覆盖了超类的一个方法，子类方法中声明的检查型异常不能比超类方法中声明的异常更通用(子类方法可以抛出更特定的异常，或者根本不抛出任何异常)。如果超类方法没有抛出任何检查型异常，子类也不能抛出任何检查型异常

如果一个类中的一个方法声明它会抛出一个异常，而这个异常是某个特定类型的实例，那么这个方法抛出的异常可能属于这个类，也可能属于这个类的任意一个子类。

#### 7.1.3、如何抛出异常

如何抛出异常：

1. 找到一个合适的异常
2. 创建这个类的一个对象
3. 将对象抛出

抛出异常语句：

```java
throw new EOFException();

var e = new EOFException();
throw e;
```

#### 7.1.4、创建异常类

创建一个异常类我们需要做的是定义一个派生与Exception的类，或者派生于Exception的某个子类，如IOException。习惯的做法是，自定义的这个类应该包含两个构造器，一个是默认的构造器，另一个是包含详细描述信息的构造器(超类Throwable的toString方法返回一个字符串，其中包含这个详细信息，这在调试中非常有用)。

```java
class FileFormatException extends IOException{
    public FileFormatException(){}
    public FileFormatException(String grips){
        super(grips);
    }
}
```

java.lang.Throwable

- Throwable()：构造一个新的Throwable对象，但没有详细的描述信息
- Throwable(String message)：构造一个新的Throwable对象，带有指定的详细描述信息。按惯例，所有派生的异常类都支持一个默认构造器和一个带有详细描述信息的构造器
- String getMessage()：获得Throwable对象的详细描述信息

### 7.2、捕获异常

#### 7.2.1、捕获异常

```java
try{
    code;
    more code;
}catch(Exception e){
    handler for this type
}
```

如果try语句块中的任何代码抛出了catch子句中指定的一个异常类，那么程序将跳过try语句块中的其余代码，并执行catch子句中的处理器代码。

如果try语句块中的代码没有抛出任何异常，那么程序将跳过catch子句

如果try语句块中的任何代码抛出了catch子句中没有声明的一个异常类型，那么这个方法就会立即退出(希望它的调用者为了这种类型的异常提供了catch子句)

一般情况下，要捕获那些你知道如何处理的异常，而继续传播那些你不知道怎样处理的异常

#### 7.2.2、捕获多个异常

```java
try{
    code that might throw exceptions;
}catch(FileNotFoundException e){
    e.getMessage();//获得详细的错误消息
    e.getClass().getName;//获得异常对象的实际类型
    emergency action for missing files;
}catch(UnkownNotException e){
    emergency action for unkown hosts;
}catch(IOException e){
    emergency action for all other I/O problems
}
```

在Java7中，同一个catch子句中可以捕获多个异常类型

```java
try{
    code that might throw exceptions;
}catch(FileNotFoundException e | UnkownNotException e){
    emergency action for missing files and unkown hosts;
}catch(IOException e){
    emergency action for all other I/O problems
}
```

只有当捕获的异常类型彼此之间不存在子类关系时才需要这种特性

#### 7.2.3、再次抛出异常与异常链

如果我们想要改变异常类型，我们可以捕获这个异常并在catch子句中抛出一个新的异常

```java
try{
    access the database;
}catch(SQLException e){
    throw new ServletException("database error:" + e.getMessage());
}
```

也可以吧原始异常设置为新异常的"原因"

```java
try{
    access the database;
}catch(SQLExcption e){
    var e = new ServletException("database error");
    e.initCause(original);
    throw e;
}
```

捕获到这个异常时，可以使用下面这条语句获取原始异常

```java
Throwable original = e.getCause();
```

强烈建议使用这种包装技术。这样可以在子系统中抛出高层异常，而不会丢失原始异常的细节。

如果在一个方法中发生了一个检查型异常，但这个方法不允许抛出检查型异常，那么我们可以捕获这个检查型异常，并将它包装成一个运行时异常

在java7后，有时你可能只想记录一个异常，再将它重新抛出，而不做任何改变

```java
try{
    access the database;
}catch(Exception e){
    logger.log(level,message,e);
    throw e;
}
```

#### 7.2.4、finally子句

不管是否有异常被捕获，finally子句中的代码都会执行

```java
var in = new FileInputStream(...);
try{
    //1
    code that might throw exception;
    //2
}catch(IOException e){
    //3
    show error message;
    //4
}finally{
    //5
    in.close;
}
//6
```

1. 代码没有抛出异常：执行1、2、5、6
2. 代码抛出一个异常，并被一个catch捕获：执行1、3、4、5、6
3. 代码抛出一个异常，并被一个catch捕获，但是catch子句也抛出了一个异常：执行1、3、5
4. 代码抛出一个异常，但是没有任何catch子句捕获：执行1、5

警告，不要把改变控制流的语句(return,throw,break,continue)放在finally子句中

#### 7.2.5、try-with-Resources

所有被打开的系统资源，比如流、文件或者Socket连接等，都需要手动关闭

```java
BufferedInputStream  in = null;
try{
    in = new BufferedInputStream(new FileInputStream(new File("test.txt")));
}catch (IOException e) {
    e.printStackTrace();
}finally {
    if (in != null){
        try {
            in.close();
        }catch (IOException ex){
            ex.printStackTrace();
        }
    }
}
```

但是这样十分繁琐

java7之后，可以使用try-with-resource语句方便的退出

```java
try(var in = new BufferedInputStream(new FileInputStream(new File("test.txt")));){
    int read = in.read();
} catch (IOException e) {
    e.printStackTrace();
}
```

try块退出时，内部实现AutoClosable接口的资源，会自动调用res.close()。

在Java9中，可以在try首部中提供之前声明的事实最终变量

```java
public static void printAll(String[] lines,PrintWriter out){
    try(out){
        for(String line : lines)
            out.println(line);
    }
}
```

反编译可以看到，close方法如果发生异常，会被自动捕获，并由addSuppressed方法增加到原来的异常

#### 7.2.6、分析堆栈轨迹元素

堆栈轨迹是程序执行过程中某个特定点上的所有挂起的方法调用的一个列表

可以使用Throwable类中的printStackTrace方法访问堆栈轨迹的文本描述信息

```java
var t = new Throwable();
var out = new StringWriter();
t.printStackTrace(new PrintWriter(out));
String description = out.toString();
```

也可以使用StackWalker类，它会生成一个StackWalker.StackFrame实例流，其中每个实例分别描述一个栈帧

```java
StackWalker walker = StackWalker.getInstance();
walker.forEach(System.out::println);
```

如果想要以懒方式处理Stream<StackWalker.StackFrame>，可以调用：

```java
walker.walk(stream -> process stream)
```

java.lang.Throwable：

- Throwable(Throwable cause)
- Throwable(String message,Throwable cause)：用给定的cause(原因)构造一个Throwable对象
- Throwable initCause(Throwable cause)：为这个对象设置原因，如果这个对象已有一个原因，则抛出一个异常。返回this
- Throwable  getCause()：获得设置为这个对象原因的异常对象。如果没有设置原因，则返回null
- StackTraceElement[] getStackTrace()：获得构造这个对象是调用堆栈的轨迹。
- void addSuppressed(Throwable t) ：==7== 为这个异常增加一个“抑制的”异常。这出现在try-with-resoures语句中，其中t是close方法抛出的一个异常
- Throwable[] getSuppressed() ：==7== 得到这个异常的所有“抑制的”异常。一般来说，这些是try-with-resource语句中close方法抛出的异常

java.lang.Exception：

- Exception(Throwable cause)
- Exception(String message,Throwable cause)：用给定的cause构造一个Exception对象

java.lang.RuntimeException：

- RuntimeException(Throwable cause)：
- RuntimeException(String message,Throwable cause)：用给定的cause构造一个RuntimeException对象

java.lang.StackWalker：==9==

- static StackWalker getInstance()：

- static StackWalker getInstance(StackWalker.Option option) ：

- static StackWalker getInstance(Set<StackWalker.Option> options) ：

  得到一个StackWalker实例。选项包括StackWalker.Option枚举中的RETAIN_CLASS_REFERENCE、SHOW_REFLECT_FRAMES和SHOW_HIDDEN_FRAMES

- void forEach(Consumer<? super StackWalker.StackFrame> action) ：在每个栈帧上完成跟定的动作，从最近调用的方法开始

- <T> T walk(Function<? super Stream<StackWalker.StackFrame>,? extends T> function) ：

  对一个栈帧流应用给定的函数，返回这个函数的结果

java.lang.StackWalker.StackFrame:==9==

- String getFileName() ：得到包含该元素执行点的源文件的文件名，如果这个信息不可用则返null
- int getLineNumber() ：得到包含钙元素执行点的源文件的行号，如果这个信息不可用则返回-1
- String getClassName() ：得到方法包含该元素执行点的类的完全限定名
- Class<?> getDeclaringClass() ：得到方法包含该元素执行点的类的Class对象。如果这个栈遍历器不是用RETAIN_CLASS_REFERENCE选项构造的，则会抛出一个异常
- String getMethodName() ：得到包含该元素执行点的方法的方法名。构造器的方法名为<init>。静态初始化器的方法名为<clinit>。无法区分同名的重载方法
- boolean isNativeMethod() ：如果这个元素的执行点在一个原生方法中，则返回true
- String toString()：返回一个格式化的字符串，包含类和方法名、文件名以及行号(如果这些信息可用)

java.lang.StackTraceElement：

- String getFileName() ：得到包含该元素执行点的源文件的文件名，如果这个信息不可用则返null
- int getLineNumber() ：得到包含钙元素执行点的源文件的行号，如果这个信息不可用则返回-1
- String getClassName() ：得到方法包含该元素执行点的类的完全限定名
- String getMethodName() ：得到包含该元素执行点的方法的方法名。构造器的方法名为<init>。静态初始化器的方法名为<clinit>。无法区分同名的重载方法
- boolean isNativeMethod() ：如果这个元素的执行点在一个原生方法中，则返回true
- String toString()：返回一个格式化的字符串，包含类和方法名、文件名以及行号(如果这些信息可用)

### 7.3、使用异常的技巧

1. 异常处理不能代替简单的测试
2. 不要过分的细化异常
3. 充分利用异常的层次结构
4. 不要压制异常
5. 在检测错误时，“苛刻”要比放任更好
6. 不要羞于传递异常

### 7.4、使用断言

#### 7.4.1、断言的概念

断言机制允许在测试期间向代码中插入一些检查，而在生产代码中会自动删除这些检查。

Java语言引入了关键字assert。这个关键字有两种形式：

```java
assert condition;
assert condition:expression;
```

```java
assert x>=0;
assert x>=0 : x;
```

两个语句都会计算条件，如果结果为false，则抛出一个AssertionError异常。第二个语句则会将表达式传入AssertionError对象的构造器，并转换成一个消息字符串。

断言使用场景，可以用作测试，也可以用来作为提示或注释

#### 7.4.2、启用和禁用断言

在默认情况下，断言是禁用的。可以在运行程序时用-enableassertions或ea选项启动断言：

```asciidoc
java -enableassertions MyApp
```

不必重新编译程序来启动或禁用断言。启用或禁用断言是类加载器的功能。禁用断言时，类加载器会去除断言代码

也可以在某个类或整个包中启用断言

```
java -ea:MyClass -ea:com.mycompany.mylib MyApp
```

如果想要关闭断言，可以用选项 -disableassertions或-da在某个特定类和包中禁用断言

```java
java -ea:... -da:MyClass MyApp
```

#### 7.4.3、使用断言完成参数检查

Java语言中，给出了三种处理系统错误的机制

- 抛出一个异常
- 日志
- 使用断言

使用断言的场景：

- 断言的失败是致命且不可恢复的错误
- 断言检查只是在开发和测试阶段打开

#### 7.4.4、使用断言提供假设文档

java.lang.ClassLoader：

- void setDefaultAssertionStatus(boolean enabled) ：为通过类加载器加载的类(没有显式的类或包断言状态)启用或禁用断言
- void setClassAssertionStatus(String className, boolean enabled) ：为给定的类和它的内部类启用或禁用断言
- void setPackageAssertionStatus(String packageName, boolean enabled) ：为给定的包及其自爆中的所有类启用或禁用断言
- void clearAssertionStatus() ：删除所有显式的类和包断言状态设置，并禁用通过这个类加载器加载的所有类的断言

### 7.5、日志

使用日志API的优点：

- 可以很容易地取消全部日志记录，或仅仅取消某个级别以下的人日志，而且可以很容易地再次打开日志开关
- 可以很简单地禁止日志记录，因此，将这些日志代码留在程序中的开销很小
- 日志记录可以被定向到不同的处理器，如控制台显示、写至文件，等等
- 日志记录器和处理器都可以对记录进行过滤。过滤器可以根据过滤器实现器指定的标准丢弃那些无用的记录项
- 日志记录可以采用不同的方式格式化，例如，纯文本或xml
- 应用程序可以使用多个日志记录器，它们使用与包名类似的有层次结构的名字，例如，com.mycompany.myapp
- 日志系统的配置由配置文件控制

#### 7.5.1、基本日志

要生成简单的日志记录，可以使用全局日志记录器，并调用器info方法：

```java
Logger.getGlobal().info("file->Open menu item selected");
```

如果想要禁用所有日志可以在适当的地方(如main的最前面)调用：

```java
Logger.getGlobal().setLevel(Level.OFF);
```

#### 7.5.2、高级日志

可以调用getLogger方法创建或获取日志记录器

```java
private static final Logger myLogger = Logger.getLogger("com.myconpany.myapp");
```

未被任何变量引用的日志记录器可能会被垃圾回收。为防止发生，可以用静态变量存储日志记录器的引用

7个日志级别：

- SEVERE
- WARNING
- INFO
- CONFIG
- FINE
- FINER
- FINEST

在默认情况下，实际上只记录前3个级别，当然可以设置其他级别：

```java
logger.setLevel(Level.FINE);
```

Level.ALL开启所有级别日志

LEVEL.OFF关闭所有级别日志

所有级别日志都有记录方法

```java
logger.warning(message);
logger.fine(message);
```

可以使用log方法并指定级别达到如上效果

```java
logger.log(LEVEL.FINE,message);
```

建议使用低级别的日志来用于诊断和一些不重要的日志

默认日志处理器会抑制INFO及以下级别的日志

可以使用logp方法获得调用类的方法的确切位置，这个方法的签名为：

```java
public void logp(Level level, String sourceClass, String sourceMethod, String msg);//记录消息，指定源类和方法，不带参数。 
```

跟踪执行流的便利方法

```java
void entering(String sourceClass, String sourceMethod)//记录方法条目。 
void entering(String sourceClass, String sourceMethod, Object param1)//使用一个参数记录方法条目。 
void entering(String sourceClass, String sourceMethod, Object[] params)//使用参数数组记录方法条目。
void exiting(String sourceClass, String sourceMethod)//记录方法返回。 
void exiting(String sourceClass, String sourceMethod, Object result) //使用结果对象记录方法返回。
```

获得日志记录中包含异常的描述

```java
void throwing(String sourceClass, String sourceMethod, Throwable thrown);//记录抛出异常。
void log(Level level, String msg, Throwable thrown);//使用关联的Throwable信息记录消息。  
```

#### 7.5.3、修改日志管理器配置

可以通过编辑配置文件来修改日志系统的各个属性，在默认情况下，配置文件位于conf/logging.properties(java9之前位于jre/lib/logging.properties)

自定义配置

```java
java -Djava.util.logging.config.file = configFile MainClass
```

想要修改默认的日志级别，就需要编辑配置文件，并修改以下的命令行

```java
.level=INFO
```

可以通过添加下面这一行来指定自定义日志记录器的日志级别

```java
com.mycompany.myapp.level=FINE
```

日志管理器在虚拟机启动时初始化，也就是在main方法执行前。如果想要定制日志属性，但是没有用-Djava.util.logging.config.file命令行选项启动应用，可以在程序中调用System.setProperty("java.util.logging.config.file",file)。不过，这样一来，你还必须调用LogManager.getLogManager().readConfiguration()重新更新日志配置

在Java9中，可以通过调用以下方法更新日志配置：

```java
LogManager.getLogManager().updateConfiguration(mapper);
```

这样就会从java.util.logging.config.file系统属性指定的位置读取一个新配置。然后应用这个映射器来解析老配置或新配置中所有键的值。映射器是一个Function<String,BiFunction<String,String,String>>。它将现有的配置中的键映射到替换函数。每个替换函数接收到与键关联的老值和新值(如果没有关联则得到null)，生成一个替换，或者如果要在更新中删除这个键则返回null

#### 7.5.4、本地化

本地化的应用程序包含资源包中的本地特定信息。资源包包括一组映射，分别对应各个本地化环境。

一个程序可以包含多个资源包，例如一个用于菜单，另一个用于日志消息，想要为资源包增加映射，需要对应每个本地化环境提供一个文件。如英文：mycompany/logmessages_en.properties，德文：mycompany/logmessages_de.properties

可以将这些文件与应用程序的类文件放在一起，以便ResouceBundle类自动找到它们。

请求时，可以指定一个资源包

```java
Logger logger = Logger.getLogger(loggerName,"com.mycompany.logmessages");
```

然后为日志消息指定资源包的键，而不是实际的日志消息字符串

```java
logger.info("readingFile");
```

通常需要将一些消息增加一些参数，因此，消息可能包括占位符{0}，{1}等

```
Reading file {0}.
Achung!Datei{0} wird eingelesen
```

```java
logger.log(Level.INFO,"readingFile",fileName);
logger.log(Level.INFO,"renamingFile",new Object[]{oldName,newName});
```

在Java9中可以在logrb方法中指定资源包对象

```java
logger.logrb(Level.INFO,bundle,"renamingFile",oldName,newName)
```

#### 7.5.5、处理器

默认情况下，日志记录器将记录发送到ConsoleHandler，并由他输出到System.err流

与日志记录器一样，处理器也有日志级别。对于一个要记录的日志记录，它的日志级别必须高于日志记录器和处理器二者的阈值

默认为：java.util.logging.ConsoleHandler.level=INFO

日志记录器和处理器都会发出日志，所以应将userParentHandlers属性设为false

想要把日志发送到其他地方需要添加其他处理器，日志API中提供了两个有用的处理器FileHandler、SocketHandler

FileHandler：可以将记录收集到文件中

SocketHandler：可以将记录发送到指定的主机和端口

可以通过设置日志管理器配置文件中的不同参数(详情请看下表)，或者使用另一个构造器，来修改文件处理器的默认行为

![image-20200620153411355](JAVA核心技术学习笔记.assets/image-20200620153411355.png)

#### 7.5.6、过滤器

在默认情况下，会根据日志记录的级别进行过滤。每个日志记录器和处理器都有一个可选的过滤器来完成附加的过滤，要定义一个过滤器，需要实现Filter接口，并定义以下方法：

```java
boolean isLoggable(LogRecord record)
```

#### 7.5.7、格式化器

ConsoleHandler类可以生成文本和XML格式的日志记录。不过，你也可以自定义格式。这需要扩展Formatter类并覆盖下面的方法：

```java
String format(LogRecord record)
```

可以根据自己的需要以任何方式对记录中的信息进行格式化，并返回结果字符串。

可以对记录中的消息部分进行格式化，将替换参数并应用本地化处理：

```java
String formatMessage(LogRecord record)
```

很多文件格式需要在已格式化的记录的前后加上一个头部和尾部

```java
String getHead(Handler h)
String getTail(Handler h)
```

最后，调用setFormatter方法将格式化器安装到处理器中

#### 7.5.8、日志技巧

1. 对一个简单的应用，选择一个日志记录器
2. 默认的日志配置会把级别等于或高于INFO的所有消息记录到控制台
3. 最好只将对程序用户有意义的消息设置为几个级别

java.util.logging.Logger：

- static Logger getLogger(String name) 

- static Logger getLogger(String name, String bundleName)：

  查找或创建指定子系统的记录器，本地化消息位于名为bundleName的资源包中  

- void severe(String msg) 

- void warning(String msg) 

- void info(String msg) 

- void config(String msg) 

- void fine(String msg) 

- void finer(String msg) 

- void finest(String msg) ：
  记录一个日志记录，包含方法名指示器的级别和给定的信息
  
- void entering(String sourceClass, String sourceMethod) 

- void entering(String sourceClass, String sourceMethod, Object param1) 

- void entering(String sourceClass, String sourceMethod, Object[] params) 

- void exiting(String sourceClass, String sourceMethod) 

- void exiting(String sourceClass, String sourceMethod, Object result)

  记录一个描述进入/退出方法(有给定的参数和返回值)的日志记录

- void throwing(String sourceClass, String sourceMethod, Throwable thrown)：记录抛出异常。

- void log(Level level, String msg) 

- void log(Level level, String msg, Object param1) 

- void log(Level level, String msg, Object[] params) 

- void log(Level level, String msg, Throwable thrown) 

  记录一个有给定级别和消息的日志记录，其中包括对象或者可抛出对象。要包括对象，消息中必须包含格式化占位符{0}、{1}等

- void logp(Level level, String sourceClass, String sourceMethod, String msg)

- void logp(Level level, String sourceClass, String sourceMethod, String msg, Object param1)

- void logp(Level level, String sourceClass, String sourceMethod, String msg, Object[] params)

- void logp(Level level, String sourceClass, String sourceMethod, String msg, Throwable thrown)

  记录一个有给定级别、准则的调用者信息和消息的日志记录，其中可以包括对象或可抛出对象

- void logp(Level level, String sourceClass, String sourceMethod, Throwable thrown, Supplier<String>==9==

- void logp(Level level, String sourceClass, String sourceMethod, Supplier<String> msgSupplier)==9==

  记录一个有给定级别、准则的调用者信息、资源包和消息的日志记录，其中可以包括对象或可抛出对象

- Level getLevel()

- void setLevel(Level newLevel)

  获得和设置这个日志记录器的级别

- Logger getParent() 

- void setParent(Logger parent) 

  获得和设置这个日志记录器的父日志记录器

- Handler[] getHandlers()

  获取与此记录器关联的处理器

- void addHandler(Handler handler)

- void removeHandler(Handler handler) 

  增加或删除这个日志记录器中的一个处理器

- boolean getUseParentHandlers() 

- void setUseParentHandlers(boolean useParentHandlers) 	

  获得和设置使用父处理器属性，如果是true，日志记录器会将全部日志转发至其父处理器

- Filter getFilter() 

- void setFilter(Filter newFilter) 

  获得和设置这个日志记录器的过滤器

java.util.logging.Handler 

- abstract void publish(LogRecord record) ：发送LogRecord 
- abstract void flush()：刷新所有缓冲输出。 

- abstract void close()：刷新所有缓冲输出，并释放所有相关资源。

- Filter getFilter() 

- void setFilter(Filter newFilter) 

  获得和设置这个处理器的过滤器

- Formatter getFormatter() 

- void setFormatter(Formatter newFormatter) 

  获得和设置这个处理器的格式化器

- Level getLevel()

- void setLevel(Level newLevel)

  获得和设置这个处理器的级别

java.util.logging.ConsoleHandler 

- ConsoleHandler()：构造一个新的控制台处理器

java.util.logging.FileHandler
- FileHandler()

- FileHandler(String pattern)

- FileHandler(String pattern, boolean append)

- FileHandler(String pattern, int limit, int count)

- FileHandler(String pattern, int limit, int count, boolean append)

- FileHandler(String pattern, long limit, int count, boolean append) ==9==

  构造一个文本处理器，limit实在打开一个新日志文件之前，日志文件可以包含的近似最大字节数。count是循环序列的文件数量。如果append为true，记录则应该准假在一个已存在的日志文件末尾

java.util.logging.LogRecord

- Level getLevel() ：获取日志消息级别

- String getLoggerName()：获取源记录器的名称。

- ResourceBundle getResourceBundle()

- String getResourceBundleName()

  获取本地化资源包或包名，如果没有提供，则返回null

- String getMessage()：在本地化或格式化之前获取“原始”日志消息

- Object[] getParameters()： 获取日志消息的参数，如果没有提供，则返回null

- Throwable getThrown()： 获得所抛出的对象，如果没有提供，则返回null

- String getSourceClassName() 

- String getSourceMethodName()

  获得记录这个日志记录的代码区域。这个信息有可能是由日志记录代码提供的，也有可能是自动从运行时堆栈推测出来的。如果日志记录代码提供的值有误，或者运行时代码由于优化而无法推测出确切的位置，这两个方法的返回值就有可能不准确

- long getMillis()：自1970年以来以毫秒为单位获取创建时间

- Instant getInstant()==9==：获取创建时间，作为java.time.Instant返回

- long getSequenceNumber()：获取序列号。 

- int getThreadID()：获取创建这个日志记录的线程的唯一ID。这些ID是由LogRecord类分配的，与其他线程的ID无关

java.util.logging.LogManager 

- static LogManager getLogManager()：返回全局LogManager对象。 

- void readConfiguration() 

- void readConfiguration(InputStream ins)

  从系统属性java.util.logging.config.file指定的文件或者给定的输入流读取日志配置

- void updateConfiguration(InputStream ins, Function<String,BiFunction<String,String,String>> mapper) ==9==

- void updateConfiguration(Function<String,BiFunction<String,String,String>> mapper) )==9==

  将日志配置与系统属性java.util.logging.config.file指定的文件或给定的输入流合并

java.util.logging.Filter

- boolean isLoggable(LogRecord record)：如果给定日志记录需要记录，则返回true

java.util.logging.Formatter 

- abstract String format(LogRecord record) 格式化给定的日志记录并返回格式化的字符串。

- String formatMessage(LogRecord record) 从日志记录中本地化和格式化消息字符串。

- String getHead(Handler h) 

- String getTail(Handler h) 

  返回应该出现在包含日志记录的文档开头和结尾的字符串。Formatter超类将这些方法返回日志记录的本地化和格式化消息部分

### 7.6、调试技巧

1. 通过日志或标准输出流打印结果

2. 通过main方法测试

3. 通过Junit测试

4. 通过日志代理测试

5. 利用Trowable类的printStackTrance方法，打印出异常的堆栈轨迹

6. 通过System.err也可以打印出堆栈轨迹

7. ```java
   //捕获System.out
   java Myprogram > error.txt
   //捕获System.error
   java Myprogram 2> error.txt
   //同时捕获
   java Myprogram 1> error.txt 2>&
   ```

8. 使用静态方法setDefaultUncaughtExceptionHandler(Thread.UncaughtExceptionHandler eh) 设置当线程由于未捕获的异常而突然终止时调用的默认处理程序，并且没有为该线程定义其他处理程序。  

9. 想要观察类的加载过程，启动Java虚拟机时可以使用-verbose标志

10. -Xlint选项告诉编译器找出常见的代码问题

11. Java虚拟机增加了对Java应用程序的监控和管理支持，允许在虚拟机中安装代理来跟踪内存消耗、线程使用、类加载等情况，java自带jconsole

12. Java任务控制器是一个专业级性能分析和诊断工具

## 第八章、泛型程序设计

泛型类和泛型方法有类型参数，这是的他们可以准确地描述用特定类型实例化时会发生什么。在有泛型类之前，程序员必须使用Object编写适用于多种类型的代码。这很繁琐也很不安全

### 8.1、为什么要使用泛型程序设计

泛型程序设计意味着编写的代码可以对多种不同类型的对象重用。

#### 8.1.1、类型参数的好处

在Java中增加泛型类之前，泛型程序设计是用继承实现的。ArrayList类值维护一个Object引用的数组

这种方法存在两个问题。当获取一个值时必须进行强制类型转换

此外，这里没有错误检查。可以向数组列表中添加任何类的值，这样极有可能发生错误

泛型提供了一个更好的解决方案，**类型参数**

```java
var files = new ArrayList<String>();
```

#### 8.1.2、谁想成为泛型程序员

试想一下，Emloyee类型是Manager类型的超类，当我们想将ArrayList\<Manager\>中的所有元素添加到ArrayList\<Employee\>中，我们可以使用addAll方法，但是反之这样做就会产生错误，Java发明了**通配符类型**来解决这样的问题

### 8.2、定义简单泛型类

泛型类就是有一个或多个类型变量的类

```java
public class Pair<T> 
{
   private T first;
   private T second;

   public Pair() { first = null; second = null; }
   public Pair(T first, T second) { this.first = first;  this.second = second; }

   public T getFirst() { return first; }
   public T getSecond() { return second; }

   public void setFirst(T newValue) { first = newValue; }
   public void setSecond(T newValue) { second = newValue; }
}
```

常见的做法是类型变量使用大写字母，而且很简短。Java库使用变量E表示集合的元素类型，K和V分别表示表的键和值的类型。T(必要时还可以用相邻的字母U和S)表示任意类型

可以用具体的类型替换类型变量来实例化泛型类型，换句话说泛型类相当于普通类的工厂

```java
Pair<String> p = new Pair;
```

### 8.3、泛型方法

```java
class ArrayAlg{
    public static <T> T getMiddle(T... a){
        return a(a.length/2);
    }
}
```

注意，类型变量放在修饰符(这里的修饰符就是public static)的后面，并在返回类型的前面

泛型方法可以在普通类中定义，也可以在泛型类中定义

调用泛型方法

```java
String middle = ArrayAlg.<String> getMiddle("John","Q","Public");
//实际上<String>可以省略
String middle = ArrayAlg. getMiddle("John","Q","Public");
```

但并非所有情况都可以省略类型参数

```java
double middle = ArrayAlg. getMiddle(3.14,1234,0);
```

编译器将吧参数自动装箱为1个Double和两个Integer对象，然后寻找这些类的共同超类型

### 8.4、类型变量的限定

可以对类型变量T设置一个限定，这样就可以使用一些特定方法

```java
public static <T extends Comparable> T min(T[] a)
```

限定表示方法：

```java
<T extends BoundingType>
```

表示T应该是限定类型的子类型。T和限定类型可以是类，也可以是接口

一个类型变量或通配符可以有多个限定

```java
<T extends Comparable & Serializable>
```

限定类型用“&”分割，而逗号用来分割类型变量

### 8.5、泛型代码和虚拟机

虚拟机没有泛型类型对象，所有对象都属于普通类

#### 8.5.1、类型擦除

无论何时定义一个泛型类型，都会自动提供一个响应的原始类型。这个原始类型的名字就是去掉类型参数后泛型类型名。类型变量会被擦除，并替换为其限定类型(如果没有限定的变量则替换为Object)

如果有多个限定类型，则使用第一个限定类型作为原始类型

```java
class Interval<T extends Serializable & Comparable>
```

原始类型会用Serializable替换T，而编译器在必要时要向Comparable插入强制类型转换，所以应该将标签接口(即没有方法的接口)放在限定列表末尾

#### 8.5.2、转换泛型表达式

编写一个泛型方法调用时，如果擦除了返回类型，编译器会插入强制类型转换。

```java
Pair<Empolyee> buddies = ...;
Employee boddy = buddies.getFirst();
```

编译器把这个方法调用转换为两条虚拟机指令：

- 对原始方法Pair.getFirst的调用
- 将返回的Object类型强制转换为Employee类型

当访问一个泛型字段时，也要插入强制类型转换

#### 8.5.3、==转换泛型方法==

类型擦除也会出现在泛型方法中，但是方法的擦除带来了两个复杂的问题

```java
class DateInterval extends Pair<LocalDate>{
    public void setSecond(LocalDate second){
        if(second.compateTo(getFirst()) >= 0)
            super.setSecond(second);
    }
}
```

这条代码擦除后变成

```java
class DateInterval extends Pair{
    public void setSecond(LocalDate second){...}
    ...
}
```

但是反编译后(javap -c className)发现DateInterval中有两个setSecond方法

- setSecond(LocalDate second)
- setSecond(Object second)

```java
var interval = new DateInterval(...);
Pair<LocalDate> pair = interval;
pair.setSecend(aDate);
```

由于Pair引用一个DateInterval对象，所以应该调用DateInterval.setSecend，问题在于类型擦除与多态发生了冲突，为了解决这个问题，编译器在DateInterval类中生成了一个桥方法

```java
public void setSecond(Object second){
    setSecond((LocalDate) second);
}
```

桥方法不仅用于泛型类型，当一个方法覆盖另一个方法是，可以指定一个更严格的返回类型

```java
public class Employee implements Cloneable{
    public Employee clone() throws CloneNotSupportedExcption{...}
}
```

Object.clone Employee.clone方法被称为有协变的返回类型

总结：

- 虚拟机中没有泛型，只有普通的类和方法
- 所有的类型参数都会替换为他们的限定类型
- 会合成桥方法来保持多态
- 为保持类型安全性，必要时会插入强制类型转换

#### 8.5.4、调用遗留代码

当一个遗留类的方法想要调用最新的泛型方法时，编译器会提出一个让你确保标签内的类型对应的警告，但是我们只能人为的控制不产生错误。

我们可以用@SuppressWarnings("unchecked")来消除这个警告，这个注解会关闭对方法中所有代码的检查

### 8.6、限制和局限性

#### 8.6.1、不能用基本类型实例化类型参数

#### 8.6.2、运行时类型查询只适用于原始类型

虚拟机中的对象总有一个特定的非泛型类型。因此，所有的类型查询只产生原始类型

同样getClass方法总是返回原始类型

#### 8.6.3、不能创建参数化类型的数组

```java
var table = new Pair<String>[10];
```

擦除之后，table的类型是Pair[]。可以把它转换为Object[]：

```java
Object[] objarray = table;
```

数组会记住它的元素类型，如果试图储存其他类型的元素，就会抛出一个ArrayStoreException异常

不过对于泛型类型，擦除会使这种机制无效

```java
objarray[0] = new Pair<Employee>();
```

虽然能通过检查，但是仍然会导致类型错误，出于这个原因，不允许创建参数化类型的数组

需要注意的是，只是不允许创建这些数组，而声明类型为Pair\<String>[]的变量仍然合法的，不过不能用new Pair\<String>[10]初始化这个变量

可以声明通配类型的数组，然后强制类型转换

```java
var table = (Pair<String>[]) new Pair<?>[10];
```

不过这样是不安全的，如果在table[0]中储存一个Pair\<Employee>，然后对table[0].get.First()调用一个String方法，会得到一个ClassCastException异常

如果需要收集参数化类型对象，简单地使用ArrayList:ArrayList\<Pair\<String>>更安全、有效

#### 8.6.4、Varargs警告

```java
public static <T> void addAll(Collection<T> coll,T... ts){
    for(T t : ts)coll.add(t);
}
```

```java
Collection<Pair<String>> table = ...;
Pair<String> pair1 = ...;
Pari<String> pair2 = ...;
addAll(table,pair1,pair2);
```

为了调用addAll方法，Java虚拟机必须建立一个Pair\<String>数组，这就违反了不支持泛型的数组规则。不过虚拟机只会给你一个警告，而不是错误。

抑制这个警告有两种方法

- 方法上加上@SuppressWarnings("unchecked")
- 在java7后可以用@SafeVarargs直接注解addAll方法

对于任何只需要读取参数数组元素的方法，都可以使用@SafeVarargs，它只能用于声明static、final或==Java9==private的构造器和方法。所有其他方法都有可能被覆盖，使之没有意义

可以使用@SafeVarargs注解来消除创建泛型数组的有关限制

```java
@SafeVarargs
static <E> E[] array(E...array){
    return array;
}
```

但并不能避免异常

























