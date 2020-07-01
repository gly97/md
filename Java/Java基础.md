# Java入门
## 1.1 Java是什么
简单的说，**Java**是一门面向对象编程语言，吸收了C/C++的优点，摒弃了C/C++复杂的指针等内容，也不需要用户手动释放内存空间。java本身还具备了很强的可移植性，通过将源代码编译成二进制字节码，然后通过不同平台的java虚拟机来解释执行字节码，从而实行了“一次编译，到处执行”的跨平台特性。
##  1.2 Java特性
Java最重要的特性是面向对象，特点：

 - 面向对象的编程思想更贴近人的正常思维模式

 - 面向对象的编程来源于生活服务于生活

 - 面向对象的特征：**抽象、封装、继承、多态**  


  **面向过程**

优点：性能比面向对象高，因为类调用时需要实例化，开销比较大，比较消耗资源;比如单片机、嵌入式开发、 Linux/Unix等一般采用面向过程开发，性能是最重要的因素。
缺点：没有面向对象易维护、易复用、易扩展。

**面向对象**

优点：易维护、易复用、易扩展，由于面向对象有封装、继承、多态性的特性，可以设计出低耦合的系统，使系统 更加灵活、更加易于维护
缺点：性能比面向过程低。
  **面向过程和面向对象的区别**：面向过程就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候一个一个依次调用就可以了；面向对象是把构成问题事务分解成各个对象，建立对象的目的不是为了完成一个步骤，而是为了描叙某个事物在整个解决问题的步骤中的行为。

**对象是什么？**

任何一个具体的事物都是一个对象，    万事万物皆对象-《java编程思想》

任何一个对象都是唯一的，即使两个相近或者相似的事物都是两个不同的对象，就像世界上找不到两片相同的树叶一样，任何一个对象一旦创建就会唯一的存在。而且对象也不一定是一个切实存在的实物，任何一项法规、政策、虚拟物品等都是一个对象。  

**Java面对对象的三大特特性**  

**1.封装：**


 - 就是把同一类事物的属性和方法归到同一个类中，方便使用 
 -  防止该类的代码和数据被外部类定义的代码随意访问 
 -  要访问该类的数据和代码必须通过严格的方法控制 
 -  封装的主要功能在于我们能修改自己的实现代码，而不用修改哪些调用程序的代码片段。
 -  优点：减少耦合，类内部自由修改，可以对类成员变量进行更精确的控制，隐藏信息、实现细节。
 -  为了实现良好的封装，通常将类的成员变量声明为private ,通过public的set和get方法完成对属性的操作

**2.继承：**（extends）

 -  继承就是子类继承父类的特征和行为，使得子类对象（实例）具有父类的实例域和方法
 -  特性： 
   子类拥有父类的非private属性，方法 
子类可以拥有自己的属性和方法，即子类可以对父类进行扩展 
子类可以用自己的方式实现父类的方法 
 -  java的继承是单继承 （但可以多重继承）


**3.多态：**  

- 多态性是指允许不同子类型的对象对同一消息作出不同的响应
-  一个对象的多种表现形态
- 使用父类类型的引用指向子类的对象；
-　该引用只能调用父类中定义的方法，不能调用子类中独有的方法。
-  如果子类中重写了父类中的一个方法，那么在调用这个方法的时候。将会调用子类中重写之后的放发
-  子类可以调用父类中所有的方法。
-  使用继承不仅可以重用已有的代码，从而避免代码重复，还可以创建一个更容易维护和修改代码的程序。

**4.抽象：**

 -  父类为子类提供一些属性和行为，子类根据业务需求实现具体的行为。

抽象类使用abstract进行修饰，子类要实现所有的父类抽象方法否则子类也是抽象类。

## 1.3 Java的环境配置及常用术语
**JVM**（Java Virtual Machine） :java虚拟机，只识别.class文件。jvm 是 Java 能够跨平台的核心。它是整个java实现跨平台的最核心的部分，所有的java程序会首先被编译为.class的类文件，这种类文件可以在虚拟机上执行。也就是说class并不直接与机器的操作系统相对应，而是经过虚拟机间接与操作系统交互，由虚拟机将程序解释给本地系统执行。  

**JRE**（Java Runtime Environment）Java 运行时环境。它主要包含两个部分，jvm 的标准实现和 Java 的一些基本类库。它主要包含两个部分，jvm 的标准实现和 Java 的一些基本类库。它相对于 jvm 来说，多出来的是一部分的 Java 类库。  

**JDK**（Java Development Kit）Java 开发工具包jdk 是整个 Java 开发的核心，它集成了 jre 和一些好用的小工具。jdk 是整个 Java 开发的核心，它集成了 jre 和一些好用的小工具。例如：javac.exe，java.exe，jar.exe 等。  

**SE** （Standard Edition）：用于桌面或简单服务器应用的java平台。（我们现在正在学习的）

**EE**（Enterprise Edition）：用于复杂服务器应用的java平台。    

集成开发环境安装：新手推荐**Idea+jdk8**，安装和使用都比较简单，网上有很多教程。
**jdk环境变量**配置：
(1)新建->变量名"JAVA_HOME"，变量值"C:\Java\jdk1.8.0_05"（即JDK的安装路径） 
(2)编辑->变量名"Path"，在原变量值的最后面加上“;%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin” 
(3)新建->变量名“CLASSPATH”,
变量值“.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar”


#  2. java 数据类型及运算符

java的**数据类型**分为两类：1.基本数据类型；2.引用数据类型。
## 2.1 基本数据类型和对应的包装类
注：Java是面向对象的语言但是为了方便也引入了基本数据类型以及类似与lambda表达式等     

| 基本类型 | 包装类型  | 长度（字节） | 默认值 | 可表示数据范围                          |
| -------- | --------- | ------------ | ------ | --------------------------------------- |
| byte     | Byte      | 1            | 0      | -128~127                                |
| short    | Short     | 2            | 0      | -32768~32767                            |
| int      | Short     | 4            | 0      | 2147483648~2147483647                   |
| long     | Long      | 8            | 0L     | 9223372036854775808~9223372036854775807 |
| float    | Flaot     | 4            | 0.0f   | 1.4E-45~3.4028235E38                    |
| double   | Double    | 8            | 0.0    | 4.9E-324~1.7976931348623157E308         |
| char     | Charcater | 2            | null   | 0~65535                                 |
| boolean  | Boolean   | (不确定)     | flase  | true/false                              |

**浮点数值**不适用于无法接受舍入误差的金融计算中 。 例如 ， 命令 `System . out . println( 2.0 - 1.1 )` 将打印出 0.8999999999999999 , 而不是人们想象的 0.9。 这种舍入误差的主要原因是浮点数值采用二进制系统表示 ， 而在二进制系统中无法精确地表示分数 1 / 10。 这就好像十进制无法精确地表示分数 1 / 3—样。 如果在数值计算中不允许有任何舍入误差 ，就应该使用Biglnteger（整数） 和 BigDecimaL（浮点数）类 

### 2.1.1大数类型

 可以使用**BigInteger**操作大整数
可以使用**BigDecimal**指定小数的保留位数 

3*0.1==0.3 返回值是什么  false;
false，因为有些浮点数不能完全精确的表示出来(位移运算,全都是乘*2或除2)。

~~~java
//构造器                   描述
BigDecimal(int)      // 创建一个具有参数所指定整数值的对象。
BigDecimal(double)   // 创建一个具有参数所指定双精度值的对象。
BigDecimal(long)      //创建一个具有参数所指定长整数值的对象。
BigDecimal(String)   // 创建一个具有参数所指定以字符串表示的数值的对象。
~~~

~~~java
//方法                    描述
add(BigDecimal)      // BigDecimal对象中的值相加，然后返回这个对象。
subtract(BigDecimal)  //BigDecimal对象中的值相减，然后返回这个对象。
multiply(BigDecimal)  //BigDecimal对象中的值相乘，然后返回这个对象。
divide(BigDecimal)   // BigDecimal对象中的值相除，然后返回这个对象。
toString()            //将BigDecimal对象的数值转换成字符串。
doubleValue()        // 将BigDecimal对象中的值以双精度数返回。
floatValue()         // 将BigDecimal对象中的值以单精度数返回。
longValue()          // 将BigDecimal对象中的值以长整数返回。
intValue()           // 将BigDecimal对象中的值以整数返回。
~~~



~~~java
package ustc.lichunchun.bigdataapi;
 
import java.math.BigDecimal;
 
public class BigDecimalDemo01 {
 
	public static void main(String[] args) {
		System.out.println("加法运算：" + MyMath.round(MyMath.add(10.345,3.333),1)) ;
		System.out.println("减法运算：" + MyMath.round(MyMath.sub(10.345,3.333),3)) ;
		System.out.println("乘法运算：" + MyMath.round(MyMath.mul(10.345,3.333),4)) ;
		System.out.println("除法运算：" + MyMath.div(10.345,3.333,3)) ;
	}
}
class MyMath{
	public static double add(double d1,double d2){		// 进行加法计算
		BigDecimal b1 = new BigDecimal(d1) ;
		BigDecimal b2 = new BigDecimal(d2) ;
		return b1.add(b2).doubleValue() ;
	}
	public static double sub(double d1,double d2){		// 进行减法计算
		BigDecimal b1 = new BigDecimal(d1) ;
		BigDecimal b2 = new BigDecimal(d2) ;
		return b1.subtract(b2).doubleValue() ;
	}
	public static double mul(double d1,double d2){		// 进行乘法计算
		BigDecimal b1 = new BigDecimal(d1) ;
		BigDecimal b2 = new BigDecimal(d2) ;
		return b1.multiply(b2).doubleValue() ;
	}
	public static double div(double d1,double d2,int len){		// 进行除法计算
		BigDecimal b1 = new BigDecimal(d1) ;
		BigDecimal b2 = new BigDecimal(d2) ;
		return b1.divide(b2,len,BigDecimal.ROUND_HALF_UP).doubleValue() ;
	}
	public static double round(double d,int len){	// 进行四舍五入
		BigDecimal b1 = new BigDecimal(d) ;
		BigDecimal b2 = new BigDecimal(1) ; // 技巧
		return b1.divide(b2,len,BigDecimal.ROUND_HALF_UP).doubleValue() ;
	}
};
~~~

##### 注意事项:

 (推荐使用)**BigDecimal.valueOf(...)** 和 new BigDecimal(...) (**可能丢失精度**)的使用效果不同。

eg.当 BigDecimal.valueOf(...) 的入参是 float 类型时，BigDecimal 会把入参**强制**转换成 double 类型(丢失精度)。 

 new BigDecimal(double val) 方法得到的结果是不可预知的。 

~~~java
BigDecimal a = new BigDecimal(1.01);
BigDecimal b = new BigDecimal(1.02);
BigDecimal c = new BigDecimal("1.01");
BigDecimal d = new BigDecimal("1.02");
System.out.println(a.add(b));//2.0300000000000000266453525910037569701671600341796875
System.out.println(c.add(d));//2.03
~~~



### 2.1.2 类型转换
类型转换  byte、short、char-->int-->long-->float-->double    注：char只有正值
类型转换分为**自动（隐式）类型转换**和**强制类型转换** 
自动（隐式）类型转换   小转大  
强制类型转换  大转小   超出范围丢失精度

```java
//强制类型转换
int n=(int)3.14159/2     //1  
//+=自带类型强转
```


```java
short a=1;
//a= (a+1);//报错：不能从 int 转换为 short
a=(short) (a+1);
a+=1;
```

## 2.2 引用数据类型  
除了基本数据类型外的都属于引用数据类型，包括String,数组等都属于引用数据类型 注:Java中只有值传递，没有引用传递

值传递和引用传递

**值传递（pass by value）** 是指在调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到实际参数。

```java
	public class Demo{
 
   	public static void main(String[] args) {
        int a = 10;
        add(a);
        System.out.println("实际输出的a  " + a);// 输出结果为10
    }

    private static void add(int a) {
        ++a;
        System.out.println("方法里的a  " + a);// 输出结果为11
      }
	}
```

**引用传递（pass by reference）** 是指在调用函数时将实际参数的地址直接传递到函数中，那么在函数中对参数所进行的修改，将影响到实际参数。

```java

	public class Demo {

     public static void main(String[] args) {
        A a = new A(10);
        add(a);
        System.out.println(a.a);// 输出结果为10
    }

    private static void add(A a) {
        a.a += 1;
    }

    static class A {
        public int a;

        public A(int a) {
            this.a = a;
        }
    }
  }

```
equals（）和"=="的区别 参见equals的源码

```java
//Object类的equals方法是比较的地址，所以最初的equals方法和==的作用是一致的
 public boolean equals(Object anObject) {
        if (this == anObject) {
            return true;
        }
        //String类equals的实现
        if (anObject instanceof String) {
            //首先判断比较的字符串是不是本身
            String anotherString = (String) anObject;
            int n = value.length;
            //逐个字符比较内容是否相等
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


## 2.3运算符
Java 运算符
计算机的最基本用途之一就是执行数学运算，作为一门计算机语言，Java也提供了一套丰富的运算符来操纵变量。参见[菜鸟教程-运算符](https://www.runoob.com/java/java-operators.html)
提下运算符的优先级![运算符优先级](https://img-blog.csdnimg.cn/20191028215239687.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNDg4Mjk3,size_16,color_FFFFFF,t_70)



```java

```
#  3. Java 循环结构和条件语句
### 3.1 Java循环
**while 循环**

```java
while( 布尔表达式 ) {
  //循环内容
}
```
在满足条件下一直做
**do…while 循环**

```java
do {
       //代码语句
}while(布尔表达式);
```
注：布尔表达式在循环体的后面，所以语句块在检测布尔表达式之前已经执行了一次，理解为一直做直到。
**for 循环**

```java
for(初始化; 布尔表达式; 更新) {
    //代码语句
}
```
最先执行初始化步骤。可以声明一种类型，但可初始化一个或多个循环控制变量，也可以是空语句。
然后，检测布尔表达式的值。如果为 true，循环体被执行。如果为false，循环终止，开始执行循环体后面的语句。执行一次循环后，更新循环控制变量。
再次检测布尔表达式。循环执行上面的过程。
 **增强 for 循环 foreach**


```java
for(声明语句 : 表达式)
{
   //代码句子
}
```
#### 3.1.1流程控制语句

#### **3.1.2在JAVA中如何跳出当前的多重嵌套循环**  

- 外面的循环语句前定义一个标号，然后在里层循环体的代码中使用带有标号的的break语句，即可跳出  
 ~~~java
public static void main(String[] args) {
    ok:
    for (int i = 0; i < 10; i++) {
        for (int j = 0; j < 10; j++) {
            System.out.println("i=" + i + ",j=" + j);
            if (j == 5) {
                    break ok;
                }
        }
    }
}
 ~~~

- 第二种,让外层的循环条件表达式的结果可以受到里层循环体代码的控制  
 ~~~java
public static void main(String[] args) {
    int[][] arr = {{1, 2, 3}, {4, 5, 6, 7}, {9}};
    boolean found = false;
    for (int i = 0; i < arr.length && !found; i++) {
        for (int j = 0; j < arr[i].length; j++) {
            System.out.println("i=" + i + ",j=" + j);
            if (arr[i][j] == 5) {
                found = true;//找到5，使外层循环判断条件变为false则终止整个循环
                break;//跳出当前循环
            }
        }
    }
}
 ~~~


- 第三种，使用方法的return
 ~~~java
private static int test() {
    int count = 0;
    for (int i = 0; i < 10;i++) {
       for (int j = 0; j < 10;j++) {
           count++;
           System.out.println("i=" + i + ",j=" + j);
           if (j == 5) {
              return count;
           }
       }
    }
    return 0;  
 ~~~

### 3.2continue，break关键字
**break 关键字**
break 主要用在循环语句或者 switch 语句中，用来跳出整个语句块。
break 跳出最里层的循环，并且继续执行该循环下面的语句。

**continue 关键字**
continue 适用于任何循环控制结构中。作用是让程序立刻跳转到下一次循环的迭代。
在 for 循环中，continue 语句使程序立即跳转到更新语句。
在 while 或者 do…while 循环中，程序立即跳转到布尔表达式的判断语句。

#### 3.2.1死循环

死循环：循环中的条件永远为true，死循环及永不结束的循环 eg  

```
while(true){
//会一直执行循环体里的语句，直至程序终止（内存溢出等）
} 
```




### 3.2 条件判断  
#### 3.2.1  if 
~~~java
if(布尔表达式)
{
   //如果布尔表达式为true，将一直执行的语句
}  
~~~
#### 3.2.2 if...else语句  
~~~java
if(布尔表达式){
   //如果布尔表达式的值为true
}else{
   //如果布尔表达式的值为false,就执行这条语句
}  
~~~
#### 3.2.3 if... else if ...else 语句  

~~~java
if(布尔表达式 1){
   //如果布尔表达式 1的值为true执行代码
}else if(布尔表达式 2){
   //如果布尔表达式 2的值为true执行代码
}else if(布尔表达式 3){
   //如果布尔表达式 3的值为true执行代码
}else {
   //如果以上布尔表达式都不为true执行代码
} 
~~~

#### 3.2.4  嵌套的 if…else 语句  


~~~java
if(布尔表达式 1){
   ////如果布尔表达式 1的值为true执行代码
   if(布尔表达式 2){
      ////如果布尔表达式 2的值为true执行代码
   }
}
~~~
#### 3.25 三元运算符

~~~java
int c = a > b ? a:b;//a>b吗？  大与的话 c=a 否则 c=b;
~~~

### 3.3 switch case 语句      

 jdk1.7之后支持char, byte, short, int, Character, Byte, Short, Integer, String, 或者 enum注意（不支持long/double）。同时 case 标签必须为字符串常量或字面量。
~~~java
switch(expression){
    case value :
       //语句
       break; //可选
    case value :
       //语句
       break; //可选
    //你可以有任意数量的case语句
    default : //可选
       //语句
}  
~~~
#### 3.3.1 **case穿透**  
1.再都有 break的情况下 有满足条件的输出对应值  
2.部分或全部没有break的情况下，从满足条件的情况下，一直往下执行，直到break或case体结束  

~~~java  
import java.util.Scanner;

public class demo06 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int i = scanner.nextInt();
        switch (i) {
            default:
                System.out.println(7);
            case 1:
                System.out.println(1);
            case 2:
                System.out.println(2);
            case 3:
                System.out.println(4);
            case 4:
                System.out.println(4);
            case 5:
                System.out.println(5);
            case 6:
                System.out.println(6);
                break;
//输入20，输出7，1，2，3，4，5，6

        }
    }

}
~~~
## 4.类与对象


### 4.1对象和类

一个 Java 程序可以认为是一系列对象的集合，而这些对象通过调用彼此的方法来协同工作。下面简要介绍下类、对象、方法和实例变量的概念。  

对象：对象是类的一个实例（对象不是找个女朋友），有状态和行为。例如，一条狗是一个对象，它的状态有：颜色、名字、品种；行为有：摇尾巴、叫、吃等。  

类：类是一个模板，它描述一类对象的行为和状态。

#### 4.1.1访问权限修饰符

|           | **同一个类** | **同一个包** | **不同包的子类** | **不同包的非子类** |
| --------- | ------------ | ------------ | ---------------- | ------------------ |
| Private   | √            |              |                  |                    |
| Default   | √            | √            |                  |                    |
| Protected | √            | √            | √                |                    |
| Public    | √            | √            | √                | √                  |

### 4.2Java 变量类型

- 类变量：独立于方法之外的变量，用 static 修饰。  

- 实例变量：独立于方法之外的变量，没有 static 修饰。  

- 局部变量：类的方法中的变量。

### 4.2.1 Java 局部变量

- 局部变量声明在方法、构造方法或者语句块中； 

- 局部变量在方法、构造方法、或者语句块被执行的时候创建，当它们执行完成后，变量将会被销毁；  
- 访问修饰符不能用于局部变量；  

- 局部变量只在声明它的方法、构造方法或者语句块中可见；  
  局部变量是在栈上分配的。  

- 局部变量没有默认值，所以局部变量被声明后，必须经过初始化，才可以使用。


### 4.2.2 Java 实例变量

- 实例变量声明在一个类中，但在方法、构造方法和语句块之外；
- 当一个对象被实例化之后，每个实例变量的值就跟着确定；
- 实例变量的值应该至少被一个方法、构造方法或者语句块引用，使得外部能够通过这些方式获取实例变量信息；
- 实例变量可以声明在使用前或者使用后；
- 访问修饰符可以修饰实例变量；
- 实例变量对于类中的方法、构造方法或者语句块是可见的。一般情况下应该把实例变量设为私有。通过使用访问修饰符可以使实例变量对子类可见；
- 实例变量具有默认值。数值型变量的默认值是0，布尔型变量的默认值是false，引用类型变量的默认值是null。变量的值可以在声明时指定，也可以在构造方法中指定；
- 实例变量可以直接通过变量名访问。但在静态方法以及其他类中，就应该使用完全限定名：ObejectReference.VariableName。

### 4.2.2 Java 类变量（静态变量）

- 类变量也称为静态变量，在类中以 static 关键字声明，但必须在方法之外。
- 无论一个类创建了多少个对象，类只拥有类变量的一份拷贝。
- 静态变量除了被声明为常量外很少使用。常量是指声明为public/private，final和static类型的变量。常量初始化后不可改变。
- 静态变量储存在静态存储区。经常被声明为常量，很少单独使用static声明变量。
- 静态变量在第一次被访问时创建，在程序结束时销毁。
- 与实例变量具有相似的可见性。但为了对类的使用者可见，大多数静态变量声明为public类型。
- 默认值和实例变量相似。数值型变量默认值是0，布尔型默认值是false，引用类型默认值是null。变量的值可以在声明的时候指定，也可以在构造方法中指定。此外，静态变量还可以在静态语句块中初始化。
- 静态变量可以通过：ClassName.VariableName的方式访问。
- 类变量被声明为public static final类型时，类变量名称一般建议使用大写字母。如果静态变量不是public和final类型，其命名方式与实例变量以及局部变量的命名方式一致。

### 4.3 代码的加载顺序 

父类静态变量→父类静态代码块→子类静态变量→子类静态代码块→父类非静态变量→父类非静态代码块→父类构造函数→子类非静态变量→子类非静态代码块→子类构造函数  

即：  

静态>非静态  

父类>子类

变量>代码块>构造函数(方法)

~~~java
class A {
//    静态域(静态变量、静态块和静态方法  谁在前面谁先执行)>非静态
//    父类>子类
//    变量/代码块>构造函数(即构造方法)
    public A(){
        System.out.println("父类构造块");
    }

    {
        System.out.println("父类块");
    }

    static {
        System.out.println("父类静态块");

    }

}
class demo extends A{

    public demo(){
        System.out.println("子类构造块");
    }
    {
        System.out.println("子类块");
    }
    static {
        System.out.println("子类静态块");
    }


    public static void main(String[] args) {
        A a = new A();
        new demo();
    }//结果：父类静态块 子类静态块 父类块 父类构造块 父类块 父类构造块 子类块 子类构造块
}
~~~



## 5  java特性详解

### 5.1 封装

就是把同一类事物的属性和方法归到同一个类中，方便使用 

- 防止该类的代码和数据被外部类定义的代码随意访问 

- 要访问该类的数据和代码必须通过严格的方法控制 

- 封装的主要功能在于我们能修改自己的实现代码，而不用修改哪些调用程序的代码片段。

优点：减少耦合，类内部自由修改，可以对类成员变量进行更精确的控制，隐藏信息、实现细节。

实践： 

为了实现良好的封装，通常将类的成员变量声明为private ,通过public的set和get方法完成对属性的操作

### 5.2继承

继承就是子类继承父类的特征和行为，使得子类对象（实例）具有父类的实例域和方法

特性： 

**子类拥有父类的非private属性，方法  只是不能调用**

- 子类可以拥有自己的属性和方法，即子类可以对父类进行扩展 

- 子类可以用自己的方式实现父类的方法 

- java的继承是单继承（）但可以多重继承 

**好处**:

1. 提高代码的复用性
2. 类与类之间产生了关系，是多态的前提

**弊端**:类的耦合性加强了

### 5.3 多态

   　一个对象的多种表现形态

　　使用父类类型的引用指向子类的对象；

　　该引用只能调用父类中定义的方法，不能调用子类中独有的方法。

如果子类中重写了父类中的一个方法，那么在调用这个方法的时候。将会调用子类中重写之后的放发

　　子类可以调用父类中所有的方法。

 

　　使用继承不仅可以重用已有的代码，从而避免代码重复，还可以创建一个更容易维护和修改代码的程序。

### 5.4 抽象

​     Java中抽象可以是抽象类、抽象方法，使用abstract修饰。其中抽象类中可以有抽象方法和非抽象方法，抽象方法。**含有抽象方法的类一定是抽象类**。抽象方法一般只有方法名，没有方法的实现。如：public abstract void test(); 且**抽象方法的权限修饰必须是protected/public** 的，因为子类在继承抽象父类的时候，必须要实现父类所有的抽象方法。

#### 5.4.1重载(overload)和override（重写）的区别

 **方法重载**是让类以统一的方式处理不同类型数据的一种手段。多个同名函数同时存在，具有不同的参数个数/类型。

**重载**是一个类中多态性的一种表现。

调用方法时通过传递给它们的**不同参数个数和参数类型**来决定具体使用哪个方法, 这就是多态性。

重载的时候，方法名要一样，但是参数类型和个数不一样，返回值类型可以相同也可以不相同。**无法以返回型别作为重载函数的区分标准。**

**override**（重写）规则

- 方法名、参数、返回值相同 
- 子类方法不能缩小父类方法的访问权限（public>protected>default>private）。
- 子类方法不能抛出比父类方法更多的异常(但子类方法可以不抛出异常)
- 存在于父类和子类之间。
- 方法被定义为final不能被重写。
- 返回的类型必须一直与被重写的方法的返回类型相同，否则不能称其为重写而是重载。

　**overload**（重载）规则

　- 参数类型、个数、顺序至少有一个不相同。 
　- 不能重载只有返回值不同的方法名。
　- 存在于父类和子类、同类中。

## 5.异常

![img](https://www.runoob.com/wp-content/uploads/2013/12/12-130Q1234I6223.jpg)

### 异常分类

- 检查性异常：**最具代表的检查性异常是用户错误或问题引起的异常，这是程序员无法预见的。例如要打开一个不存在文件时，一个异常就发生了，这些异常在编译时不能被简单地忽略。
- **运行时异常(RuntimeException)：** 运行时异常是可能被程序员避免的异常。与检查性异常相反，运行时异常可以在编译时被忽略。
- **错误(error)：** 错误不是异常，而是脱离程序员控制的问题。错误在代码中通常被忽略。例如，当栈溢出时，一个错误就发生了，它们在编译也检查不到的。

### 异常处理

Java异常处理涉及到五个关键字，分别是：`try`、`catch`、`finally`、`throw`、`throws`。下面将骤一介绍，通过认识这五个关键字，掌握基本异常处理知识。

　　• **try**     -- 用于监听。将要被监听的代码(可能抛出异常的代码)放在try语句块之内，当try语句块内发生异常时，异常就被抛出。
　　• **catch**  -- 用于捕获异常。catch用来捕获try语句块中发生的异常。
　　• **finally** -- finally语句块总是会被执行。它主要用于回收在try块里打开的物力资源(如数据库连接、网络连接和磁盘文件)。只有finally块，执行完成之后，才会回来执行try或者catch块中的return或者throw语句，如果finally中使用了return或者throw等终止方法的语句，则就不会跳回执行，直接停止。
　　• **throw**  -- 用于抛出异常。
　　• **throws** -- 用在方法签名中，用于声明该方法**可能抛出**的异常。

### finally的执行顺序

 **finally块的语句在try或catch中的return语句执行之后返回之前执行且finally里的修改语句可能影响也可能不影响try或catch中 return已经确定的返回值，若finally里也有return语句则覆盖try或catch中的return语句直接返回。** 

### 常见异常

**1、java.lang.NullPointerException(空指针异常)**
　 调用了未经初始化的对象或者是不存在的对象

经常出现在创建图片，调用数组这些操作中，比如图片未经初始化，或者图片创建时的路径错误等等。对数组操作中出现空指针，
即把数组的初始化和数组元素的初始化混淆起来了。数组的初始化是对数组分配需要的空间，而初始化后的数组，其中的元素并没有实例化，
依然是空的，所以还需要对每个元素都进行初始化（如果要调用的话）。

------

**2、 java.lang.ClassNotFoundException**

**指定的类不存在**

这里主要考虑一下类的名称和路径是否正确即可，通常都是程序试图通过字符串来加载某个类时可能引发 异常

比如：调用Class.forName();

```
    或者调用ClassLoad的finaSystemClass();或者LoadClass();
```

------

**3、 java.lang.NumberFormatException**

**字符串转换为数字异常**

当试图将一个String转换为指定的数字类型，而该字符串确不满足数字类型要求的格式时，抛出该异常.如现在讲字符型的数据“123456”转换为数值型数据时，是允许的。

但是如果字符型数据中包含了非数字型的字符，如123#56，此时转换为数值型时就会出现异常。系统就会捕捉到这个异常，并进行处理.

------

**4、java.lang.IndexOutOfBoundsException**

**数组下标越界异常**

查看调用的数组或者字符串的下标值是不是超出了数组的范围，一般来说，显示（即直接用常数当下标）调用不太容易出这样的错，但隐式（即用变量表示下标）调用就经常出错了，还有一种情况，是程序中定义的数组的长度是通过某些特定方法决定的，不是事先声明的，这个时候，最好先查看一下数组的length，以免出现这个异常。

------

**5、java.lang.IllegalArgumentException**

**方法的参数错误**

比如g.setColor(int red,int green,int blue)这个方法中的三个值，如果有超过２５５的也会出现这个异常，因此一旦发现这个异常，我们要做的，就是赶紧去检查一下方法调用中的参数传递是不是出现了错误。

------

**6、java.lang.IllegalAccessException**

**没有访问权限**

当应用程序要调用一个类，但当前的方法即没有对该类的访问权限便会出现这个异常。对程序中用了Package的情况下要注意这个异常

------

**7、java.lang.ArithmeticException**

**数学运算异常**

当算术运算中出现了除以零这样的运算就会出这样的异常。

**8、java.lang.ClassCastException**

**数据类型转换异常**

当试图将对某个对象强制执行向下转型，但该对象又不可转换又不可转换为其子类的实例时将引发该异常，如下列代码。

```java
                         Object obj = new Integer(0);java

                         String str = obj;
123
```

------

**9、 java.lang.FileNotFoundException**

**文件未找到异常**

当程序试图打开一个不存在的文件进行读写时将会引发该异常。该异常由FileInputStream,FileOutputStream,RandomAccessFile的构造器声明抛出

即使被操作的文件存在，但是由于某些原因不可访问，比如打开一个只读文件进行写入，这些构造方法仍然会引发异常

------

**10、java.lang.ArrayStoreException**

**数组存储异常**

当试图将类型不兼容类型的对象存入一个Object[]数组时将引发异常

```
                      Object[] obj = new String[3];

                      obj[0] = new Integer(0);
123
```

------

**11、java.lang.NoSuchMethodException**

**方法不存在异常**

当程序试图通过反射来创建对象，访问(修改或读取)某个方法，但是该方法不存在就会引发异常

------

**12、 java.lang.NoSuchFiledException**

**方法不存在异常**

当程序试图通过反射来创建对象，访问(修改或读取)某个filed，但是该filed不存在就会引发异常

------

**13、 java.lang.EOFException**

**文件已结束异常**

当程序在输入的过程中遇到文件或流的结尾时，引发异常。因此该异常用于检查是否达到文件或流的结尾

------

**14、java.lang.InstantiationException**

**实例化异常**

当试图通过Class的newInstance()方法创建某个类的实例,但程序无法通过该构造器来创建该对象时引发

Class对象表示一个抽象类，接口，数组类，基本类型
该Class表示的类没有对应的构造器

------

**15、java.lang.InterruptedException**

被中止异常

当某个线程处于长时间的等待、休眠或其他暂停状态，而此时其他的线程通过Thread的interrupt方法终止该线程时抛出该异常。

------

**16、java.lang.CloneNotSupportedException**
**不支持克隆异常**

当没有实现Cloneable接口或者不支持克隆方法时,调用其clone()方法则抛出该异常。

**17、java.lang.OutOfMemoryException**
**内存不足错误**

当可用内存不足以让Java虚拟机分配给一个对象时抛出该错误。

------

**18、java.lang.NoClassDefFoundException**
**未找到类定义错误**

当Java虚拟机或者类装载器试图实例化某个类，而找不到该类的定义时抛出该错误。

违背安全原则异常：SecturityException

操作数据库异常：SQLException

输入输出异常：IOException

通信异常：SocketException

