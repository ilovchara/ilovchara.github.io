---
title: Fox
date: 2024-01-18 11:02:38
categories: unity
typora-root-url: ./Fox
---

# [Ruby's Adventure](https://learn.unity.com/project/ruby-s-adventure-2d-chu-xue-zhe?uv=2020.3)

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
            //拾取之后删除
            Destroy(gameObject);
        }
    }
}
```

当然，一直设置类函数为**public**是不太安全的，如果我们需要访问私有变量，这个时候就需要用到属性了。

在 **RubyController** 脚本中定义一个属性：

```c#
//get 关键字来获取第二个代码块中的任何内容。 - 用途是访问私有变量
public int health { get { return currentHealth; } }
```

这里的`currentHealth`是一个私有变量(感觉可以理解为换个名字)

下面是文件代码

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using static UnityEditor.Searcher.SearcherWindow.Alignment;

public class RubyController : MonoBehaviour
{
    public float speed = 3.0f;

    //生命值
    public int maxHealth = 5;

    public int health { get { return currentHealth; } }
    int currentHealth;


    Rigidbody2D rigidbody2d;
    // 横 - 纵
    float horizontal;
    float vertical;
    
    //速度


    // 只执行一次
    void Start()
    {
        //QualitySettings.vSyncCount = 0; //垂直同步关闭
        //Application.targetFrameRate = 10; // 限制刷新率 - 这里目的是限制移动
        rigidbody2d = GetComponent<Rigidbody2D>();
        //初始化
        currentHealth = maxHealth;
    }

    // 按帧执行
    void Update()
    {
        horizontal = Input.GetAxis("Horizontal");
        vertical = Input.GetAxis("Vertical");
    }

    private void FixedUpdate()
    {
        Vector2 position = transform.position;
        position.x = position.x + speed * horizontal * Time.deltaTime;
        position.y = position.y + speed * vertical * Time.deltaTime;

        rigidbody2d.MovePosition(position);
    }
    // 改变血量
    public void ChangeHealth(int amount)
    {
        //使用 Mathf.Clamp() 函数确保健康值在指定范围内（0 到 maxHealth 之间）
        currentHealth = Mathf.Clamp(currentHealth + amount, 0, maxHealth);
        Debug.Log(currentHealth + "/" + maxHealth);
    }

}
```

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
            if (controller.health < controller.maxHealth)
            {
                controller.ChangeHealth(1);
                Destroy(gameObject);
            }
        }
    }
}
```

## 伤害扣取生命值

布置陷阱精灵，配置触发器。

![image-20240122131846228](image-20240122131846228.png)

在陷阱精灵处，添加一个脚本文件，控制陷阱伤害人物。这里和获取生命值差不多，只是换了一个函数**OnTriggerStay2D**

这里的函数是按照帧来触发的

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class DamageZone : MonoBehaviour
{
    // 降低生命值 - 用触发器 - 这里Stay是按照帧触发
    void OnTriggerStay2D(Collider2D other)
    {
        RubyController controller = other.GetComponent<RubyController>();

        if (controller != null)
        {
            controller.ChangeHealth(-1);
        }
    }
}
```

但是，在陷阱范围内静止就不会触发伤害了，这是因为优化资源，**物理系统**在**刚体**停止移动时会停止计算刚体的碰撞。这里我们让刚体永不休眠就行：

![image-20240122134338963](image-20240122134338963.png)

可以看到不断刷新生命值了。这里dps有点高了

![image-20240122135136478](image-20240122135136478.png)

我们改为按照一定时间！过后才能再受到伤害

```c#
public class RubyController : MonoBehaviour
{
    public float speed = 3.0f;
    
    public int maxHealth = 5;
    public float timeInvincible = 2.0f;
    
    public int health { get { return currentHealth; }}
    int currentHealth;
    // 是否处于无敌状态
    bool isInvincible;
    // 恢复到可受伤状态之前剩下的无敌状态时间
    float invincibleTimer;
    
    Rigidbody2D rigidbody2d;
    float horizontal;
    float vertical;
    
    // 在第一次帧更新之前调用 Start
    void Start()
    {
        rigidbody2d = GetComponent<Rigidbody2D>();
        currentHealth = maxHealth;
    }

    // 每帧调用一次 Update
    void Update()
    {
        horizontal = Input.GetAxis("Horizontal");
        vertical = Input.GetAxis("Vertical");
        
        if (isInvincible)
        {
            // 通过这个时间间隔控制受伤开关
            invincibleTimer -= Time.deltaTime;
            if (invincibleTimer < 0)
                isInvincible = false;
        }
    }
    
    void FixedUpdate()
    {
        Vector2 position = rigidbody2d.position;
        position.x = position.x + speed * horizontal * Time.deltaTime;
        position.y = position.y + speed * vertical * Time.deltaTime;

        rigidbody2d.MovePosition(position);
    }

    public void ChangeHealth(int amount)
    {
        if (amount < 0)
        {
            if (isInvincible)
                return;
            
            isInvincible = true;
            // 更新时间
            invincibleTimer = timeInvincible;
        }
        
        currentHealth = Mathf.Clamp(currentHealth + amount, 0, maxHealth);
        Debug.Log(currentHealth + "/" + maxHealth);
    }
}
```

这里就可以2s伤害一次了

![image-20240122143524317](image-20240122143524317.png)

然后配置另一种减少生命的形式，添加一些敌人

拉入敌人的图片制作成精灵

![image-20240122145944625](image-20240122145944625.png)

创建控制敌人的脚本，写入让敌人周期不同方向移动

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class EnemyController : MonoBehaviour
{
    public float speed;
    public bool vertical;
    public float changeTime = 3.0f;
	// 物体的刚体 - 有我们需要的变量
    Rigidbody2D rigidbody2D;
    float timer;
    int direction = 1;

    // 在第一次帧更新之前调用 Start
    void Start()
    {
        rigidbody2D = GetComponent<Rigidbody2D>();
        timer = changeTime;
    }

    void Update()
    {
        //时间周期
        timer -= Time.deltaTime;

        if (timer < 0)
        {
            //方向
            direction = -direction;
            timer = changeTime;
        }
    }

    void FixedUpdate()
    {
        Vector2 position = rigidbody2D.position;

        if (vertical)
        {
            position.y = position.y + Time.deltaTime * speed * direction; ;
        }
        else
        {
            position.x = position.x + Time.deltaTime * speed * direction; ;
        }

        rigidbody2D.MovePosition(position);
    }
}
```

> 这里补充一下
>
> - `timer -= Time.deltaTime;` `Time` 是一个静态类，用于获取有关时间的信息，它返回上一帧和当前帧之间的时间间隔。
> - `MovePosition` 字面意思，移动到指定的position
> - `direction` 表示的是方向

在之前的陷阱精灵布置，布置大范围的陷阱的时候拉伸精灵会变得很难看。这里就需要用到**精灵渲染器 (Sprite Renderer)** 的功能

- 首先，确保**游戏对象**的缩放在 **Transform 组件**中设置为 **1**,**1**,**1**。
- 然后在 **Sprite Renderer** 组件中将 **Draw Mode** 设置为 **Tiled**，并将 **Tile Mode** 更改为 **Adaptive**。

这里的Tiled表示平铺，Adaptive表示自适应大小

![image-20240123121857814](image-20240123121857814.png)

如果平铺会显示警告，这是因为原素材没有采用**Full Rect:**

![image-20240123122215213](image-20240123122215213.png)

在Unity游戏引擎中，Mesh Type（网格类型）通常是指用于渲染3D模型的网格（Mesh）的类型。在Renderer组件或Mesh Filter组件中，你可能会找到Mesh Type这个属性。以下是常见的Mesh Type选项：

1. **Full Rect:**
   - 这个选项表示网格将占据整个矩形区域。通常用于渲染整个模型。
2. **Tight:**
   - Tight表示网格将紧密包围在模型的非透明区域周围。这可以减少不透明区域之外的渲染开销，特别是对于复杂形状的模型。
3. **Bounding Box:**
   - 这个选项使用包围盒（Bounding Box）来定义模型的边界。包围盒是一个最小的立方体，完全包围模型。这种方式可以更快速地进行渲染，但可能会导致在一些情况下出现不准确的渲染。

## 添加动画

先添加敌人的，打开对应的预制体，添加`animator`

![image-20240123131920499](image-20240123131920499.png)

再创建一个Controller,拖入到animator中，这里的控制器就创建完成了

![image-20240124134530016](image-20240124134530016.png)

我们需要做四个方向的运动，这里需要用到动画树的效果

现在先制作向左运动的动画，打开window中的animation，拉入对应的动画即可

![image-20240124134933727](image-20240124134933727.png)

这里需要注意的是，Samples可能被隐藏了，可以在右边的三点找到

![image-20240124135021647](image-20240124135021647.png)

直接将Samples的值调为4，表示每秒刷新四张图片。可以将这个理解为帧率

![image-20240124135327448](image-20240124135327448.png)

制作四个动画的时候发现一个问题，我没找到向右运动的动画，这里就将向左运动的y轴翻转180°。

![image-20240124141616203](image-20240124141616203.png)

然后发现了更简单的方法：这里有个按钮可以直接反转x轴？"Flip X" 表示在水平方向上进行翻转或镜像。

![image-20240124142102487](image-20240124142102487.png)

接下来就是布置动画状态机了，打开我们之前创建的状态机

![image-20240124142605326](image-20240124142605326.png)

这里学习使用混合树来构造播放效果，将上面的动画删除，右键创建Blend Tree

![image-20240124155013305](./image-20240124155013305.png)

打开Blend Tree，选用2d

![image-20240124155117389](image-20240124155117389.png)

选用之前记得创建两个，创建两个维度

![image-20240124155159438](image-20240124155159438.png)

在右侧插入动画，中间红点靠近会播放动画

![image-20240124160112558](image-20240124160112558.png)

然后我们需要在脚本中调用这些动画，这里只注释有关动画的部分

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class EnemyController : MonoBehaviour
{
    public float speed;
    public bool vertical;
    public float changeTime = 3.0f;

    Rigidbody2D rigidbody2D;
    float timer;
    int direction = 1;
	// 声明一个动画
    Animator animator;


    // 在第一次帧更新之前调用 Start
    void Start()
    {
        rigidbody2D = GetComponent<Rigidbody2D>();
        timer = changeTime;
        animator = GetComponent<Animator>();
    }

    void Update()
    {
        timer -= Time.deltaTime;

        if (timer < 0)
        {
            direction = -direction;
            timer = changeTime;
        }
    }

    void FixedUpdate()
    {
        Vector2 position = rigidbody2D.position;
        // 这里只在竖直方向运动
        if (vertical)
        {
            // 调用设置好的动画名字
            animator.SetFloat("Move X", 0);
            animator.SetFloat("Move Y", direction);
            position.y = position.y + Time.deltaTime * speed * direction; ;
        }
        else
        {
            // 调用设置好的动画名字
            animator.SetFloat("Move X", direction);
            animator.SetFloat("Move Y", 0);
            position.x = position.x + Time.deltaTime * speed * direction; ;
        }

        rigidbody2D.MovePosition(position);
    }

    void OnCollisionEnter2D(Collision2D other)
    {
        // 获取玩家的碰撞体
        RubyController player = other.gameObject.GetComponent<RubyController>();

        if (player != null)
        {
            player.ChangeHealth(-1);
        }
    }


}
```

这里的X,Y决定了Position,可以看作显示红点的方位

![image-20240124181619146](image-20240124181619146.png)

![image-20240124181740210](image-20240124181740210.png)

同样的对于Ruby我们也做一个动画，不过是用传统的动画机做的。打开角色的预制体，然后插入状态机

![image-20240125155537908](image-20240125155537908.png)

这里导入写好的状态过度，左侧可以理解为编译器可以检测到的变量

![image-20240125155832472](./image-20240125155832472.png)

然后对应的脚本

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using static UnityEditor.Searcher.SearcherWindow.Alignment;

public class RubyController : MonoBehaviour
{
    public float speed = 3.0f;

    //生命值
    public int maxHealth = 5;
    //造成伤害时间
    public float timeInvincible = 2.0f;

    public int health { get { return currentHealth; } }
    int currentHealth;

    bool isInvincible;
    float invincibleTimer;

    Rigidbody2D rigidbody2d;
    // 横 - 纵
    float horizontal;
    float vertical;

    //动画
    Animator animator;
    Vector2 lookDirection = new Vector2(1, 0);



    // 只执行一次
    void Start()
    {
        rigidbody2d = GetComponent<Rigidbody2D>();
        currentHealth = maxHealth;
        
        //获取动画
        animator = GetComponent<Animator>();
    }

    // 按帧执行
    void Update()
    {
        horizontal = Input.GetAxis("Horizontal");
        vertical = Input.GetAxis("Vertical");
	    //移动位置
        Vector2 move = new Vector2(horizontal, vertical);

        if (!Mathf.Approximately(move.x, 0.0f) || !Mathf.Approximately(move.y, 0.0f))
        {
            lookDirection.Set(move.x, move.y);
            lookDirection.Normalize();
        }
	   // 根据对应的变量 播放不同的动画
        animator.SetFloat("Look X", lookDirection.x);
        animator.SetFloat("Look Y", lookDirection.y);
        animator.SetFloat("Speed", move.magnitude);

        if (isInvincible)
        {
            invincibleTimer -= Time.deltaTime;
            if(invincibleTimer < 0 )
            {
                isInvincible = false;
            }
        }

    }

    private void FixedUpdate()
    {
        Vector2 position = transform.position;
        position.x = position.x + speed * horizontal * Time.deltaTime;
        position.y = position.y + speed * vertical * Time.deltaTime;

        rigidbody2d.MovePosition(position);
    }
    // 改变血量
    public void ChangeHealth(int amount)
    {
        if (amount < 0)
        {
            if (isInvincible) { return; }

            isInvincible = true;
            invincibleTimer = timeInvincible;
        }


        //使用 Mathf.Clamp() 函数确保健康值在指定范围内（0 到 maxHealth 之间）
        currentHealth = Mathf.Clamp(currentHealth + amount, 0, maxHealth);
        // 打印
        Debug.Log(currentHealth + "/" + maxHealth);
    }

}
```

## 攻击模式

拉入齿轮精灵作为主角发射的飞弹，在拉入之前设置PPU

![image-20240125162432217](image-20240125162432217.png)

"Pixels Per Unit"（PPU）是一个用于定义图像和实际世界尺寸之间关系的度量。它指定了在Unity中的一个单位（通常是一个米或一个Unity单位）对应于游戏中的多少个像素。可以控制游戏中的图像在屏幕上的大小，而无需直接改变图像文件的分辨率

为了对这个齿轮进行操作，这里创建一个脚本`Projectile`

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Projectile : MonoBehaviour
{
    Rigidbody2D rigidbody2d;

    void Start()
    {
        rigidbody2d = GetComponent<Rigidbody2D>();
    }

    public void Launch(Vector2 direction,float force)
    {
        // 调用物理函数 - 理解为推出去就行
        rigidbody2d.AddForce(direction * force);
    }

    void OnCollisionEnter2D(Collision2D other)
    {
        //我们还增加了调试日志来了解飞弹触碰到的对象
        Debug.Log("Projectile Collision with " + other.gameObject);
        // 碰撞完成就销毁
        Destroy(gameObject);
    }

}
```

在Ruby脚本处添加有关于子弹的函数

```c#
// 预制件 - 获取
public GameObject projectilePrefab;

 // 发射子弹函数
 void Launch()
 {
     //配置生成的预制体
     GameObject projectileObject = Instantiate(projectilePrefab, rigidbody2d.position + Vector2.up * 0.5f, Quaternion.identity);

     Projectile projectile = projectileObject.GetComponent<Projectile>();
     projectile.Launch(lookDirection, 300);

     animator.SetTrigger("Launch");
 }
```

然后记得在挂载子弹

![image-20240125190003442](image-20240125190003442.png)

> 这里补充一些知识点：用口水话说`Instantiate`函数,就是在运行的时候通过你设定的机制生成指定的预制体。
>
> 当在Unity中需要在运行时创建新的游戏对象实例时，可以使用 `Instantiate` 函数。该函数允许你根据给定的预制件（Prefab）在场景中动态生成新的游戏对象。以下是 `Instantiate` 函数的一般用法和作用：
>
> ```c#
> public static Object Instantiate(Object original);
> public static Object Instantiate(Object original, Vector3 position, Quaternion rotation);
> public static Object Instantiate(Object original, Transform parent);
> public static Object Instantiate(Object original, Vector3 position, Quaternion rotation, Transform parent);
> ```
>
> - `original`：要实例化的对象或预制件。
> - `position`：新实例的初始位置。
> - `rotation`：新实例的初始旋转。
> - `parent`：新实例的父级 `Transform`。如果指定了父级，新实例将成为父级 `Transform` 的子对象。
>
> 使用 `Instantiate` 的典型场景包括：
>
> 1. **生成游戏对象：** 在游戏运行时，你可能需要根据某些条件或事件创建新的游戏对象。通过 `Instantiate`，你可以在代码中动态生成这些对象。
>
>    ```c#
>    public GameObject prefab; // 预制件
>    
>    void Start()
>    {
>        // 在场景中生成预制件的实例
>        GameObject newObject = Instantiate(prefab, new Vector3(0, 0, 0), Quaternion.identity);
>    }
>    ```
>
> 2. **子弹发射：** 在射击游戏中，当玩家发射子弹时，你可能希望在发射点生成一个子弹实例。
>
>    ```c#
>    public GameObject bulletPrefab;
>    
>    void Update()
>    {
>        if (Input.GetButtonDown("Fire1"))
>        {
>            // 在当前位置生成子弹实例
>            Instantiate(bulletPrefab, transform.position, transform.rotation);
>        }
>    }
>    ```
>
> 3. **敌人生成：** 在敌人波次开始时，你可以使用 `Instantiate` 在指定位置生成一组敌人实例。
>
>    ```c#
>    public GameObject enemyPrefab;
>    
>    void StartWave()
>    {
>        for (int i = 0; i < 5; i++)
>        {
>            // 在指定位置生成敌人实例
>            Instantiate(enemyPrefab, new Vector3(i * 2, 0, 0), Quaternion.identity);
>        }
>    }
>    ```

但是创建的子弹预制体会和我们角色碰撞从而被摧毁掉，这里就需要配置图层。添加两个图层。

![image-20240125192539255](image-20240125192539255.png)

但是设置完成图层之后还会碰撞，这里就需要查看碰撞逻辑了，打开**Edit** **>** **Project Settings** **>** **Physics 2D** 

![image-20240125193005838](image-20240125193005838.png)

把`Character`和`Projectile`对应的位置取消对勾，这样就不会相互碰撞了

![image-20240125193237983](image-20240125193237983.png)

然后就可以发射了

![image-20240125212233200](image-20240125212233200.png)

> 这里补充一下我犯的错误
>
> 我把Ruby发射的子弹，拉成了我创建在地图的预制体了，当预制体被摧毁的时候，Ruby就发射不了子弹了。所以说一定要拉文件夹的。

接下来需要做机器人的受击，这里射击机器人 = 修复机器人

原理是，创建一个bool变量来控制机器人的Update

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class EnemyController : MonoBehaviour
{
    public float speed;
    public bool vertical;
    public float changeTime = 3.0f;

    Rigidbody2D rigidbody2D;
    float timer;
    int direction = 1;

    Animator animator;
	// 判断运动
    bool broken = true;

    // 在第一次帧更新之前调用 Start
    void Start()
    {

        rigidbody2D = GetComponent<Rigidbody2D>();
        timer = changeTime;
        animator = GetComponent<Animator>();
    }

    void Update()
    {
        timer -= Time.deltaTime;

        if (timer < 0)
        {
            direction = -direction;
            timer = changeTime;
        }
		// 修好停止
        if (!broken)
        {
            return;
        }
    }

    void FixedUpdate()
    {
        // 修好停止
        if (!broken)
        {
            return;
        }

        Vector2 position = rigidbody2D.position;
        // 这里只在竖直方向运动
        if (vertical)
        {
            animator.SetFloat("Move X", 0);
            animator.SetFloat("Move Y", direction);
            position.y = position.y + Time.deltaTime * speed * direction; ;
        }
        else
        {
            animator.SetFloat("Move X", direction);
            animator.SetFloat("Move Y", 0);
            position.x = position.x + Time.deltaTime * speed * direction; ;
        }

        rigidbody2D.MovePosition(position);


    }

    void OnCollisionEnter2D(Collision2D other)
    {
        // 获取玩家的碰撞体
        RubyController player = other.gameObject.GetComponent<RubyController>();

        if (player != null)
        {
            player.ChangeHealth(-1);
        }
    }
	// 控制修理成功
    public void Fix()
    {
        broken = false;
        rigidbody2D.simulated = false;
        // 控制对应的控制器
        animator.SetTrigger("Fixed");
    }

}
```

这里创建了一个变量用于控制动画播放，这里声明了一个Triger变量

![image-20240125231634469](image-20240125231634469.png)

在子弹脚本部分，加上一下内容。目的是子弹击中之后播放机器人修理动画

```c#
    void OnCollisionEnter2D(Collision2D other)
    {
        EnemyController e = other.collider.GetComponent<EnemyController>();
        if (e != null)
        {
            e.Fix();
        }

        Destroy(gameObject);
    }
```

这样就完成了。

![image-20240125232108572](image-20240125232108572.png)
