## static

> static 关键字可用于变量、方法、代码块和内部类，表示某个特定的成员只属于某个类本身，而不是该类的某个对象。

### 01、静态变量

静态变量也叫类变量，它属于一个类，而不是这个类的对象。 静态变量的值都会在**所有类的对象中共享**. 

1）由于静态变量属于一个类，所以不要通过对象引用来访问，而应该**直接通过类名来访问**；

2）**不需要初始化类**就可以访问静态变量。



### 02、静态方法

 静态方法也叫类方法，它和静态变量类似，属于一个类，而不是这个类的对象。 

1）Java 中的静态方法在编译时解析，因为静态方法不能被重写（方法重写发生在运行时阶段，为了多态）。

2）抽象方法不能是静态的。



![img](https://user-gold-cdn.xitu.io/2020/6/5/172821531482ce65?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



3）静态方法不能使用 this 和 super 关键字。

4）成员方法可以直接访问其他成员方法和成员变量。

5）成员方法也可以直接方法静态方法和静态变量。

6）静态方法可以访问所有其他静态方法和静态变量。

7）静态方法无法直接访问成员方法和成员变量。



### 03、静态代码块

静态代码块可以用来初始化静态变量，尽管静态方法也可以在声明的时候直接初始化，但有些时候，我们需要多行代码来完成初始化。

### 04、静态内部类

Java 允许我们在一个类中声明一个内部类，它提供了一种令人信服的方式，允许我们只在一个地方使用一些变量，使代码更具有条理性和可读性。

常见的内部类有四种，成员内部类、局部内部类、匿名内部类和静态内部类，限于篇幅原因，前三种不在我们本次文章的讨论范围，以后有机会再细说。

```java
public class Singleton {
    private Singleton() {}

    private static class SingletonHolder {
        public static final Singleton instance = new Singleton();
    }

    public static Singleton getInstance() {
        return SingletonHolder.instance;
    }
}

```

以上这段代码是不是特别熟悉，对，这就是创建单例的一种方式，第一次加载 Singleton 类时并不会初始化 instance，只有第一次调用 `getInstance()` 方法时 Java 虚拟机才开始加载 SingletonHolder 并初始化 instance，这样不仅能确保线程安全也能保证 Singleton 类的唯一性。不过，创建单例更优雅的一种方式是使用枚举。

简单小结一下：

1）静态内部类不能访问外部类的所有成员变量。

2）静态内部类可以访问外部类的所有静态变量，包括私有静态变量。

3）外部类不能声明为 static。



## final

 final可以修饰变量，方法和类，用于表示所修饰的内容一旦赋值之后就不会再被改变，比如String类就是一个final类型的类 .

### final修饰成员变量

```java
public class FinalExample {    //声明变量的时候，就进行初始化
    private final int num=6;    //类变量必须要在静态初始化块中指定初始值或者声明该类变量时指定初始值
    // private final String str; //编译错误：因为非静态变量不可以在静态初始化快中赋初值
    private final static String name;    private final double score;    private final char ch;    //private final char ch2;//编译错误:TODO：因为没有在构造器、初始化代码块和声明时赋值
    
    {        //实例变量在初始化代码块赋初值
        ch='a';
    }    
    static {
        name="aaaaa";
    }    
    public FinalExample(){        //num=1;编译错误：已经赋值后，就不能再修改了
        score=90.0;
    }    
    public void ch2(){        //ch2='c';//编译错误：实例方法无法给final变量赋值
    }
}
```

- 类变量：必须要在静态初始化块中指定初始值或者声明该类变量时指定初始值，而且只能在这两个地方之一进行指定
- 实例变量：必要要在非静态初始化块，声明该实例变量或者在构造器中指定初始值，而且只能在这三个地方进行指定

### final修饰局部变量

final局部变量由程序员进行显式初始化， 如果final局部变量已经进行了初始化则后面就不能再次进行更改， 如果final变量未进行初始化，可以进行赋值，当且仅有一次赋值，一旦赋值之后再次赋值就会出错。

~~~java
public void test(){    final int a=1;    //a=2;//编译错误：final局部变量已经进行了初始化则后面就不能再次进行更改}
~~~

 final修饰的是引用数据类型 可以修改值(地址值不变)

### final修饰方法

> 重写(Override)

被final修饰的方法不能够被子类所重写。 比如在Object中，getClass()方法就是final的，我们就不能重写该方法， 但是hashCode()方法就不是被final所修饰的，我们就可以重写hashCode()方法。

> 重载(Overload)

被final修饰的方法是可以重载的

### final修饰类

当一个类被final修饰时，该类是不能被子类继承的。 子类继承往往可以重写父类的方法和改变父类属性，会带来一定的安全隐患， 因此，当一个类不希望被继承时就可以使用final修饰。

> 不可变类

final经常会被用作不变类上。我们先来看看什么是不可变类：

- 使用private和final修饰符来修饰该类的成员变量
- 提供带参的构造器用于初始化类的成员变量
- 仅为该类的成员变量提供getter方法，不提供setter方法，因为普通方法无法修改fina修饰的成员变量
- 如果有必要就重写Object类的hashCode()和equals()方法，应该保证用equals()判断相同的两个对象其Hashcode值也是相等的。

JDK中提供的八个包装类和String类都是不可变类。



## this/super

**this**

this是自身的一个对象，代表对象本身，可以理解为：指向对象本身的一个指针。

this的用法在java中大体可以分为3种：

### **1.普通的直接引用**

指向当前对象本身。

### **2.形参与成员名字重名，用this来区分：**

```java
`class` `Person {``  ``private` `int` `age = ``10``;``  ``public` `Person(){``  ``System.out.println(``"初始化年龄："``+age);``}``  ``public` `int` `GetAge(``int` `age){``    ``this``.age = age;``    ``return` `this``.age;``  ``}``}` `public` `class` `test1 {``  ``public` `static` `void` `main(String[] args) {``    ``Person Harry = ``new` `Person();``    ``System.out.println(``"Harry's age is "``+Harry.GetAge(``12``));``  ``}``}    `
```

**运行结果：**

**初始化年龄：10**
**Harry's age is 12**

可以看到，这里age是GetAge成员方法的形参，this.age是Person类的成员变量。

### **3.引用构造函数**

这个和super放在一起讲，见下面。

**super**

super可以理解为是指向自己超（父）类对象的一个指针，而这个超类指的是离自己最近的一个父类。

super也有三种用法：

### 1.普通的直接引用

与this类似，super相当于是指向当前对象的父类，这样就可以用super.xxx来引用父类的成员。

### 2.子类中的成员变量或方法与父类中的成员变量或方法同名

~~~java
class Country {
    String name;
    void value() {
       name = "China";
    }
}
  
class City extends Country {
    String name;
    void value() {
    name = "Shanghai";
    super.value();      //调用父类的方法
    System.out.println(name);
    System.out.println(super.name);
    }
  
    public static void main(String[] args) {
       City c=new City();
       c.value();
       }
}
~~~

**运行结果：**

**Shanghai**
**China**

 可以看到，这里既调用了父类的方法，也调用了父类的变量。若不调用父类方法value()，只调用父类变量name的话，则父类name值为默认值null。 

### 3.引用构造函数

super（参数）：调用父类中的某一个构造函数（应该为构造函数中的第一条语句）。

this（参数）：调用本类中另一种形式的构造函数（应该为构造函数中的第一条语句）。

~~~java
class Person { 
    public static void prt(String s) { 
       System.out.println(s); 
    } 
   
    Person() { 
       prt("父类·无参数构造方法： "+"A Person."); 
    }//构造方法(1) 
    
    Person(String name) { 
       prt("父类·含一个参数的构造方法： "+"A person's name is " + name); 
    }//构造方法(2) 
} 
    
public class Chinese extends Person { 
    Chinese() { 
       super(); // 调用父类构造方法（1） 
       prt("子类·调用父类”无参数构造方法“： "+"A chinese coder."); 
    } 
    
    Chinese(String name) { 
       super(name);// 调用父类具有相同形参的构造方法（2） 
       prt("子类·调用父类”含一个参数的构造方法“： "+"his name is " + name); 
    } 
    
    Chinese(String name, int age) { 
       this(name);// 调用具有相同形参的构造方法（3） 
       prt("子类：调用子类具有相同形参的构造方法：his age is " + age); 
    } 
    
    public static void main(String[] args) { 
       Chinese cn = new Chinese(); 
       cn = new Chinese("codersai"); 
       cn = new Chinese("codersai", 18); 
    } 
}
~~~

**运行结果：**

**父类·无参数构造方法： A Person.**
**子类·调用父类”无参数构造方法“： A chinese coder.**
**父类·含一个参数的构造方法： A person's name is codersai**
**子类·调用父类”含一个参数的构造方法“： his name is codersai**
**父类·含一个参数的构造方法： A person's name is codersai**
**子类·调用父类”含一个参数的构造方法“： his name is codersai**
**子类：调用子类具有相同形参的构造方法：his age is 18**



## transient

1）一旦变量被transient修饰，变量将不再是对象持久化的一部分，该变量内容在序列化后无法获得访问。

2）transient关键字**只能修饰变量**，而不能修饰方法和类。注意，本地变量是不能被transient关键字修饰的。变量如果是用户自定义类变量，则该类需要实现Serializable接口。

3）被transient关键字修饰的变量**不再能被序列化**，一个静态变量不管是否被transient修饰，均不能被序列化。

