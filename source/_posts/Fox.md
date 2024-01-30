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

## 摄影机

之前在b站学的小RPG用过这个摄像头组件，就是制作一个跟随角色的游戏界面

下载包资源可以按照这个[链接教程](https://zhuanlan.zhihu.com/p/516625841)，这里下载好了之后发现页顶处没有出现对应的下载完成的组件，原因是新版本组件在组件序列里了（也就是说，可以在里面找到下载的内容）

![image-20240127000137102](./image-20240127000137102.png)

给地图添加一个跟随摄像头

![image-20240127155035499](./image-20240127155035499.png)

创建完成之后会出现这个东西

![image-20240127222229081](image-20240127222229081.png)

为了适应屏幕，我们扩大了点地图

![image-20240127222246808](image-20240127222246808.png)

让摄像机跟着我们角色运动，实现效果如下

![image-20240127222357192](image-20240127222357192.png)

![image-20240127234929583](image-20240127234929583.png)

然后设置摄像机边界，创建一个空物体搭载下面的组件。这个组件可以理解为碰撞体，记得勾选is trigger

![image-20240127235615619](image-20240127235615619.png)

然后在摄像机处，添加一个组件 名字是Cinemachine Confiner 2D。如果按照官网的教程的话，是实现不了的（具体原因不清楚）

![image-20240127235718652](image-20240127235718652.png)

这样就行了

![image-20240127235827367](image-20240127235827367.png)

## 特效制作

使用粒子系统为游戏创建一些粒子，例如**损坏的机器人**的烟雾效果

先制作一个特效，右键 Effects > Particle System

![image-20240128144434812](image-20240128144434812.png)

然后制作特效的切割 

![image-20240128144608434](image-20240128144608434.png)

插入我们切割完成的特效，启用Texture Sheet Animation > 插入两个动画 > 删除Particle System Curver 的图曲线。这里还要设置Start Frame 的范围，设置为0-2即可

![image-20240128144901433](image-20240128144901433.png)

然后特效就有了。

![image-20240128145204790](image-20240128145204790.png)

下一步是更改创建这些烟雾精灵的方式，因为现在它们在方向上过于分散。在 **Inspector** 中打开 **Shape 部分**。**Scene 视图**将显示发射粒子的锥体。将 **Radius** 设置为 **0**，让粒子都从一个点发出。

![image-20240128145746916](image-20240128145746916.png)

在 **Particle System** 顶部的主要部分中，查找以下**三个设置**：

**1.Start Lifetime**：粒子的生命周期是指粒子在屏幕上被粒子系统销毁之前存在的时间。如果在 **Scene 视图**中**缩小**，所有粒子都差不多在同一位置消失。这是因为粒子的初始速度和生命周期都相同，因此最终在相同的距离处被销毁。

单击 **Start Lifetime** 右侧的小向下箭头，然后选择 **Random Between Two Constants**。输入 **1.5** 和 **3**。粒子的消失速度会更快，因为现在它们的生命周期更短了。而且粒子也会以更加自然的方式消失，因为粒子现在的生命周期各不相同。

**2.Start Size**：这是粒子创建后的大小。现在只设置了一个数字，因此所有粒子的大小都相同。和上面一样，选择 **Random Between Two Constants** 并分别设置为 **0.3** 和 **0.5**，这样来引入一些随机性。粒子现在变小了且大小不同，但移动速度仍然过快。

**3.Start Speed**：通过将这个属性设置为 **Random Between Two Constants**，可以降低粒子的初始移动速度并增加一些随机性。将两个常量分别设置为 **0.5** 和 **1**。

![image-20240128150807404](image-20240128150807404.png)

设置完成之后烟雾就合理了

![image-20240128151112779](image-20240128151112779.png)

但是消失的时候透明度不自然，这里调整一下颜色属性。上面的标表示透明度，下面表示颜色深度。将Alpha调整为0，让它在末尾完全透明，演示消失的效果。

![image-20240128185030322](image-20240128185030322.png)

然后就自然许多了。

![image-20240128185238914](image-20240128185238914.png)

现在我们脚本还不能识别这个烟雾，我们得生成一个物体变量可以让unity和c#交互

```c#
// 变量拖入    
public ParticleSystem smokeEffect;
```

不过为什么是这个类型，教程是这样说的：

如果公共成员是 **Component** 或 **Script 类型**（而不是 **GameObject**），则当你在 **Inspector** 中为这个成员分配**游戏对象**时，**Unity** 将存储**游戏对象**上的组件类型。

这样可以避免必须像以前一样在脚本中执行 **GetComponent**。此外还会阻止你将没有该组件类型的**游戏对象**分配给该设置。这也避免了用户不小心制造 bug。

接下来拖入我们的烟雾

![image-20240128185521846](image-20240128185521846.png)

脚本中的Fix()函数，在修理完成之后停止特效渲染

```c#
public void Fix()
{
    broken = false;
    rigidbody2D.simulated = false;
    animator.SetTrigger("Fixed");
    // 修理
    smokeEffect.Stop();
}
```

然后就可以了

![GIF1](GIF1-1706439722524-3.gif)

**Stop** 只会阻止**粒子系统**创建粒子，已经存在的粒子可以正常结束自己的生命周期。这比所有粒子突然消失要看起来自然得多。

## UI制作

制作一个显示Ruby血量的UI，这里要学到遮罩的效果

![image-20240129231607864](image-20240129231607864.png)

创建一个UI画布

![image-20240129231633477](image-20240129231633477.png)

画布会比地图大上不少，方便我们在上面加入素材

这里创建UI，作用是显示血量

![image-20240129232237596](image-20240129232237596.png)

这里UI可以插入图片，插入完成是这个样子的

![image-20240129232309910](image-20240129232309910.png)

接下来就是设计这个UI的样式了，由三个部分组成

![image-20240129233315205](image-20240129233315205.png)

需要学的是这个UI的锚点和遮罩

新建的Health的锚点设置在左上角

![image-20240129233410467](image-20240129233410467.png)

头像的锚点设置在图片四周

![image-20240129233448308](image-20240129233448308.png)

遮罩部分也是四周

![image-20240129233510930](image-20240129233510930.png)

什么是锚点呢？在选择图像后，观察 **Scene 视图**：

![img](https://connect-prd-cdn.unity.com/20190827/learn/images/af042431-a565-4001-b6e8-dd86f35242ce_pasted_image_0__11_.png)

屏幕中央的十字就是图像的锚点。这是计算对象位置时的起点（由绿色箭头显示，从图像的锚点到图像的轴心（即小蓝色圆圈））。

因此，在调整屏幕大小时，该位置将保持不变，并且图像不会随着**屏幕边框**移动。

接下来创建一个遮罩，**遮罩**是 **UI 系统**中的一种技术，利用这种技术可以将一张图像用作其他图像的“遮罩”。我们可以看成是第一张图像充当一个模板。

创建Health的子对象，叫做Mask

![image-20240129234718140](image-20240129234718140.png)

在UI处调整到合适的大小，同时添加一个组件Mask

![image-20240129234829592](image-20240129234829592.png)

Show Mask Graphic属性用于指定是否显示遮罩区域的图形，这里我们需要白色背景隐藏，所以说取消勾选

所以说在Mask中，我们创建一个子对象来让它成为被遮罩的物件

![image-20240129235524423](image-20240129235524423.png)

这样调整Mask，就可以调整这个血量了。

![GIF3](GIF3.gif)

接下来写一个脚本，将血量减少和遮罩联系起来。

```c#
public class UIHealthBar : MonoBehaviour
{
    // 单例变量 - 不用多声明对象
    public static UIHealthBar instance { get; private set; }
    // 获取遮罩
    public Image mask;
    float originalSize;

    void Awake()
    {
        instance = this;
    }

    void Start()
    {
        originalSize = mask.rectTransform.rect.width;
    }

    public void SetValue(float value)
    {				      
        mask.rectTransform.SetSizeWithCurrentAnchors(RectTransform.Axis.Horizontal, originalSize * value);
    }
}
```

然后再ruby血量变化的地方

```c#
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
    // SetValue 方法通过更新 mask 对应的 RectTransform 的水平尺寸
    UIHealthBar.instance.SetValue(currentHealth / (float)maxHealth);
}
```

也就是这个

```c#
    public void SetValue(float value)
    {	
        //这一行代码用于根据 value 更新血条的宽度。originalSize 可能是血条的初始宽度，RectTransform.Axis.Horizontal 表示更新的是水平方向的尺寸。   
        mask.rectTransform.SetSizeWithCurrentAnchors(RectTransform.Axis.Horizontal, originalSize * value);
    }
```

然后再一个物体上挂载这个脚本，就可以实现了。

![image-20240130002016727](image-20240130002016727.png)

![image-20240130002234191](image-20240130002234191.png)

## 制作NPC

将连续的图片拖入到Hierarchy中，就可以制作动画。不过帧数还是要调整一下，三张图片，就调整为3帧就好。

![image-20240130142945671](image-20240130142945671.png)

这里调整，1.画布的宽度和长度。2.界面的大小 3.图层等级

![image-20240130143116131](image-20240130143116131.png)

画布右键，建立

![image-20240130150511168](image-20240130150511168.png)

然后建立画布子对象，用于背景。在Rect Transform 按Alt键点击右下角平铺

![image-20240130150719609](./image-20240130150719609.png)

加入文本

![image-20240130151111397](image-20240130151111397.png)

然后就实现了

![image-20240130151125694](image-20240130151125694.png)

然后创建一个脚本用来判断对话框显示。这里对话框是用碰撞体和射线判断的，记得加上碰撞体。

![image-20240130143243194](image-20240130143243194.png)

接下来写一下脚本

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class NonPlayerCharacter : MonoBehaviour
{
    // 对话框显示的持续时间
    public float displayTime = 4.0f;

    // 对话框的 GameObject 引用
    public GameObject dialogBox;

    // 控制对话框显示时间的计时器
    float timerDisplay;

    void Start()
    {
        // 初始时禁用对话框
        dialogBox.SetActive(false);

        // 使用负值初始化计时器，表示计时器未激活
        timerDisplay = -1.0f;
    }

    void Update()
    {
        // 检查对话框当前是否处于激活状态
        if (timerDisplay >= 0)
        {
            // 根据经过的时间减少计时器的值
            timerDisplay -= Time.deltaTime;

            // 检查计时器是否已经达到或超过零
            if (timerDisplay < 0)
            {
                // 当计时器到期时禁用对话框
                dialogBox.SetActive(false);
            }
        }
    }

    // 方法用于触发对话框的显示
    public void DisplayDialog()
    {
        // 将计时器设置为指定的显示时间
        timerDisplay = displayTime;

        // 激活对话框
        dialogBox.SetActive(true);
    }
}
```

在`RubyController`的`Update`中加入：**物理系统**功能“**射线投射**”

```c#
// 控制对话
if (Input.GetKeyDown(KeyCode.X))
{
    // 从角色当前位置向上偏移0.2个单位，沿着朝向 lookDirection 发射射线
    RaycastHit2D hit = Physics2D.Raycast(rigidbody2d.position + Vector2.up * 0.2f, lookDirection, 1.5f, LayerMask.GetMask("NPC"));

    // 检查射线是否击中了某个物体
    if (hit.collider != null)
    {
        // 再次验证击中的物体确实是属于 "NPC" 图层
        if (hit.collider != null)
        {
            // 从击中的物体获取 NonPlayerCharacter 组件
            NonPlayerCharacter character = hit.collider.GetComponent<NonPlayerCharacter>();

            // 检查获取的组件是否有效
            if (character != null)
            {
                // 触发 NonPlayerCharacter 的 DisplayDialog 方法，显示对话框
                character.DisplayDialog();
            }
        }
    }
}
```

![GIF4](GIF4.gif)

## 制作音频

添加一个物体，用于播放背景音乐。拉入Clip就行

![image-20240130200353123](image-20240130200353123.png)

然后添加一个吃食物的音乐，这里用函数来生成。现在获取生命值的时候加入一个脚本

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class HealthCollectible : MonoBehaviour
{
    public AudioClip collectedClip;

    void OnTriggerEnter2D(Collider2D other)
    {
        RubyController controller = other.GetComponent<RubyController>();

        if (controller != null)
        {
            if (controller.health < controller.maxHealth)
            {
                controller.ChangeHealth(1);
                Destroy(gameObject);
			   // 添加播放音乐
                controller.PlaySound(collectedClip);
            }
        }

    }
}
```

在Ruby controler 中加入如下代码

```c#
    // 声明变量过度
	private AudioSource audioSource;

	
	void Start()
	{
		//获取变量组件
    	audioSource = GetComponent<AudioSource>();
	}
	
	public void PlaySound(AudioClip clip)
    {
        audioSource.PlayOneShot(clip);
    }
```

记得在Ruby中添加音乐播放

![image-20240130201954042](image-20240130201954042.png)

然后踩水果就有音效了。教程到这里游戏就完成了！
