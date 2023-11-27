---
title: RPG
date: 2023-10-30 22:56:26
tags: 工程
categories: 工程
typora-root-url: ./RPG
---

# RPG

## 导入素材

我们素材是在`unity 的 Assert store`中下载的，链接是下面两个：

- [素材1](https://assetstore.unity.com/packages/2d/environments/minifantasy-forgotten-plains-208907)

- [素材2](https://assetstore.unity.com/packages/2d/environments/minifantasy-dungeon-206693)

导入素材就`window - Package Manager` 下载对应的素材即可：

![image-20231031132703256](image-20231031132703256.png)

但是开了梯子就下载不了，我也不知道是什么原因。

下载完成之后我们就得到了两个素菜包，打开素材包我们开始构造我们的地图

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

`detectionZone.detectedObjs` 这个是在`DetectionZone`这里出现的

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



