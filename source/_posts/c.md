---
title: c#
date: 2023-05-29 15:39:15
tags: 工程
categories: 工程
typora-root-url: c
description: "记录c#的基础知识"
swiper_index: 8
---

# C#

> 阅读c#本质论的一些笔记。c#在语法结构上，和C语言没啥两样。所以说这里只提到我觉得有意思的一些语法结构。

## 基础

> 提到一些c#的基础操作

### 插值法

在输出处，用`$和{}`对应，`$`修饰的冒号语句中的`{}`可插入变量，例如：

```c#
string firstname;	
System.Console.WriteLine($"{firstname}");
```

### 数据类型转化

分为显示和隐式：

- 显示

  ```c#
  long a = 21345623030;
  int b = (int) a;
  ```

- 隐式

  ```c#
  int o = 123456789;
  long l = o;
  ```


也可以不用这两种方法，可以使用Parse()。当然，也可以使用TryParse()方法，这个方法在失败的时候会返回false.

  ```c#
  string text = "123486";
  float t = float.Parse(text);
  ```

常见的`ToString`是将其他数据类型转化为字符串

```c#
bool lean = true;
string text = lean.ToString();
```

### OUT

常规函数中，只能`return`一个值，但是**out参数**可以帮助我们在一个方法中**返回多个值**，**不限类型**。

**在使用out参数的时候需要注意**：

  - 在调用方法之前，对out参数传递的变量**只需声明**，可以赋值也可以不赋值，赋值也会在方法中被覆盖掉。
  - 方法使用了out参数传递变量时，就是要返回值，所以必须在**方法内为其赋值**。这个性质类似于局部变量。
  - 方法的参数使用out关键字修饰时，在调用该方法时，也要在out参数传递的变量前面加上**out关键字**才行。

```c#
using System;

class Program
{
    static void ProcessValue(int input, out int doubledValue)
    {
        // out 参数在方法内部必须在所有执行路径上都被显式赋值
        doubledValue = input * 2;
    }

    static void Main()
    {
        int inputValue = 5;
        int result;

        // 调用带有 out 参数的方法
        ProcessValue(inputValue, out result);

        // 输出结果
        Console.WriteLine($"Doubled value of {inputValue} is {result}");
    }
}

```

### 值类型和引用类型

这个两个类型，需要扯到内存。简单来说，值类型存储的位置就是内存的指定位置；引用类型，存储的是存储这个值内存的位置，也就是地址。引用类型存储的是地址，值类型存储的是值。

![image-20231204131804165](image-20231204131804165.png)

引用变量可以不像c++，这里用的变量修饰符ref，用这个来修饰的变量可以称之为引用类型。

### [元组](https://www.zhihu.com/question/28600808)

允许一条语句中完成所有变量的赋值：![image-20231204135416803](/image-20231204135416803.png)

![image-20231204135434082](image-20231204135434082.png)

### 偏移量

是数学的一种概念，可以在数组中清晰的显示出来。先给一个基准位置，例如在数组中，我们将0这个位置设置为基准位置，在之后增加的序列号可以理解为对于基准位置的偏移量。

![image-20231204141104689](image-20231204141104689.png)

### 数组

和c语言的数组没什么区别，只是在结构命名上有差别。

![image-20231204141604427](image-20231204141604427.png)

![image-20231204141620743](image-20231204141620743.png)

### 预处理

就是宏，将一些包装好的变量和函数，变为另一个名字。

![image-20231204180235251](image-20231204180235251.png)

## 类

类和对象都能关联数据。将类想象成模具，将对象想象成根据该模具浇铸的零件，可以更好地理解这一点。

例如，一个模具拥有的数据可能包括：到目前为止已用模具浇铸的零件数、下个零件的序列号、当前注入模具的液态塑料的颜色以及模具每小时生产零件数量。类似地，零件也拥有它自己的数据：序列号、颜色以及生产日期/时间。虽然零件颜色就是生产零件时在模具中注入的塑料的颜色，但它显然不包含模具中当前注入的塑料颜色数据，也不包含要生产的下个零件的序列号数据。

面向对象标志，作用类似于结构体，但实际上比结构体强上不少。具有三种性质，封装，继承，多态。一个普通类的命名如下：

```c#
Class Employee{
    
}
```

常见的数据类型也是一种类，我们声明的变量其实算是类的对象。这种操作就是类的实例化

```c#
Class Employee{
    
}

Class Program{
    static void Main()
    {
        Employee em = new Employee();
    }
}
```

这里的new操作符起到了分配内存的作用，在等于号的右侧其实是类的构造函数。

### 构造函数

构造函数是给类对象初始值的一个函数，会在命名对象的时候调用。举个例子：人类一出生就会呼吸，这个能力就是由构造函数赋予的（可能不太准确）。构造函数的名称和类的名称是一样的，下面是一个简单的例子：构造函数是我们的A，作用是对于A中的两个变量赋值。

```c#
Class A{
	int a;
	int b;
	
	A(){
		a = 1;
		b = 2;
	}

}
```

当然，没有声明构造函数的话，系统也有一个默认的构造函数供我们使用。

### 封装

类的封装性质体现在，可以对变量或者方法进行私有声明。私有声明之后的变量只能在该类中使用，这样做的优点是，不会产生变量同名的冲突。私有变量用关键字`private`修饰，与之对应不是私有变量的修饰关键字是`public`。如果在类中，不对声明的变量采用修饰词，默认是`private`。

```c#
Class A
{
	private int a;
	public int b;
}

Class Program{
    static void Main()
    {
        A a = new A();
        //a.a 是不能调用的
        a.b = 10;
    }
}
```

这里拓展一下修饰符：public、private、protected、internal、protected internal和private protected。常用的也就是我们上面提到的两个。

### This关键字

C#允许用关键字this显式指出当前访问的字段或方法是包容类的实例成员。引用当前类的实例的变量，确定是类的成员变量而不是局部变量。

```c#
public class Employee{
	public string FirstName;
    public string LastName;
    public string? Salary;
    
    public string GetName(){
        return $"{ FirstName } { LastName }";
    }
    
    public void SetName(string Fname,string Lname){
        //this指出当前类的变量
        this.FirstName = Fname;
        this.LastName = Lname;
    }
}
```

### 属性

属性相当于给私有变量开个小门，让我们能对其进行修改。属性有两个部分组成，获取值和赋予值，下面是一个简单的声明属性的操作：

![image-20231204191018307](image-20231204191018307.png)

![image-20231204191037958](image-20231204191037958.png)

属性可以在vs中快速构造，按住ALT。

### [静态](https://zhuanlan.zhihu.com/p/46362682)

> 好泛，不懂

静态修饰需要提到内存，定义的静态变量和方法特点之一是会在全局数据区域分配内存，这就表示了对应的静态变量不会被改变值，类似于c的全局变量。

C#没有全局变量或全局函数。C#的所有字段和方法都在类的上下文中。在C#中，与全局字段或函数等价的是静态字段或方法。“全局变量/函数”和“C#静态字段/方法”在功能上没有差异，只是静态字段/方法可包含访问修饰符（比如private），从而限制访问并提供更好的封装。

```c#
//不知道咋写
```

![image-20231204204139372](image-20231204204139372.png)

`static`方法是类中的一个成员方法,属于整个类,即不用创建任何对象也可以直接调用 ! `static`内部只能出现static变量和其他static方法!而且static方法中还不能使用this....等关键字..因为它是属于整个类!

### `Base`关键字

`base`关键字和`This`关键字功能类似，用于提示我们当前的变量是属于哪个部分的，base强调调用的是基类的成员和方法。使得代码可读性增强。

![image-20231204212524513](image-20231204212524513.png)

### 重写基类

C#支持重写实例方法和属性，但不支持字段和任何静态成员的重写。为进行重写，要求在基类和派生类中都显式执行一个操作。基类必须将允许重写的每个成员都标记为virtual。如一个public或protected成员没有包含virtual修饰符，就不允许子类重写该成员。这类似于c++的虚方法。

### 抽象类

抽象类的大多数功能通常都没有实现。一个类要从抽象类成功地派生，必须为抽象基类中的抽象方法提供具体的实现。也就是说，实现在继承抽象类的类，抽象类不负责实现类的方法。抽象类要求为类定义添加abstract修饰符。从这些推导出，抽象类只能是基类。

![image-20231204221955700](image-20231204221955700.png)

### Is关键字

用于判断变量的类型，是一个匹配操作符。返回的是bool值。is操作符的优点是允许验证一个数据项是否属于特定类型，常用于判断一个对象是否兼容于其他指定的类型然后用as操作符进行转化。

![image-20231205104217403](image-20231205104217403.png)

### as操作符

as操作符更进一步，它会像一次转型所做的那样，尝试将对象转换为特定数据类型。可以理解为as操作符是用于类型转换的。`as` 只能用于引用类型不能用于值类型;

```c#
object o = new object();  
 
 Label lb = o as Label;   // as 操作符
 
 if (lb == null)  
 {  
     Response.Write("类型转换失败");  
 }  
 else 
 {  
     Response.Write("类型转换成功");  
 } 
```

as也可以转换对象，可以用于尝试将一个对象转换为另一个类或接口类型。前提是目标类型必须是源类型的基类、接口或派生类。

```c#
class Animal
{
    public void Eat()
    {
        Console.WriteLine("Animal is eating.");
    }
}

class Dog : Animal
{
    public void Bark()
    {
        Console.WriteLine("Dog is barking.");
    }
}

class Program
{
    static void Main()
    {
        Animal animal = new Dog();
        //只能转换成派生类的对象
        Dog dog = animal as Dog;

        if (dog != null)
        {
            dog.Bark();
        }
        else
        {
            Console.WriteLine("Conversion failed.");
        }
    }
}
```

### [接口](https://www.zhihu.com/question/430902220)

C#放弃了多重继承，采用了单继承操作。但是这样会导致需求比较复杂时捉襟见肘。采用接口的方式代替多重继承。接口的意义是可以让不同的类型具有相同的操作标准。就像插头，两孔是一个接口，实现这个接口就可以插到两孔的插座上，三孔是另一个接口。如果同时实现这两个接口就可以既插到两孔插头也可以插到三孔插头。

> 接口的意义就是实现最大的封装，当两个实现类针对一个接口的时候，就叫互为多态。
>
> 总结：1.规范 2.多态

![image-20231205112412961](image-20231205112412961.png)

“继承”本身是有局限性的。如果我们把需要关心的“功能和特性”列出表来，实际上是这样的：

![img](v2-33b195f52fc6d296eec6b672d388fb33_720w.webp)

单纯从一个角度分类，远远不如像这样把功能摊开的好。如果把左边的每个`可XXXX`看做一种接口（**接口这个名词翻译得有问题，应当理解为“满足的协议”或“承诺可进行的操作”**）

```c#
// 可按下按键的“协议”
    interface IPressButton
    {
        // 按下某个按键
        void Press(int button);
    }
    // 可插USB的“协议”
    interface IUSB
    {
        // 插USB
        void PlugUSB();
    }
```

所有的具体类型，要根据是否满足协议来设计：

```c#
class 键盘 : IPressButton, IUSB
    {
        public void Press(int b)
        {
            Console.WriteLine("按下键盘"+b+"键");
        }
        public void PlugUSB() { }
    }
    class 鼠标 : IPressButton, IUSB
    {
        public void Press(int b)
        {
            Console.WriteLine("按下鼠标"+b+"键");
        }
        public void PlugUSB() { }
    }
    class 显示器 : IPressButton
    {
        public void Press(int b)
        {
            Console.WriteLine("按下显示器"+b+"键");
        }
    }
    class 手机 : IPressButton, IUSB
    {
        public void Press(int b) {
            Console.WriteLine("按下手机"+b+"键");
        }
        public void PlugUSB() { }
    }
```

### 枚举

类似于Switch语句，定义的枚举其实是一种变量：

```c#
enum Con{
	a,
	b
}
```

通过`.`操作符，可以像数组一样来枚举变量内部的值。

## [委托](https://www.jb51.net/article/103224.htm)

[C#中的委托](https://www.jianshu.com/p/674da938f3d0)是用来实现传递回调函数的。回调函数是什么，举个例子

> 你到一个商店买东西，刚好你要的东西没有货，于是你在店员那里留下了你的电话，过了几天店里有货了，店员就打了你的电话，然后你接到电话后就到店里去取了货。在这个例子里，你的电话号码就叫回调函数，你把电话留给店员就叫登记回调函数，店里后来有货了叫做触发了回调关联的事件，营业员直接帮你买了，送到你家，叫做调用回调函数，你到店里去取货叫做响应回调事件。

`C#`是一门强类型的语言。函数的参数必须要有严格的类型。而且也是不允许直接把函数作为另一个函数的参数的。委托的功能就是将一个函数作为另一个函数的参数传递。

下面用js的逻辑写一个例子：

```c#
using System;

namespace Test
{
    class Test
    {
        public Test()
        {
            Calculate(23, 45, ShowText);
        }

        public void Calculate(int num1, int num2, callback)
        {
            int sum = num1 + num2;
            callback(sum.ToString());
        }

        public void ShowText(string text)
        {
            Console.WriteLine(text);
        }
    }
}
```

直接把`ShowText`这个函数名传递给`Calculate`方法显然在C#中是会报错的。这时我们就需要用到委托`delegate`。于是我们添加添加一个委托，如下：

```c#
using System;

namespace Test
{
    class Test
    {
        public Test()
        {
            ShowTextDel showTextDel = ShowText;  // 委托就像方法的容器，用户存储方法。
            Calculate(23, 45, showTextDel);
        }

        public void Calculate(int num1, int num2, ShowTextDel callback)
        {
            int sum = num1 + num2;
            callback(sum.ToString());
        }

        // 定义一个只有一个`string`类型参数返回值为`void`的方法的委托。
        public delegate void ShowTextDel(string text);  

        public void ShowText(string text)
        {
            Console.WriteLine(text);
        }
    }
}
```

有了委托就可以在`C#`中传递回调函数了。但委托有如下的一些注意点：

1. 既然委托作为函数或方法的容器，那么在强类型的C#中就需要函数或方法和委托具有相同的结构（**即委托与函数或方法必须有相同的参数列表和相同的返回值**）。
2. 委托既然作为函数或方法的容器，一个委托可以容纳多个函数或方法。在调用这个委托的时候会依次调用这个委托中的各个方法。具体可以参考微软的官方文档：[使用委托（C# 编程指南）](https://link.jianshu.com?t=https://msdn.microsoft.com/zh-cn/library/ms173172.aspx)

### 匿名函数

没有方法名字的函数，称之为匿名函数。但是没有名称我们怎么调用？

主要用于简化委托，常常与Lambda表达式结合。

![image-20240201202033173](image-20240201202033173.png)

### [Lambda表达式 ](https://zhuanlan.zhihu.com/p/257275006)

就是匿名方法的另一种表达方式，简化方法体。

```c#
//方法只有一行的情况
MyDelegate.Say("张三", (name) => Console.WriteLine("你好，" + name));
```

## [事件](https://www.cnblogs.com/SkySoot/archive/2012/04/05/2433639.html)

**声明一个事件不过类似于声明一个进行了封装的委托类型的变量而已**。

## [异步](https://www.cnblogs.com/yilezhu/p/12045018.html)

## [线程](https://www.cnblogs.com/dotnet261010/p/6159984.html)

https://www.cnblogs.com/Survivalist/p/11527949.html

[C#多线程学习总结(持续更新中) - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/119074326)

## 反射

## https://www.cnblogs.com/wangshenhe/p/3256657.html

