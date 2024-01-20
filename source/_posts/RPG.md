---
title: RPG
date: 2023-10-30 22:56:26
categories: unity
typora-root-url: ./RPG
---

# [RPG](https://www.bilibili.com/video/BV1Xj411r7Mm/?share_source=copy_web&vd_source=0d2493ac4766210855634c1c51302689)

> 这里是我初学在哔哩哔哩跟做的RPG小游戏，记录生活，仅供参考。

## 导入素材

我们素材是在`unity 的 Assert store`中下载的，链接是下面两个：

- [素材1](https://assetstore.unity.com/packages/2d/environments/minifantasy-forgotten-plains-208907)

- [素材2](https://assetstore.unity.com/packages/2d/environments/minifantasy-dungeon-206693)

导入素材就`window - Package Manager` 下载对应的素材即可：

![image-20231031132703256](image-20231031132703256.png)

但是开了梯子就下载不了，我也不知道是什么原因。

下载完成之后我们就得到了两个素材包，打开素材包我们开始构造我们的地图

![image-20231031133124761](image-20231031133124761.png)

## 构造地图

在`Hierarchy`处右键创建`Tile Map` , 这里创建的组建是格子地图。

![image-20231031133338951](image-20231031133338951.png)

然后再颜料板导入我们的地图素材，但是得先切割一下：

![image-20231031133633831](image-20231031133633831.png)

然后就跳转到切割的界面了，设置切割格子的尺寸为8*8

![image-20231031133711587](image-20231031133711587.png)

![image-20231031133959355](image-20231031133959355.png)

切割完就是这个样子的，这里有一个问题，我们切割过细好像回复不了，就变成三角形了。

![image-20231031134201172](image-20231031134201172.png)

导入的Tile需要建一个文件夹来收集，在创建的Gird处新建`TilePalette`。然后拖入素材就是这样了。

![image-20231031134328762](image-20231031134328762.png)

![image-20231031135455945](image-20231031135455945.png)

这里需要注意的是，素材处理最好让它无压缩，还有勾选到`Multipl`
		![image-20231031140050227](image-20231031140050227.png)

-----

开始绘制地图，直接将按调试版的素材就可以在左侧看到我们要画的图片了：

![image-20231031140727982](image-20231031140727982.png)

![image-20231031140739313](image-20231031140739313.png)

![image-20231031144324298](image-20231031144324298.png)

有点太单调了，导入一些树木和墙，步骤和上面一样。新建一个绘制板，这个新绘制板作为旧绘制板的子类，让它覆盖我们之前的地图，就有层次感。

![image-20231031145213933](image-20231031145213933.png)

![image-20231031145219696](image-20231031145219696.png)

创建一个新的Tile,名字为`Obstacle`逻辑是让它和角色进行碰撞，类似于树那些。

创点树

![image-20231031151237483](image-20231031151237483.png)

给创建树的`Tile`赋一个`Tilemap Collider 2D`组件，使得它可以碰撞

![image-20231031152957687](image-20231031152957687.png)

为了节省性能，赋予组件`Composite Collider `。 作用是把重叠起来的物品合为整体

![image-20231101140306648](image-20231101140306648.png)

点击`Used By Composite`，合并

![image-20231101140556421](image-20231101140556421.png)

顺便设置物理属性，设置为静态

![image-20231101140456191](image-20231101140456191.png)

页面也就成了下面这个样子，然后我们拉大摄像头：

![image-20231101143129785](image-20231101143129785.png)

![image-20231101143142868](image-20231101143142868.png)

## 创建人物

创建一个正方形来表示我们的人物

![image-20231101143835013](image-20231101143835013.png)

设置人物的层级为：`Player`。在unity中，层级是由大到小显示的，大覆盖小。



![image-20231101144257198](image-20231101144257198.png)

![image-20231101144623611](image-20231101144623611.png)

然后就可以显示人物了

![image-20231101144748092](image-20231101144748092.png)

为了能简单控制人物，编写一个脚本

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    //获取角色的物理
    Rigidbody2D rb;
    Vector2 moveInput;
    public float moveSpeed;

    private void Awake()
    {
        //获取
        rb = GetComponent<Rigidbody2D>();
    }

    private void FixedUpdate()
    {
        rb.AddForce(moveInput * moveSpeed);
    }

}
```

在人物处导入`Player Input`插件，简化操作

![image-20231101152835760](image-20231101152835760.png)

![image-20231101150536021](image-20231101150536021.png)

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerMovement : MonoBehaviour
{
    //获取角色的物理
    Rigidbody2D rb;
    Vector2 moveInput;
    public float moveSpeed;

    private void Awake()
    {
        //获取
        rb = GetComponent<Rigidbody2D>();
    }

    private void OnMove(InputValue value)
    {
        moveInput = value.Get<Vector2>();   
    }

	//添加移动的逻辑主要是这个 - 逻辑是给物体添加一个力 F = ma
    private void FixedUpdate()
    {
        //移动 F = ma
        rb.AddForce(moveInput * moveSpeed);
    }
}
```

就可以成功移动了

![image-20231101152942528](image-20231101152942528.png)

![image-20231101152928386](image-20231101152928386.png)

但是移动太快了，像是滑冰一样。可以改变阻力和物体质量（改变质量影响加速度）

![image-20231101153917076](image-20231101153917076.png)

## 制作角色动画

制作角色待机动画，需要多张图片构成。打开：

![image-20231125200942136](image-20231125200942136.png)

选中我们的`HumanBaseWalk`发现是一张大图：

![image-20231125201015440](image-20231125201015440.png)

已经被切割好了，我们就用这些图片来制作主角的待机动画。在界面中，新建一个`Animation`文件夹，用于存储我们角色的动画，制作动画的过程很简单：

- 首先，在`window`处打开`animation`

![image-20231125202536034](image-20231125202536034.png)

打开之后是这样的界面:

![image-20231125202717803](image-20231125202717803.png)

`animation`制作动画的方式，是拉入几张画然后按照一定的频率去播放，`Samples`是控制每秒刷新几次。

- 创建待机动画和走路动画，这里就是拉入图片就行。

  ![image-20231125203230059](image-20231125203230059.png)

![image-20231125203241701](image-20231125203241701.png)

实现完就这个样子，就放一张图看看吧：

![录制_2023_11_25_20_42_49_609](录制_2023_11_25_20_42_49_609.gif)

--------

转到动画机，这里是控制动画播放顺序的。角色会在游戏中攻击，受击，达成一定条件的时候就会播放这些动画，在`Animator`中，用连线和控制脚本来判断什么时候执行。

![image-20231125204524448](image-20231125204524448.png)

例如上图，起始连线连到了`playerwalk`这里就会一直播放这个动画，我们需要将起始动画设置为`PlayerIdle`

![image-20231125205336241](image-20231125205336241.png)

连的箭头表示状态过度，对这些箭头进行过度编辑，来实现我们需要的效果，需要注意的是：

- 在`Parameters`处，创建过度条件变量，这里创建了一个bool变量，true过度，false不过度。

  ![image-20231125205709463](image-20231125205709463.png)

- 在箭头处我们可以控制过度效果：图中我们标记了两个，第一个`Has Exit Time`是控制过度时间，使动画达到瞬时转化的效果.第二个`Transition Duratior`是控制过度间隔时间，简单来说，就是会播放过度前动画的一小部分之后再播放过度之后的动画。这里不需要，我们改为0.

  ![image-20231125210152217](image-20231125210152217.png)

- 两个箭头处的`conditions`设置变量，之后再编写脚本的时候调用这两个变量的bool值就可以切换动画了。

  ![](image-20231125211341966.png)

![image-20231125211348498](image-20231125211348498.png)

编写状态动画代码，这里只是展示部分：

```c#
	//声明一个变量 - 存储我们的animator
	Animator animator;
	//创建的函数 - 控制角色的运动
    private void OnMove(InputValue value)
    {
        //控制移动的逻辑
        moveInput = value.Get<Vector2>();
        //如果捕捉移动为0，播放idle
        if(moveInput == Vector2.zero)
        {
            //对isWalking控制
            animator.SetBool("isWalking",false);
        }             

    }
```

这样子，每当控制方向移动都会判断是否要播放这两个动画了，展示如下

![录制_2023_11_25_21_36_47_122](录制_2023_11_25_21_36_47_122.gif)

但是现在行走很怪，我们需要按左键就让它翻转，翻转在`Player`的`Flip`这里，我们只需要在脚本处加个判断即可。

![image-20231125213820512](/image-20231125213820512.png)

代码逻辑如下：

```c#
//创建变量
//这里获取Flip
SpriteRenderer spriteRenderer;

private void OnMove(InputValue value)
    {
        //控制移动的逻辑
        moveInput = value.Get<Vector2>();
        if(moveInput == Vector2.zero)
        {
            //控制移动动画播放
            animator.SetBool("IsWalk", false);
        }
        else
        {
            animator.SetBool("IsWalk", true);
            if(moveInput.x>0)
            {
                spriteRenderer.flipX = false;

            }
            if(moveInput.x<0)
            {
                spriteRenderer.flipX= true;
            }
        }

    }
```

## 实现镜头移动

下载一个插件，用于跟随镜头：

![image-20231125215356077](image-20231125215356077.png)

创建一个`2dCamera`

![image-20231125215824751](image-20231125215824751.png)

- 将player拖入到follow，让相机随着player移动

![image-20231125215936669](image-20231125215936669.png)

这里补充一个知识点：

- 将我们创建的物品拉入到文件夹中可以获得这个物品的预制体，预制体的所有配置和我们创建的物品相同。拉入会变蓝。

  ![image-20231126195521070](image-20231126195521070.png)

## 制作小怪

和制作角色一样，先在界面处创建一个2d的物品

![image-20231126210643767](image-20231126210643767.png)

然后新建一个动画机，拖入图片

![image-20231126211227095](image-20231126211227095.png)

就可以得到我们的小怪了

![image-20231126211245694](image-20231126211245694.png)

然后给小怪设置一些基本的属性

![image-20231126211334541](image-20231126211334541.png)

制作四个行为动画：

- 死亡

  ![录制_2023_11_26_21_52_10_418](录制_2023_11_26_21_52_10_418.gif)

- 运动

  ![录制_2023_11_26_21_52_34_846](录制_2023_11_26_21_52_34_846.gif)

- 待机

  ![录制_2023_11_26_21_49_31_620](录制_2023_11_26_21_49_31_620.gif)

- 受击

![录制_2023_11_26_21_53_45_279](录制_2023_11_26_21_53_45_279.gif)

## 制作小怪的ai

设计一个简单的小怪ai，逻辑是声明一个以这个小怪为圆心的圆，然后检测我们的角色是否在这个圆的面积之内，如果是就追踪角色，不是就停止。

```c#
    private void FixedUpdate()
    {
        if (detectionZone.detectedObjs != null)
        {
            //计算位置 玩家位置 - 怪物位置
            Vector2 direction = (detectionZone.detectedObjs.transform.position - transform.position);
            if (direction.magnitude <= detectionZone.viewRadius)
            {
				//沿 force 矢量的方向连续施加力。
                rb.AddForce(direction.normalized * speed);
            }
        }
        
    }
```

`detectionZone.detectedObjs` 这个是在`DetectionZone`这里出现的，表示的是获取处理之后的碰撞器（应该是player的）

```
   public Collider2D detectedObjs;
   public LayerMask playerLayerMask; //扫描的layer
   void Update()
    {
        //位置 半径 目标 - 这个方法返回的是 Collider2D 与圆重叠的碰撞器。
        Collider2D collider =  Physics2D.OverlapCircle(transform.position, viewRadius, playerLayerMask);

        if(collider != null)
        {
            detectedObjs = collider;
        }

    }
```

半径的效果：在unity可以输入变量的值。

![image-20231126221908087](image-20231126221908087.png)

这里需要将，player和Oct的碰撞体打开，才可以获取我们layer的碰撞体。我就是犯了这个错误找了半个小时。原理就是用碰撞体定位方位，然后用`add.force`对这个矢量方向施加一个力，来运动。演示如下：

![录制_2023_11_27_22_16_16_838](录制_2023_11_27_22_16_16_838.gif)

同时我们应该限制，两个角色之间的z轴坐标：如果打开它们就会转圈圈

![image-20231129211451721](/image-20231129211451721.png)

同样的，对于小怪也需要搞方向，这里的原理是最开始的方向基准为0，向左就小于0，向右就大于0.通过翻转 `spriteRenderer` 的 `flipX` 属性来实现。

![录制_2023_11_29_21_29_08_794](录制_2023_11_29_21_29_08_794.gif)

## 制作角色攻击

首先制作角色攻击动画，拉入图片然后设置刷新率就行。

![录制_2023_11_29_22_06_18_832](录制_2023_11_29_22_06_18_832.gif)

在下载的插件处，有预设的攻击方式：``

![image-20231129221120631](image-20231129221120631.png)

这里的`Left Button`是控制鼠标攻击：

![image-20231129221224064](image-20231129221224064.png)

在`playManager`处添加攻击动画

```c#
    void OnFire()
    {
        animator.SetTrigger("swordAttack");
    }

```

就可以成功了：

![录制_2023_11_29_22_21_05_862](录制_2023_11_29_22_21_05_862.gif)

但是现在攻击是没有作用的，因为我们的攻击只是一个动画，所以说我们需要实例化这个动画。在player处建一个object，我们要对这个object进行操作。

![image-20231129224822837](image-20231129224822837.png)

在挥剑最大的动画，需要配置一个碰撞体（好像直接设置触发器就行），上面已经创建了物体了，接下来配置一下：左上角是标记，然后做了一个碰撞箱。

![image-20231129232145618](/image-20231129232145618.png)

![image-20231129232255491](image-20231129232255491.png)

然后再动画里面，插入碰撞体

![image-20231129232459448](image-20231129232459448.png)

设置这条竖线再我们需要的动画帧处：这里表示再这个位置处启动BOX，记得在player里面吧box关掉，然后就可以正常使用了。

![image-20231129232554020](image-20231129232554020.png)

![录制_2023_11_30_10_48_48_862](录制_2023_11_30_10_48_48_862.gif)

但是这样还不够，角色在移动的时候剑不会跟着角色一起转方向，这里新建一个类来实现挥剑的动作：

```c#
    //控制剑的方向
    Vector3 position;

    void Start()
    {
        //位置基准是以父物体的
        position = transform.localPosition;
    }

    //函数翻转剑
    void IsFacingRight(bool isFacingRight)
    {
        if (isFacingRight)
        {
            transform.localPosition = position;
        }
        else
        {
            //单单翻转x
            transform.localPosition = new Vector3(-position.x,position.y,position.z);
        }
    }

```

调用的时候，我们采用父类调用子类的方法：因为这个剑的碰撞体是在player上的嘛。

```c#
private void OnMove(InputValue value)
    {
        //控制移动的逻辑
        moveInput = value.Get<Vector2>();
        if(moveInput == Vector2.zero)
        {
            //控制移动动画播放
            animator.SetBool("IsWalk", false);
        }
        else
        {
            animator.SetBool("IsWalk", true);
            if(moveInput.x>0)
            {
                spriteRenderer.flipX = false;
                //调用这个物体的子物体的方法
                gameObject.BroadcastMessage("IsFacingRight", true);

            }
            if(moveInput.x<0)
            {
                spriteRenderer.flipX= true;
                gameObject.BroadcastMessage("IsFacingRight", false);
            }
        }

    }
```

## 制作受击脚本

创建两个脚本，`DamageableCharater`和`IDamageable`.其中`IDamageable`作为接口依附在`DamageableCharater`.

`IDamageable`接口抽象出来的意思是挂载在物品上就认为是可摧毁的

```c#
//接口才能被继承
public interface IDamageable
{
    //挂上这个脚本就是攻击？
    public void OnHit(int damage, Vector2 knockback);    
}

```

`DamageableCharater` 重写方法

```c#
public class DamageableCharater : MonoBehaviour, IDamageable
{
    Rigidbody2D rb;
    Collider2D physicsCollider;

    public void OnHit(int damage,Vector2 knockback)
    {
        rb.AddForce(knockback);
    }

    private void Awake()
    {
        rb = GetComponent<Rigidbody2D>();
        physicsCollider = GetComponent<Collider2D>();
    }

}
```

在`sword`处将`box2d`配置为触发器

![image-20231130133501319](image-20231130133501319.png)

设置打击时实现怪物击退效果，

![image-20231130133953588](image-20231130133953588.png)

```c#
    private void OnTriggerEnter2D(Collider2D collider)
    {
        IDamageable damageable = collider.GetComponent<IDamageable>();
        if(damageable != null )
        {
            Vector3 _position = transform.parent.position;
            Vector2 direction = collider.transform.position - _position;

            attackPower = 1;
            damageable.OnHit(attackPower, direction.normalized * knockbackForce);
        }
    }
```

原理是给打击对象施加一个力，让它远离自己。施加函数是`rb.AddForce(knockback);`

![录制_2023_11_30_13_51_45_258](录制_2023_11_30_13_51_45_258.gif)

## 配置剩余动画

创建两个触发器，来触发我们的动画：

![image-20231130143608642](image-20231130143608642.png)

在这个的基础之上绘制我们的动画转移图

![image-20231130143629369](image-20231130143629369.png)

对于过度的箭头，我们设置为无过度时间且过度间隔为0.但是在受击恢复行动的时候，我们需要播放完受击动画才回到正常状态。在死亡的时候也需要播放完成，设置为下面这张图的状态。

![image-20231130144204167](image-20231130144204167.png)

然后将死亡动画取消循环播放：

![image-20231130144857701](image-20231130144857701.png)

在orc管理脚本中添加两个播放动画

```c#
    void OnDamage()
    {
        animator.SetTrigger("isDamage");
    }

    void OnDie()
    {
        animator.SetTrigger("isDead");
    }
```

在受击脚本中添加一个属性，`health`用于配置怪物的血量

```c#
    public int Health
    {
        get { return health; }
        set { health = value;
            if (health <= 0)
            {
                //血量小于0 播放死亡
                gameObject.BroadcastMessage("OnDie");
            }
            else
            {	
                //大于0播放受击
                gameObject.BroadcastMessage("OnDamage");
            }    
        
        }
    }
```

然后就实现完成了：

![录制_2023_11_30_14_52_27_209](录制_2023_11_30_14_52_27_209.gif)

对于小怪死亡还在追踪角色，我们将他的`Simulated`关闭即可

![image-20231130150226190](image-20231130150226190.png)

然后就可以实现了：

![录制_2023_11_30_15_03_56_510](录制_2023_11_30_15_03_56_510.gif)

## 死亡动画添加事件

在动画处我们可以用这个小东西添加事件，这里可以调用我们生成的类的方法来实现死亡动画的清除

![image-20231130213553575](image-20231130213553575.png)

在结尾帧处，插入我们消除物品的事件

![image-20231130213651267](image-20231130213651267.png)

## 小怪攻击

攻击和player攻击模式就行，就是血量下降到0就死亡。在player处搭载，调用小怪的方法，但是现在没有主角受击的情况，我们需要添加。

![image-20231130215741056](image-20231130215741056.png)

添加了几个动画：

![image-20231130221959593](image-20231130221959593.png)

运用我们之前的事件，在死亡结尾处调用函数：

![image-20231130222044248](image-20231130222044248.png)

效果达成：

![image-20231130222110951](image-20231130222110951.png)

## 伤害飘字

制作一个3D类型的字体，让我们脚本可以挂载

![image-20231202105829509](image-20231202105829509.png)

原理是小怪受到攻击掉血的时候，我们将掉血的值创建出来。我们创建一个gameobject来挂载这个脚本

![image-20231202105944654](image-20231202105944654.png)

获取这个脚本利用属性和文件定位：

```c#
    private static GameAssets instance;
    //位置
    public Transform DamagePopop;

    public static GameAssets Instance
    {
        get
        {
            if(instance == null)
            {
                //在resource文件夹中找到GameAssets
                instance = Resources.Load<GameAssets>("GameAssets");
            }
            return instance;
        }
    }
```

对于其挂载的Text我们对其构造一个脚本

```c#
private TextMeshPro textMesh;
    private Vector3 moveVector;
    public float disappearTimer;
    public float disappearSpeed;
    private Color textColor;
    private const float DISAPPEAR_TIMER_MAX = 1f;

    private static int sortingOrder;


    private void Awake()
    {
        textMesh = transform.GetComponent<TextMeshPro>();
        textColor = textMesh.color;
    }
    //创建飘字
    public static DamagePopup Create(Vector3 position,int damageAmount,bool isCritical)
    {
        Transform damagePopupTransform = Instantiate(GameAssets.Instance.DamagePopop,position,Quaternion.identity);
        DamagePopup damagePopup = damagePopupTransform.GetComponent<DamagePopup>();
        damagePopup.Setup(damageAmount,isCritical);
        return damagePopup;
    }
    //飘字设置
    private void Setup(int damageAmount,bool iscriticalHit)
    {
        textMesh.SetText(damageAmount.ToString());
        if (!iscriticalHit)
        {
            textMesh.fontSize = 5;
        }
        else
        {
            textMesh.fontSize = 7;
            textColor = Color.red;            
        }
        textMesh.color = textColor;
        disappearTimer = DISAPPEAR_TIMER_MAX;

        //数字转字符
        moveVector = new Vector3(0.7f,1)*10;
        sortingOrder++;
        textMesh.sortingOrder = sortingOrder; 

    }

    private void Update()
    {
        transform.position += moveVector * Time.deltaTime;
        moveVector -= moveVector*7 * Time.deltaTime;

        if(disappearTimer > DISAPPEAR_TIMER_MAX*0.5f)
        {
            float increaseScaleAmount = 1f;
            transform.localScale += Vector3.one * increaseScaleAmount * Time.deltaTime;
        }
        else
        {
            float drcreaseScaleAmount = 1f;
            transform.localScale -= Vector3.one * drcreaseScaleAmount * Time.deltaTime;
        }

       
        disappearTimer -= Time.deltaTime;
        if(disappearTimer < 0)
        {
            //降低透明度
            textColor.a -= disappearSpeed * Time.deltaTime;
            textMesh.color = textColor;
            if (textColor.a < 0)
            {
                Destroy(gameObject);
            }
            
        }
    }
```

这里需要提到的只有两个：

- 这个函数是为了创造飘字

  ```c#
  public static DamagePopup Create(Vector3 position,int damageAmount,bool isCritical)
      {
          Transform damagePopupTransform = Instantiate(GameAssets.Instance.DamagePopop,position,Quaternion.identity);
          DamagePopup damagePopup = damagePopupTransform.GetComponent<DamagePopup>();
          //damagePopup.Setup(damageAmount,isCritical); //添加花里胡哨的特效
          return damagePopup;
      }
  ```

- Update挂载的代码是为了让字消失

  ![录制_2023_12_02_11_07_21_596](录制_2023_12_02_11_07_21_596.gif)

-------

以上完结！！！
