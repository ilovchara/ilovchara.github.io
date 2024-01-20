---
title: Fox
date: 2024-01-18 11:02:38
categories: unity
typora-root-url: ./Fox
---

# Ruby's Adventure

> 前面创建角色啥的就不细谈了，这里直接快进到画地图

## 使用Tile制作瓦片地图

创建一个Tilemap，作用是显示格子让我们铺设瓦片。

![image-20240118111149798](image-20240118111149798.png)

导入在官网下载Sprite。

![image-20240118202139676](image-20240118202139676.png)



简单铺一下地板，这里直接用涂刷就行。

![image-20240118202156517](image-20240118202156517.png)

只有一个瓦片太单调了，这里把包中的瓦片切割一下。

![image-20240118202722412](image-20240118202722412.png)

打包切割完成开始绘制地图：

![image-20240118202921780](image-20240118202921780.png)

这里需要注意的是，Tile Palette 这里的瓦片会被不小心覆盖掉，上面的几个按键的功能如下：

![img](https://connect-prd-cdn.unity.com/20190828/learn/images/f2ec2a3c-a53a-4d32-a3d9-7050d17d2e35_Tools.PNG)

- 1.选择，没啥好说的，选中的可以拖动
- 2.移动，移动对应的Tile
- 3.画笔，主要用的是这个，就用来绘制地图的
- 4.填充，可以填充多块地图
- 5.选择器，选择片段然后一起填充
- 6.橡皮擦
- 7.大块填充

下面就绘制一下地图：

![image-20240118212859780](image-20240118212859780.png)

## 添加碰撞物体

添加物体和添加人物的操作是一样的，只不过我们需要注意一些问题。例如，角色和物体的层次是怎么样的，有什么碰撞效果。

我们需要指示 **Unity** 根据游戏对象的 y 坐标来绘制**游戏对象**（请记住，**y** 是垂直轴，**x** 是水平轴）。

在教程中我们需要这样：将Sort Axis 改为 X:0 Y:1 Z:0

![image-20240118213645350](image-20240118213645350.png)

轴向量 x = 0, y = 1, z = 0 表示世界坐标系中的上方向（y轴正方向）。这意味着透明物体将根据它们在这个方向上的位置进行排序，确保在视图中更靠近上方的物体先绘制，而在下方的物体后绘制。

![image-20240118213928333](image-20240118213928333.png)

![image-20240118213937381](image-20240118213937381.png)

大概是和图这样的区别，可以简单理解为按照y轴的高低来决定渲染的物件的优先级。

打开角色的渲染器，将精灵的排序点选择为以精灵为中心。如下

![image-20240118214717552](image-20240118214717552.png)

同样的需要定义箱子精灵的渲染中心，目的是让其在底部渲染，这样角色会被箱体遮挡住

![image-20240118215119899](image-20240118215119899.png)

当然更改轴心的方式不止一种，可以在对应的编辑中修改，这里修改一下Ruby的轴心

![image-20240118215604893](image-20240118215604893.png)

中间小蓝点可以物理修改，右下角也有对应的修改托盘。修改轴心可以配合上面渲染顺序，轴心如果覆盖了其他的物品的轴心，角色就会被渲染出来。

当然，碰撞的时候需要碰撞效果，所以说需要给他们加上Rigidbody 2d 还有 C

- Rigidbody 2d 提供给挂载物体物理效果
- Collider 2d 提供给挂载物体碰撞效果

这里的碰撞可以理解为制作一个盒子包住我们的贴图。

![image-20240120203613525](image-20240120203613525.png)

![image-20240120203627661](image-20240120203627661.png)

记住，我们制作的2d可以说是没有重力的，所以说需要把重力设置为0

![image-20240120203745509](image-20240120203745509.png)

在对Hierarchy这里处理的时候，需要同步处理这些加载过的属性。

![image-20240120204013460](./image-20240120204013460.png)

点击overrides，然后选择所有就可以同步预制体了

![image-20240120204046316](./image-20240120204046316.png)

然后运行游戏，可以查看到碰撞效果

![image-20240120204533801](image-20240120204533801.png)

但是，角色会旋转和抖动，我们需要解决这个问题。在Rigdbody 2d 这里禁用这个z轴让它不在偏移。

![image-20240120204621501](./image-20240120204621501.png)

这里是官方文档的解释，大致意思是。帧演算物体碰撞弹回造成的抖动效果

![image-20240120204755261](./image-20240120204755261.png)

这里需要在脚本中，需要移动**刚体**本身而不是**游戏对象变换组件**，并让**物理系统**将**游戏对象**位置同步到**刚体**位置。

```c#
public class RubyController : MonoBehaviour
{
    Rigidbody2D rigidbody2d;
    float horizontal; 
    float vertical;
    
    // 在第一次帧更新之前调用 Start
    void Start()
    {
        rigidbody2d = GetComponent<Rigidbody2D>();
    }

    // 每帧调用一次 Update
    void Update()
    {
        horizontal = Input.GetAxis("Horizontal");
        vertical = Input.GetAxis("Vertical");
    }
	// 
    void FixedUpdate()
    {
        Vector2 position = rigidbody2d.position;
        position.x = position.x + 3.0f * horizontal * Time.deltaTime;
        position.y = position.y + 3.0f * vertical * Time.deltaTime;

        rigidbody2d.MovePosition(position);
    }
}
```

补充一些知识点：

- Transform 组件 可以理解为挂载目标的位置
- **FixedUpdate** 函数处理物理组件和对象，与updata不同不会持续运行。在每个物理更新步骤中执行一次，与帧率无关。

然后我们需要调整碰撞体的大小，来让游戏更加合理：

![image-20240120214837405](image-20240120214837405.png)

点击这个调整就行，同样的其他搭载了Collider 2d 的也需要调整

![image-20240120214853621](image-20240120214853621.png)

当然，在地图中我们也需要制作碰撞体阻止角色前进，这里我们在Tilemap中，把水这些位置加上Tilemap Collider 2d,让他和我们角色进行碰撞。这里的碰撞器的类型稍微不同。

![image-20240120215539584](image-20240120215539584.png)

选上之后，全部地图都会被绿色覆盖，绿色显示的是碰撞区域。将不希望执行碰撞的区块取消勾选：在制作完成的Tile中选择修改，修改Collider Type 为none

![image-20240120215739202](image-20240120215739202.png)

![image-20240120215654759](image-20240120215654759.png)

实现之后，就只有绿色部分是可以碰撞的了。

![image-20240120220030381](./image-20240120220030381.png)

当然，这样设置的碰撞体计算量很大，毕竟每一个块都会累计一次碰撞计算，在Tilemap 中加入一个新组件：**Composite Collider 2D** ，加入这个组件之后会自动加上**Rigidbody 2D** , 接下来：配置图片中的选项

![image-20240120221541650](image-20240120221541650.png)

设置为静态就是无物理特性，设置Composite将Tilemap合并。

## 为主角添加生命值

```c#
public class RubyController : MonoBehaviour
{
    public int maxHealth = 5;
    int currentHealth;
    
    Rigidbody2D rigidbody2d;
    float horizontal;
    float vertical;
    
    // 在第一次帧更新之前调用 Start
    void Start()
    {
        rigidbody2d = GetComponent<Rigidbody2D>();
		// 初始化最大生命值
        currentHealth = maxHealth;
    }

    // 每帧调用一次 Update
    void Update()
    {
        horizontal = Input.GetAxis("Horizontal");
        vertical = Input.GetAxis("Vertical");
    }
    
    void FixedUpdate()
    {
        Vector2 position = rigidbody2d.position;
        position.x = position.x + 3.0f* horizontal * Time.deltaTime;
        position.y = position.y + 3.0f * vertical * Time.deltaTime;

        rigidbody2d.MovePosition(position);
    }

    void ChangeHealth(int amount)
    {
        currentHealth = Mathf.Clamp(currentHealth + amount, 0, maxHealth);
        Debug.Log(currentHealth + "/" + maxHealth);
    }

}
```

这里变量的意思：

- maxHealth 最大生命值
- currentHealth 当前生命值

解释一下这个函数：

```c#
void ChangeHealth(int amount)
    {
    	//使用 Mathf.Clamp() 函数确保健康值在指定范围内（0 到 maxHealth 之间） - 官网说是钳制很形象
        currentHealth = Mathf.Clamp(currentHealth + amount, 0, maxHealth);
        Debug.Log(currentHealth + "/" + maxHealth);
    }
```

声明的公开变量可以在unity中显示

![image-20240120225851095](image-20240120225851095.png)

这里改一下移动的计算公式，将写死的3.0f改为我们设置的变量

```c#
    void FixedUpdate()
    {
        Vector2 position = rigidbody2d.position;
        position.x = position.x + speed * horizontal * Time.deltaTime;
        position.y = position.y + speed * vertical * Time.deltaTime;

        rigidbody2d.MovePosition(position);
    }
```

为了容易获取生命值或者失去生命值。这里需要加入unity中的触发器，拉入生命值物体，拾取会增加生命

![image-20240120233755851](image-20240120233755851.png)

创建一个脚本，用于控制角色收集血量，名字叫做**HealthCollectible**。

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class HealthCollectible : MonoBehaviour
{

    
    void OnTriggerEnter2D(Collider2D other)
    {
        //获取碰撞体上的 RubyController 组件
        RubyController controller = other.GetComponent<RubyController>();
        //有敌人 - 碰撞
        if (controller != null)
        {
            controller.ChangeHealth(1);
            Destroy(gameObject);
        }
    }
}
```

