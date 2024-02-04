---
title: Complete Tutorial
date: 2024-02-02 13:20:14
tags: 工程
categories: unity
typora-root-url: ./Complete-Tutorial
---

# [谷歌小恐龙游戏复刻](https://www.youtube.com/watch?v=UPvW8kYqxZk)

## [素材导入](https://github.com/zigurous/unity-dino-game-tutorial)

在作者的GitHub中，找到对应素材的png，也就是照片导入到我们创建的2D工程中，这里创建了一个Sprites文件夹容纳这些照片素材.当然也可以在谷歌小恐龙游戏按F12拉取。

![image-20240202134039530](image-20240202134039530.png)

然后设置一下这些图片素材的PPU，这里设置为96。是为了适应我们素材中的跑到

> Pixels Per Unit 表示在 Unity 中一个单位（Unity单位）的长度对应多少个像素。

![image-20240202134436380](image-20240202134436380.png)

然后设置一下图片的过滤模式，调为Point(no filter)

>在Unity中，"Filter Mode"（过滤模式）是用于控制纹理在被缩放或拉伸时的采样方式的设置。Unity支持三种主要的过滤模式：
>
>1. **Point**: 这是最基本的过滤模式，也称为“最近邻”或“点采样”。在这种模式下，当纹理被缩放或拉伸时，像素的颜色值采用最近的一个像素的值，而不会进行插值。这可能导致在纹理上看到锯齿状的边缘或失真，特别是在纹理被放大时。
>2. **Bilinear**: 这是一种插值技术，使用周围的四个像素的加权平均来计算新的颜色值。这会在缩放或拉伸时产生相对平滑的结果，减少了锯齿状的边缘。
>3. **Trilinear**: 这是在Bilinear的基础上添加了Mipmap的过滤模式。Mipmap是纹理的预先生成的不同分辨率的版本，用于在远处或不同缩放级别下提高性能和视觉质量。Trilinear过滤在Bilinear的基础上对相邻的两个Mipmap级别进行插值，以进一步减少纹理缩放时的颜色过渡。

分辨率调为4096，因为这个图片是4K的嘛。接着调整Wrap Mode（环绕模式:是用于定义纹理坐标超出标准范围时的处理方式)。调整为Repeat。

> Unity支持以下三种主要的Wrap Mode：
>
> 1. **Repeat**: 这是默认的环绕模式。当纹理坐标超出0到1的范围时，纹理将被重复。例如，如果纹理坐标是1.2，那么它将被视为0.2，从而在纹理上重复。
> 2. **Clamp**: 在这个模式下，超出范围的纹理坐标会被截断到0到1的范围。这意味着纹理坐标超出1或小于0的部分将被强制设置为1或0，以避免重复。
> 3. **Mirror**: 这个模式下，超出范围的纹理坐标会被镜像。例如，如果纹理坐标是1.2，它将被镜像为0.8，这样纹理就会像镜子中的图像一样进行翻转。

得到了如下的设置，这里记得是把全部图片素材按shift整理在一起：

![](image-20240202141245168.png)

## 铺设世界

这里拉入我们的轨道，这是小恐龙走路的地方。在界面处拖入精灵就行

![image-20240202141739533](./image-20240202141739533.png)

但是这里颜色太深了，这里调整为白色的底，调整的是摄像头的

![image-20240202141944555](image-20240202141944555.png)

![image-20240202142001688](image-20240202142001688.png)

这里发现线太上了，我们拉下一些。这里调整Position中的Y轴，调整的就是位置。顺便设置一下摄像头的尺寸(游戏框显示的矩阵大小)

![image-20240202142959382](image-20240202142959382.png)

> 以下是 `Transform` 的主要作用：
>
> 1. **位置（Position）：** `Transform` 定义了物体在三维空间中的位置。通过修改 `Transform.position` 属性，你可以将游戏对象移动到场景中的不同位置。
> 2. **旋转（Rotation）：** `Transform` 定义了物体的旋转角度。通过修改 `Transform.rotation` 属性，你可以旋转游戏对象，改变它的方向。
> 3. **缩放（Scale）：** `Transform` 定义了物体在各个轴上的缩放比例。通过修改 `Transform.localScale` 属性，你可以调整游戏对象的尺寸。
> 4. **层级关系：** `Transform` 组成了一个层级结构，反映了游戏对象之间的父子关系。通过修改 `Transform.parent` 属性，你可以更改对象的父级，从而改变其在场景中的层级位置。
> 5. **坐标空间转换：** `Transform` 提供了一些方法，如 `Transform.TransformPoint` 和 `Transform.InverseTransformPoint`，用于在不同坐标空间之间进行转换。这在处理对象之间的相对位置时非常有用。

在这个物件上，我们加入一个Box Collider。目的是让小恐龙能站在上面，记得调整一下Box的大小使得它可以站在上面。

![image-20240202155459419](image-20240202155459419.png)

![image-20240202155749548](image-20240202155749548.png)

之后拉入我们的恐龙，给恐龙配置一个组件[Character Controller](https://blog.csdn.net/m0_61777011/article/details/125457481).可以简单理解为代替rigid body的组件，比起刚体少了一些物理属性。

![image-20240202143457334](image-20240202143457334.png)

创建一个脚本控制恐龙的移动。

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Player : MonoBehaviour
{
    // 代替刚体的组件
    private CharacterController character;
    private Vector3 direction;
	//控制重力
    public float gravity = 9.18f * 2f;
    //跳跃的力度
    public float jumpFore = 8f;

    private void Awake()
    {
        character = GetComponent<CharacterController>();
    }

    private void OnEnable()
    {
        direction = Vector3.zero;
    }

    private void Update()
    {
        //位置移动
        direction += Vector3.down * gravity * Time.deltaTime;

        if(character.isGrounded)
        {
            direction = Vector3.down;

            if (Input.GetButton("Jump"))
            {
                direction = Vector3.up * jumpFore;
            }

        }
        character.Move(direction*Time.deltaTime);
    }

}
```

然后就可以了：

![GIF4](GIF4.gif)

然后补充一些知识点：

> `GetComponent` 是 Unity 引擎中的 MonoBehaviour 类提供的一个方法，用于获取挂载在同一 GameObject 上的其他组件的引用。
>
> `Input.GetButton("Jump")` 是Unity自带的控制按键，可以在Project Settings中查询具体的按键信息
>
> ![image-20240202161851192](image-20240202161851192.png)
>
> `Time.deltaTime` 是 Unity 中的一个变量，用于在游戏中进行时间相关的计算。它表示自上一帧（frame）到当前帧所经过的时间，以秒为单位。

我们的目的是让场景动起来，但是玩家并不会动，这种游戏类型被称之为无限。创建一个游戏管理器，我们需要让背景移动的越来越快，这里用到单例设计模式。

这一部分是按照时间来增加`gameSpeed` 变量

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class GameManager : MonoBehaviour
{
    //声明单例 - 用属性
    public static GameManager Instance { get; private set; }

    public float initialGameSpeed = 5f;
    //每隔一段时间加速
    public float gameSpeedIncrease = 0.1f;

    public float gameSpeed {  get; private set; }

    private void Awake()
    {
        if(Instance == null)
        {
            Instance = this;
        }
        else
        {
            DestroyImmediate(gameObject);
        }
    }

    private void OnDestroy()
    {
        // this表示当前类的实例
        if( Instance == this)
        {
            Instance = null;
        }
    }


    private void Start()
    {
        NewGame();    
    }

    private void NewGame()
    {
        gameSpeed = initialGameSpeed;

    }

    private void Update()
    {
        gameSpeed += gameSpeedIncrease * Time.deltaTime;
    }

}

```

然后处理背景，我们不可能调整`Transform`中的`Position`中的x来调整地图移动。只能用地图平铺来设计

删除Sprite Renderer，添加Mesh Renderer来构造平铺效果。

![image-20240202175206713](image-20240202175206713.png)

这里的Quad我们需要搭载一个材质，这里创建一下

![image-20240202175729834](image-20240202175729834.png)

选择材质Transparent，拖入我们的地图素材

![image-20240202175806313](image-20240202175806313.png)

完成之后就可以使用Offset这个变量来调整地图平铺了。

![image-20240202175920320](./image-20240202175920320.png)

编写一个脚本，使得随着时间的变化平铺我们的地图

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEditor;
using UnityEngine;

public class Ground : MonoBehaviour
{
    private MeshRenderer mershRenderer;

    private void Awake()
    {
        mershRenderer = GetComponent<MeshRenderer>();
    }

    private void Update()
    {
        // 移动速度
        float speed = GameManager.Instance.gameSpeed / transform.localScale.x;
        mershRenderer.material.mainTextureOffset += Vector2.right * speed * Time.deltaTime; 
    }
}
```

 然后启动游戏看看效果

![GIF5](GIF5.gif)

## 制作动画

接下来为小恐龙制作动画，跳跃和走路让这个恐龙更加自然。这里因为动画需要匹配环境的播放速度，所以说我们不用动画机制作这些动画，而是采用写代码调用精灵的方式制作。

创建一个脚本，叫做`AnimationSprite`

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class AnimationSprite : MonoBehaviour
{
    // 这里创建一个数组 - 容纳我们的动画精灵
    public Sprite[] sprites;
	// 负责渲染 2D 精灵
    private SpriteRenderer spriteRenderer;
    private int frame;

    private void Awake()
    {
        spriteRenderer = GetComponent<SpriteRenderer>();    
    }

    private void OnEnable()
    {
        // 调整Animate函数启动时间 - 防止分母为0 - 这里是在0f之后调用
        Invoke(nameof(Animate), 0f);
    }

    private void OnDisable()
    {
        CancelInvoke();
    }


    private void Animate()
    {
        //数组索引
        frame++;

        if(frame >= sprites.Length)
        {
            frame = 0;
        }

        // 索引当前的精灵
        if(frame >= 0 && frame < sprites.Length)
        {
            spriteRenderer.sprite = sprites[frame];
        }
        //Invoke 函数用于在一定时间延迟之后执行指定的方法。它允许你调度在将来某个时间点执行的函数，而不是在每一帧都执行。
        //这里是按照gameSpeed的速率加速播放动画
        Invoke(nameof(Animate),1f/GameManager.Instance.gameSpeed);
    }

}

```

补充一下知识点：

> `SpriteRenderer` 是 Unity 引擎中用于在场景中渲染 2D 精灵（Sprite）的组件。它是 Unity 2D 渲染系统的一部分，用于在游戏中显示平面图像、角色、背景等元素。`SpriteRenderer` 主要负责两个方面的工作：
>
> 1. **渲染 2D 精灵：** 将关联的 2D 精灵呈现到屏幕上。精灵可以是图像、角色、UI 元素等。`SpriteRenderer` 接收精灵的纹理（Texture）并在场景中绘制它们。
> 2. **处理材质和着色：** 通过 `SpriteRenderer`，你可以指定与精灵相关联的材质（Material），并控制渲染时的颜色、透明度等属性。这允许你在运行时改变精灵的外观，比如淡入淡出、颜色变化等。

然后就可以了：

![GIF6](GIF6.gif)

接下来创建障碍物的预制件，配置Box Collider，并且点击触发器。这里逻辑是小恐龙碰撞触发器游戏就会结束，这里制作如下预制件

![image-20240202210540581](image-20240202210540581.png)

预制件的具体配置是：配置一个tag

![image-20240202210827332](image-20240202210827332.png)

配置`Box Collider`，设置Is Trigger

![image-20240202210929051](image-20240202210929051.png)

添加动画，这里是因为这是鸟，其他障碍物就不用了。

![image-20240202211001184](image-20240202211001184.png)

然后需要生成这些障碍物，我们创建一个物体挂载生成脚本

![image-20240202230952897](image-20240202230952897.png)

脚本代码如下：

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Obstacle : MonoBehaviour
{
    // 定义边界 - 摧毁
    private float leftEdge;
    private void Start()
    {
        // 这里是用主相机视角定义摧毁边界
        leftEdge = Camera.main.ScreenToWorldPoint(Vector3.zero).x - 2f;
    }

    private void Update()
    {
        // 生成位置？ - 这里需要考虑时间 - 有关速度都需要考虑
        // 每一帧都会更新 - 所以说就有移动的效果
        transform.position += Vector3.left * GameManager.Instance.gameSpeed * Time.deltaTime;

        if(transform.position.x < leftEdge)
        {
            Destroy(gameObject);
        }
    }
}
```

![image-20240203143821370](image-20240203143821370.png)

> `Camera:` 这里是主相机视角，根据`Tag：MainCamera`标签定位

![GIF6](GIF6-1706942647654-1.gif)

## 碰撞结束

现在来制作碰撞效果，小恐龙碰撞之后游戏结束

我们用之前定义的`tag:Obstacle` 来定位我们碰撞的物体。在Player.cs中添加如下函数

```c#
    // 获取碰撞体的信息
    private void OnTriggerEnter(Collider other)
    {
        if (other.CompareTag("Obstacle"))
        {
            GameManager.Instance.GameOver();
        }
    }
```

在`GameManager`中处理有关游戏结束的事件

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class GameManager : MonoBehaviour
{
    //声明单例 - 用属性
    public static GameManager Instance { get; private set; }

    public float initialGameSpeed = 5f;
    //每隔一段时间加速
    public float gameSpeedIncrease = 0.1f;

    // 这里是获取实体
    private Player player;
    private Spawner spawner;

    public float gameSpeed {  get; private set; }

    private void Awake()
    {
        if(Instance == null)
        {
            Instance = this;
        }
        else
        {
            DestroyImmediate(gameObject);
        }
    }

    private void OnDestroy()
    {
        // this表示当前类的实例
        if( Instance == this)
        {
            Instance = null;
        }
    }


    private void Start()
    {
        // 获取实体
        player = FindObjectOfType<Player>();
        spawner = FindObjectOfType<Spawner>();

        NewGame();    
    }
	
    //启动新游戏
    private void NewGame()
    {
        // 当前场景中的全部对象 - 游戏结束的时候需要删除
        Obstacle[] obstacles = FindObjectsOfType<Obstacle>();

        foreach (Obstacle obstacle in obstacles)
        {
            Destroy(obstacle);
        }

		//重置游戏速度
        gameSpeed = initialGameSpeed;
        enabled = true;

        player.gameObject.SetActive(true);
        spawner.gameObject.SetActive(true);

    }

    

    public void GameOver()
    {
        // 游戏结束 - 速度不再增加
        gameSpeed = 0f;
        enabled = false;

        // 生成器停止
        player.gameObject.SetActive(false);
        spawner.gameObject.SetActive(false);
    }

    private void Update()
    {
        gameSpeed += gameSpeedIncrease * Time.deltaTime;
    }
}

```

> `SetActive` 方法是 Unity 引擎中的内置方法，用于控制游戏对象的活动状态。每个 Unity 的游戏对象都有一个 `gameObject` 属性，通过该属性可以访问到 `SetActive` 方法。

![GIF7](GIF7.gif)

## 制作UI

创建一个画布，承载我们的UI

![image-20240204150448180](image-20240204150448180.png)

这Canvas的子对象都是Text

![image-20240204150548307](./image-20240204150548307.png)

这里演示创建其中的一个，因为其它创建的操作基本一致

![image-20240204150722417](image-20240204150722417.png)

这里的Font Asset是字体，我们拖入对应创建好的字体模板

![image-20240204150851661](image-20240204150851661.png)

然后就算创建完成了，这里需要注意的是对应的UI需要设置好锚点，供给分辨率调整UI的位置。

![image-20240204151658321](image-20240204151658321.png)

然后我们需要控制UI的显示，例如游戏开始时显示积分，游戏结束之后显示GameOver。这里在GameManager中，编写脚本

```c#
using UnityEngine;
using UnityEngine.UI;

public class GameManager : MonoBehaviour
{
    //声明单例 - 用属性
    public static GameManager Instance { get; private set; }

    public float initialGameSpeed = 5f;
    //每隔一段时间加速
    public float gameSpeedIncrease = 0.1f;

    // 这里是获取实体
    private Player player;
    private Spawner spawner;
	
    // UI的挂载变量
    public TextMeshProUGUI gameOverText;
    public Button retryButton;
    public TextMeshProUGUI scoreText;
    public TextMeshProUGUI hiscoreText;

    private float score;

    public float gameSpeed {  get; private set; }

    private void Awake()
    {
        if(Instance == null)
        {
            Instance = this;
        }
        else
        {
            DestroyImmediate(gameObject);
        }
    }

    private void OnDestroy()
    {
        // this表示当前类的实例
        if( Instance == this)
        {
            Instance = null;
        }
    }


    private void Start()
    {
        // 获取实体
        player = FindObjectOfType<Player>();
        spawner = FindObjectOfType<Spawner>();

        NewGame();    
    }

    public void NewGame()
    {
        // 当前场景中的全部对象
        Obstacle[] obstacles = FindObjectsOfType<Obstacle>();

        // 清除
        foreach (var obstacle in obstacles)
        {
            Destroy(obstacle.gameObject);
        }
        score = 0f;

        gameSpeed = initialGameSpeed;
        enabled = true;

        player.gameObject.SetActive(true);
        spawner.gameObject.SetActive(true);
        gameOverText.gameObject.SetActive(false);
        retryButton.gameObject.SetActive(false);
        UpdateHiscore();
    }

    

    public void GameOver()
    {
        // 游戏结束 - 速度不再增加
        gameSpeed = 0f;
        enabled = false;

        // 生成器停止
        player.gameObject.SetActive(false);
        spawner.gameObject.SetActive(false);
        //游戏结束显示
        gameOverText.gameObject.SetActive(true);
        retryButton.gameObject.SetActive(true);
        UpdateHiscore();
    }

    private void Update()
    {
        gameSpeed += gameSpeedIncrease * Time.deltaTime;
        // 根据时间加分数
        score += gameSpeed * Time.deltaTime;
        scoreText.text = score.ToString();
        scoreText.text = Mathf.FloorToInt(score).ToString("D5");
    }
    
	// 这里是保存UI分数的
    public void UpdateHiscore()
    {
        // 注册表
        float hiscore = PlayerPrefs.GetFloat("hiscore",0);

        if(score>hiscore)
        {
            hiscore = score;
            PlayerPrefs.SetFloat("hiscore", hiscore);
        }
        hiscoreText.text = Mathf.FloorToInt(hiscore).ToString("D5");
    }

}
```

脚本中用`SetActive`来控制UI的显示，这里再游戏开始和游戏结束的时候调用。基本上每一个部件都会有一个这样的函数，可以用来控制预制体的动画。

> 调用 `SetActive(true)` 时，对象将变为活动状态，它将在场景中可见并响应交互。相反，当你调用 `SetActive(false)` 时，对象将被禁用，它将在场景中不可见且不响应交互。

这里用`PlayerPrefs`存储最高分数。

> `PlayerPrefs` 是Unity中的一个用于存储和检索玩家偏好设置或游戏数据的类。它提供了一种简单的方式，可以在游戏中保存和读取数据，而不需要使用更复杂的文件系统或数据库。
>
> 主要用途包括保存玩家的最高分、解锁的关卡、游戏设置等。这些数据会在游戏重新启动时保留，即使应用程序关闭和重新打开，这些数据仍然存在。
>
> `PlayerPrefs` 使用键值对的形式存储数据，其中键是字符串，值可以是整数、浮点数或字符串等。这使得在游戏中轻松地保存和检索简单的用户数据成为可能。

## 打包

这里打包遇到一个问题，我之前删除了命名错误的Tag，但是没有重启。导致打包完成之后检测不到这个碰撞体，这里记得删除Tag之后重启unity。

![image-20240204153027803](image-20240204153027803.png)

点击`File->Build Settings`

![image-20240204153201035](image-20240204153201035.png)

选择添加Scenes,可以理解为你创建的游戏世界，然后点击创建就可以啦。

![image-20240204153430603](image-20240204153430603.png)

然后运行一下：

![GIF8](GIF8.gif)
