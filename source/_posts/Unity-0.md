---
title: Unity
date: 2024-02-17 20:45:58
tags: unity
categories: unity
typora-root-url: ./Unity-0
---

# Unity

> 本篇简单介绍c#和unity的部分组件

## 组件脚本

Unity的界面布局可以自定义，这里简单介绍一下界面的功能

![image-20240218120534355](image-20240218120534355.png)

- `Hierarchy`(阶层): 显示在场景中的所有物品
- `Project`: 显示unity中所有需要的组件
- `Console`: 运行输出
- `Inspector`(检查): 显示物品搭载脚本的信息

需要注意的是，`Inspector`中显示的组件可以改变物体的状态，这里简单举个例子：`Rigidbody`可以赋予物体物理性质。

![image-20240218121237089](image-20240218121237089.png)

同样的我们可以创建c#脚本控制物体，下面就编写一个脚本改变在场景中正方体的颜色。

```c#
using UnityEngine;
using System.Collections;

public class ExampleBehaviourScript : MonoBehaviour
{
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.R))
        {
            GetComponent<Renderer> ().material.color = Color.red;
        }
        if (Input.GetKeyDown(KeyCode.G))
        {
            GetComponent<Renderer>().material.color = Color.green;
        }
        if (Input.GetKeyDown(KeyCode.B))
        {
            GetComponent<Renderer>().material.color = Color.blue;
        }
    }
}
```

打开脚本需要`IDE`，这里需要绑定一下我们的VS，点击`edit` -> `preferences` -> `External Tools` -> `external script editor`。

![image-20240218134510645](image-20240218134510645.png)

编辑完之后我们就可以改变正方体的颜色了，这里需要注意的是需要搭载Mesh组件，Mesh组件是用于渲染网格的，在这里可以理解为渲染物体的材质。

![image-20240218135515812](image-20240218135515812.png)

> `GetComponent<>()` : 这个组件理解为获取游戏对象上附加`<>`的组件

![image-20240218140159399](image-20240218140159399.png)

![image-20240218140214926](image-20240218140214926.png)

## 变量函数

初始情况下，创建一个脚本会有两个预设函数。`Start` 在运行游戏的时候只会执行一次。`Update`会按照帧率来执行写入函数之中的内容

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class colo : MonoBehaviour
{
    // 用于对象的初始化
    void Start()
    {
        
    }

    // 按照帧来执行的函数
    void Update()
    {
        
    }
}
```

写入函数很常见，写入变量也很常见。但是需要按照一定的逻辑，减少耦合度。所以说掌握设计模式很重要。Unity也自带了一些函数，比较常用的也就是Debug。这里找了一个[Debug](https://www.cnblogs.com/tp25959/p/10596632.html)博客，可以看看。

具体的语法这里就不写了，这里就举一个常用的控制语句：`foreach`。简单来说就是在`for`循环的基础上自动迭代。

```c#
// item 是迭代器
foreach(var item in collection[])
```

## 特殊函数

前面说了，初始化创建c#脚本会植入两个函数，`Update`和`start`。其实还有几个函数和这两个函数相似。

- `awake` 可以在 `Awake()` 方法中进行变量初始化、获取其他组件的引用或执行一些对象启用后立即需要进行的操作。因为它在 `Start()` 之前调用，所以通常用于准备工作，但不涉及游戏逻辑的操作。

- `FixedUpdate()` 与 `Update()` 方法类似，但是它的执行频率是固定的，不受帧率的影响。在物理引擎中，`FixedUpdate()` 通常用于处理物理相关的逻辑，比如移动刚体、施加力或应用物理效果等操作。

## [矢量数学](https://blog.csdn.net/LLLLL__/article/details/105291241)

矢量在Unity很常使用，物体常常需要利用矢量组件确定物体的位置。

## 调用组件

有些场景中，需要启用组件和禁用组件。这个时候就可以调用组件的属性`enable`

```c#
public class EnableComponents : MonoBehaviour
{
    private Light myLight;
    
    
    void Start ()
    {
        myLight = GetComponent<Light>();
    }
    
    
    void Update ()
    {
        if(Input.GetKeyUp(KeyCode.Space))
        {
            myLight.enabled = !myLight.enabled;
        }
    }
}
```

当然在`Unity`中，我们可以通过取消物体激活的方式，让物体"消失"

![image-20240218154636903](image-20240218154636903.png)

代码可以这样写，需要注意的是，取消激活意味这当前组件的子组件也一并被取消激活了。

重新激活的代码就是把其中的`false`改为`true`

```c#
using UnityEngine;
using System.Collections;

public class ActiveObjects : MonoBehaviour
{
    void Start ()
    {
        gameObject.SetActive(false);
    }
}
```

## 位置旋转

控制物体移动位置在unity中使用的是`Transform`组件

![image-20240218160129020](image-20240218160129020.png)

- `position` 表示的是三维坐标，三维坐标系
- `Rotation` 表示的是旋转方位，以中心为基准的方向旋转
- `Scale` 控制各个方向的大小

在脚本中，可以对组件的属性进行修改，这里利用这个组件来实现物体移动和旋转

这里设置了两个变量，移动速度和旋转速度

```c#
using UnityEngine;
using System.Collections;

public class TransformFunctions : MonoBehaviour
{
    public float moveSpeed = 10f;
    public float turnSpeed = 50f;
    
    
    void Update ()
    {
        //移动
        if(Input.GetKey(KeyCode.UpArrow))
            transform.Translate(Vector3.forward * moveSpeed * Time.deltaTime);
        
        if(Input.GetKey(KeyCode.DownArrow))
            transform.Translate(-Vector3.forward * moveSpeed * Time.deltaTime);
        // 旋转
        if(Input.GetKey(KeyCode.LeftArrow))
            transform.Rotate(Vector3.up, -turnSpeed * Time.deltaTime);
        
        if(Input.GetKey(KeyCode.RightArrow))
            transform.Rotate(Vector3.up, turnSpeed * Time.deltaTime);
    }
}

```

> - `transform.Translate(Vector3.forward * moveSpeed * Time.deltaTime)` 将使物体沿着自身的前方向（Z 轴正方向）移动一定的距离。
> - `Rotate(Vector3 axis, float angle)`: 将物体绕指定的轴进行旋转。

## [Lookat](https://zhuanlan.zhihu.com/p/85384647)

> 这个暂时没用过

`Transform.LookAt()` 方法用于使一个物体朝向另一个目标位置或物体。它会使调用该方法的物体的正方向（通常是物体的前方）朝向目标位置，从而实现物体的旋转。

具体来说，`LookAt()` 方法有两种重载形式：

1. **`LookAt(Transform target)`**：
   - 这个重载版本接受一个 Transform 类型的参数 `target`，表示要朝向的目标对象。
   - 调用这个方法后，当前物体的正方向将指向目标对象的位置。
2. **`LookAt(Vector3 target)`**：
   - 这个重载版本接受一个 Vector3 类型的参数 `target`，表示要朝向的目标位置。
   - 调用这个方法后，当前物体的正方向将指向目标位置。

## [线性插值](https://zhuanlan.zhihu.com/p/114898567)

> 没用过

## [Destroy](https://www.cnblogs.com/Haha1999/p/13277830.html)

删除物体

## GetAxis

> 用于控制物体移动，没用过

`Input.GetAxis()` 方法用于获取指定输入轴的值。它的作用是检测输入设备（如键盘、手柄、触摸屏）上的轴向输入，并返回该轴向输入的值。

`GetAxis()` 方法常用于检测输入设备的移动、旋转、缩放等操作，通常与游戏对象的移动、旋转等行为相关联。它可以返回一个介于 -1 和 1 之间的浮点数值，表示输入轴的相对强度或位置。具体来说：

- 当输入设备处于中立位置时，`GetAxis()` 返回值为 0。
- 当输入设备向正方向移动时，返回值逐渐增加，最终接近于 1。
- 当输入设备向负方向移动时，返回值逐渐减小，最终接近于 -1。

例如，如果您想检测键盘上的水平移动输入，您可以使用 `Input.GetAxis("Horizontal")`。这将返回一个值，表示左右箭头键的相对按压情况，如果按下左箭头键，返回值为 -1；如果按下右箭头键，返回值为 1；如果没有按下任何键或同时按下左右箭头键，返回值为 0。

## [OnMouseDown](https://juejin.cn/post/7105744886464249870)

> 鼠标点击

[**MouseClick**]()

## GetComponent

## DeltaTime

https://blog.csdn.net/ChinarCSDN/article/details/82914420

## Invoke

https://www.cnblogs.com/WhiteTaken/p/6381984.html

## 属性

一句话理解，在不改变类中变量的封装性的情况下访问类的私有变量。属性样例

在vs中，可以通过输入prop+tab键快速创建一个属性

```c#
using UnityEngine;
using System.Collections;

public class Player
{
    //成员变量可以称为
    //字段。
    private int experience;

    //Experience 是一个基本属性
    public int Experience
    {
        get
        {
            //其他一些代码
            return experience;
        }
        set
        {
            //其他一些代码
            experience = value;
        }
    }

    //Level 是一个将经验值自动转换为
    //玩家等级的属性
    public int Level
    {
        get
        {
            return experience / 1000;
        }
        set
        {
            experience = value * 1000;
        }
    }

    //这是一个自动实现的属性的
    //示例
    public int Health{ get; set;}
}

```

## 静态动态

> 静态变量通常在类的内部。 类创建的实例，不同实例的变量值不同就叫做动态变量

- **静态变量**（有时也称为类变量）是在类级别上定义的变量，它们被类的所有实例共享，可以通过类名直接访问。这意味着静态变量在整个类中保持相同的值。
- **动态变量**（有时也称为实例变量）是在类的实例化过程中创建的变量，每个实例都有自己的一份。这意味着即使是同一个类创建的多个实例，它们的动态变量可以是不同的值，每个实例可以在其自己的作用域内管理和操作其动态变量。

```c#

```

## 方法重载

在同一个类中，可以定义多个方法，它们具有相同的名称但具有不同的参数列表。在编译时或运行时，根据调用方法时提供的参数数量或类型来确定使用哪个方法。

与其概念类似的是方法重写，方法重写是在子类中重新构造父类的方法。

```c#
using UnityEngine;
using System.Collections;

public class SomeClass
{
    //第一个 Add 方法的签名为
    //“Add(int, int)”。该签名必须具有唯一性。
    public int Add(int num1, int num2)
    {
        return num1 + num2;
    }

    //第二个 Add 方法的签名为
    //“Add(string, string)”。同样，该签名必须具有唯一性。
    public string Add(string str1, string str2)
    {
        return str1 + str2;
    }
}
```

## 泛型

泛型是一种编程语言特性，允许在编写代码时使用参数化类型。通过泛型，可以编写可以处理各种数据类型的代码，而不需要为每种类型都编写单独的代码。泛型提供了代码重用和类型安全性，并且在编译时会进行类型检查。

```c#
using UnityEngine;
using System.Collections;

//这是一个通用类。注意通用类型“T”。
//“T”将被替换为实际类型，同样，
//该类中使用的“T”类型实例也将被替换。
public class GenericClass <T>
{
    T item;

    public void UpdateItem(T newItem)
    {
        item = newItem;
    }
}
```

## 继承

派生类，会获得基类的方法。不过需要

```c#
using UnityEngine;
using System.Collections;

//这是基类，
//也称为父类。
public class Fruit 
{
    public string color;

    //这是 Fruit 类的第一个构造函数，
    //不会被任何派生类继承。
    public Fruit()
    {
        color = "orange";
        Debug.Log("1st Fruit Constructor Called");
    }

    //这是 Fruit 类的第二个构造函数，
    //不会被任何派生类继承。
    public Fruit(string newColor)
    {
        color = newColor;
        Debug.Log("2nd Fruit Constructor Called");
    }

    public void Chop()
    {
        Debug.Log("The " + color + " fruit has been chopped.");        
    }

    public void SayHello()
    {
        Debug.Log("Hello, I am a fruit.");
    }
}
```

```c#
using UnityEngine;
using System.Collections;

//这是派生类，
//也称为子类。
public class Apple : Fruit 
{
    //这是 Apple 类的第一个构造函数。
    //它立即调用父构造函数，甚至
    //在它运行之前调用。
    public Apple()
    {
        //注意 Apple 如何访问公共变量 color，
        //该变量是父 Fruit 类的一部分。
        color = "red";
        Debug.Log("1st Apple Constructor Called");
    }

    //这是 Apple 类的第二个构造函数。
    //它使用“base”关键字指定
    //要调用哪个父构造函数。
    public Apple(string newColor) : base(newColor)
    {
        //请注意，该构造函数不会设置 color，
        //因为基构造函数会设置作为参数
        //传递的 color。
        Debug.Log("2nd Apple Constructor Called");
    }
}
```

## 多态

父类为抽象类，派生类重写父类的抽象函数。称之为多态。

例如，动物类是父类，那么父类中构造了一个`叫`的抽象函数。那么子类就重写这个函数，例如狗是汪汪叫，猫是喵喵叫。

- 父类 `动物` 是一个抽象类，它定义了一个抽象函数 `叫`。
- 派生类 `狗` 和 `猫` 分别继承自 `动物` 类，并且重写了 `叫` 这个抽象函数。
- 在 `狗` 类中，重写的 `叫` 函数输出为 "汪汪"；在 `猫` 类中，重写的 `叫` 函数输出为 "喵喵"。

这样，当我们创建 `狗` 对象时，调用 `叫` 函数时会执行 `狗` 类中的具体实现，输出 "汪汪"；当创建 `猫` 对象时，调用 `叫` 函数会执行 `猫` 类中的具体实现，输出 "喵喵"。这就是多态的体现，不同的对象调用相同的方法但表现出不同的行为。

```c#
using UnityEngine;
using System.Collections;

public class Fruit 
{
    public Fruit()
    {
        Debug.Log("1st Fruit Constructor Called");
    }

    public void Chop()
    {
        Debug.Log("The fruit has been chopped.");        
    }

    public void SayHello()
    {
        Debug.Log("Hello, I am a fruit.");
    }
}
```

```c#
using UnityEngine;
using System.Collections;

public class Apple : Fruit 
{
    public Apple()
    {
        Debug.Log("1st Apple Constructor Called");
    }

    //Apple 有自己的 Chop() 和 SayHello() 版本。
    //运行脚本时，请注意何时调用
    //Fruit 版本的这些方法以及何时调用
    //Apple 版本的这些方法。
    //此示例使用“new”关键字禁止
    //来自 Unity 的警告，同时不覆盖
    //Apple 类中的方法。
    public new void Chop()
    {
        Debug.Log("The apple has been chopped.");        
    }

    public new void SayHello()
    {
        Debug.Log("Hello, I am an apple.");
    }
}
```

## 成员隐藏

成员隐藏（member hiding）是指在继承关系中，子类中的成员（如变量或方法）覆盖了父类中同名的成员，导致父类中的成员被隐藏起来而无法直接访问。

在面向对象的编程语言中，子类可以重新定义（覆盖）父类中的成员。如果子类中定义了与父类相同名称的成员，那么子类中的成员会隐藏（遮蔽）父类中的同名成员。这意味着，通过子类的实例访问该成员时，将会调用子类中的版本，而不是父类中的版本。

成员隐藏通常应用于方法和变量。当子类中重新定义了父类中的方法时，子类的实例调用该方法将执行子类中的版本。类似地，如果子类中定义了与父类中相同名称的变量，那么子类的实例访问该变量时将得到子类中的值。

需要注意的是，成员隐藏与多态是不同的概念。在多态中，子类中重写的方法会替代父类中的方法，但实际上仍然是同一个方法，只是行为可能有所不同。而成员隐藏则是完全替代了父类中的成员，隐藏了父类的成员而不是简单地覆盖。

## 覆盖

覆盖（override）是指子类重新定义了父类中已经存在的方法，以提供特定于子类需求的新实现。在继承关系中，当子类重写了父类的方法时，子类中的方法将覆盖（取代）父类中相同名称的方法。

覆盖发生的条件通常包括：

1. 子类必须继承自父类。
2. 子类中的方法签名（名称和参数列表）必须与父类中被覆盖的方法相同。
3. 子类中的方法必须具有与父类中被覆盖方法相同的访问修饰符或更加宽松的权限修饰符。

通过覆盖，子类可以为其自己的实例提供特定的实现，而不必使用父类的默认实现。这使得在子类中可以根据需要修改或扩展父类的行为，增加代码的灵活性和可维护性。

## [接口](https://www.cnblogs.com/Philip-Tell-Truth/p/6196156.html)

接口不是类，更像是一种协议。例如，充电协议，你这个充电器能充多少W，就是由这个接口定义的。接口一般在类的外部声明，一般一个脚本一个接口。接口一般实现类所需要的功能。

## [拓展方法](https://www.cnblogs.com/lxblog/archive/2013/04/25/3041826.html)

没用过

## [列表字典](https://www.donet5.com/Doc/27/2500)

没用过

## [协程](https://zhuanlan.zhihu.com/p/279383752)

很少用

## [四元数](https://cloud.tencent.com/developer/article/1546639)

没用过

## 委托

委托就是方法参数。类似于函数指针

## 事件

高级的委托
