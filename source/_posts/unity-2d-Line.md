---
title: unity 2d Line
date: 2023-10-12 14:19:34
tags: 工程
categories: 工程
typora-root-url: ./unity-2d-Line
hidden: true
---

## 菜单搭建

搭建一个简单的菜单界面，添加了下雨的特效

![image-20231012143020995](image-20231012143020995.png)

主相机配置

在主相机创建两个文本，`Text`。在其中设置锚点处为界面顶，如图所示：

![image-20231012142931745](image-20231012142931745.png)

当然，锚点也可以设置为以父节点为基准，也就是说父结点运动我们的子节点会跟着运动，关于父结点和子节点的关系：

![image-20231012143547929](image-20231012143547929.png)

在`Canvas`下的都是子节点，`Canvas`是父结点。将物件和按钮调整到合适的位置我们加一些组件。

![image-20231012143709780](image-20231012143709780.png)

`shadow`给字体添加阴影，阴影的原理是和字体不太同一个平面内就形成了阴影，在`2d`中，我们的光源不可能在`z`轴中出现，所以说就用不同平面表示阴影。`x,y`表示阴影平面的偏移。

![image-20231012143942632](image-20231012143942632.png)

我们还需要对摄像机画面进行修改，让它可以根据我们屏幕大小缩放我们的界面，这里设置的`Scale With Screen Size`设置缩放。将显示设置为`1k`，这里`Reference Resolution`用于适配分辨率。`Match`选择用那个参考缩放，这里设置为`0.5`就是两者（高度和宽度）都参考缩放。

![image-20231012144945648](image-20231012144945648.png)

添加特效，我们需要的是下雨的特效，但是初始的时候添加发射的是框框：

![image-20231012145059238](image-20231012145059238.png)

这是因为我们没有设置样式，我们需要调整这个特效的样式，先看看我们的需求：

- 需要添加下雨的样式
- 方向改为向下
- 雨滴需要有碰撞效果

![image-20231012150656950](image-20231012150656950.png)

将发射的物体形状改为`Box`这样就会集中向一个方向发射，然后将位置改为`90度`，记得重新设置一下物体的位置，按`reset`

![image-20231012151606522](image-20231012151606522.png)

调整完特效的位置之后，我们需要装上我们的样式

![image-20231012151658906](image-20231012151658906.png)

这里采用函数的形式循环播放我们的动画，可以理解为我们插入的都是一帧一帧的图片，然后调整函数的转化时间就可以做一个动画了：

![image-20231012151813221](image-20231012151813221.png)

然后设置我们的雨滴大小`start size`，调整完雨滴的颜色，这里需要设置一下雨滴的重力来模仿我们的现实世界。然后取消我们雨滴的反弹效果，也就是`Flip Rotation`修改为`0`.

![image-20231012151849330](image-20231012151849330.png)

然后设置碰撞器，让雨滴和地面有碰撞效果

![image-20231012152151764](image-20231012152151764.png)

然后就有动画效果了：

![录制_2023_10_12_15_30_05_568](录制_2023_10_12_15_30_05_568.gif)

然后我们需要对水滴进行增光处理，建一个材质：`Material`

![image-20231012153318588](image-20231012153318588.png)

然后调整颜色和`Alpha Clipping`这个是不会更改原先样式，将这个材质插入到我们的组件当中：

![image-20231012153503333](image-20231012153503333.png)

然后建一个：`Global Volume`控制我们界面的光效，然后配置：

![image-20231012153546333](image-20231012153546333.png)

具体的功能如这个[博客](https://docs.unity3d.com/cn/Packages/com.unity.render-pipelines.universal@12.1/manual/shaders-in-universalrp.html)，然后我们就实现了功能了。

![image-20231012153710502](image-20231012153710502.png)

## 创建画画机制

在新建一个页面作为我们的选择关卡界面（但是目前关卡只有一个）。在`Scence`中创建一个

![image-20231014115649999](image-20231014115649999.png)

然后得到和我们主相机一样的界面，我们需要对其进行一些配置，首先将我们的相机背景颜色调成暗色。

![image-20231014115759967](image-20231014115759967.png)

![image-20231014115732567](image-20231014115732567.png)

我们在这个界面建立一个线条特效，名称是`Line`，这个特效的功能是在我们的界面上绘制线条，在对应的仪表盘可以设置线条的样式。

![image-20231014120001906](image-20231014120001906.png)

![image-20231014120905205](image-20231014120905205.png)

`size`是分为几个段，然后下面的`Width`是调整线段的宽度，然后记得点开世界坐标向量，这个的意思是依据我们摄像机的坐标来绘制我们的线条，也就是这个：

![image-20231014123116684](image-20231014123116684.png)

然后我们需要在我们的主界面，按住鼠标左键绘制图形。我们插入一个`c#`文件：

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class LineManager : MonoBehaviour
{
    //获取Line
    private LineRenderer lineRenderer;
    //存储我们绘制的点
    private List<Vector2> pointList = new List<Vector2>();

    private void Start()
    {
        lineRenderer = GetComponent<LineRenderer>();
    }


    private void Update()
    {
        if (Input.GetMouseButton(0))
        {
            //获取主界面的坐标
            Vector2 position = Camera.main.ScreenToWorldPoint(Input.mousePosition);
            //如果当前的坐标没绘制，就绘制
            if (!pointList.Contains(position))
            {
                pointList.Add(position);
                lineRenderer.positionCount++;
                lineRenderer.SetPosition(lineRenderer.positionCount - 1, position);
            }
        }


        #region 检测按下
        if (Input.GetMouseButton(0))
        {
            Debug.Log("按下");
        }
        if (Input.GetMouseButton(1))
        {
            Debug.Log("抬起");
        }
        #endregion
    }
}
```

逻辑就是，设置一个数组它的值全为0，只要检测到我们绘制的坐标就依据我们的数组来检测它是否绘制即可。然后给我们的线加材质，新建一个材质：

补充材质的选择：`Universal Render Pipeline - > Particles -> unit`

![image-20231014132148652](image-20231014132148652.png)

然后拖入我们线中的材质选项：

![image-20231014132236750](image-20231014132236750.png)

然后画画

![image-20231014132414944](image-20231014132414944.png)

光效是由这个来判断的：[`Bloom`](https://docs.unity3d.com/cn/2018.2/Manual/PostProcessing-Bloom.html) 这个暂时不清楚

![image-20231014133008869](image-20231014133008869.png)

## 创建人物行走

人物是在我们划线结束的时候，前进到目标点的。所以说我们需要界定我们划线的状态：

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class LineManager : MonoBehaviour
{
    //获取Line
    private LineRenderer lineRenderer;
    //存储我们绘制的点
    private List<Vector2> pointList = new List<Vector2>();

    //声明的默认值是什么 - 这里判断我们画完了没 刚开始错误是因为一直判断我们画完了，然后检索数组发现啥都没有
    private bool isRun;

    //获取玩家坐标
    private Transform playerPos;

    //索引
    private int currentIndex = 0;

    [SerializeField]
    private float speed;

    private void Start()
    {
        lineRenderer = GetComponent<LineRenderer>();

        playerPos = GameObject.Find("Player").transform;
        Debug.Log("Player");
    }


    private void Update()
    {
        if (Input.GetMouseButton(0))
        {
            //获取主界面的坐标
            Vector2 position = Camera.main.ScreenToWorldPoint(Input.mousePosition);
            //如果当前的坐标没绘制，就绘制
            if (!pointList.Contains(position))
            {
                pointList.Add(position);
                lineRenderer.positionCount++;
                lineRenderer.SetPosition(lineRenderer.positionCount - 1, position);
            }
        }


        #region 鼠标抬起
        if (Input.GetMouseButton(0))
        {
            isRun = true;
        }
        #endregion

        if (isRun)
        {
            //这个是移动？ - 不懂这个函数
            playerPos.position = Vector3.MoveTowards(playerPos.position, pointList[currentIndex], speed * Time.deltaTime);
            if(Vector3.Distance(playerPos.position, pointList[currentIndex]) < 0.1f)
            {
                currentIndex++;
                if(currentIndex >= pointList.Count)
                {
                    //数组索引是从0开始 所以说最后一个位置是长度-1
                    currentIndex = pointList.Count - 1;
                }
            }
        }
    
    }
}

```

这里的执行逻辑是用`isRun`判断我们画完没，画完了就根据我们的`playerPos.position`位置和数组的位置来移动我们的`player`。实现完的画面：

![image-20231014165249457](image-20231014165249457.png)

## 绘制地图

创建一个`Tilemap`，这个是我们画布的基准，可以理解为在白色的纸上添加格子。

![image-20231015145137798](image-20231015145137798.png)

创建完成也就是这个样子：

![image-20231015144451452](image-20231015144451452.png)

然后添加我们的`sprite`，这里就要新建一个画布，开启位置在`window`处创建.

![image-20231015144547599](image-20231015144547599.png)

然后把精灵拖入进去，就可以对这些素材进行绘制了：

![image-20231015145031291](image-20231015145031291.png)

简单绘制一下我们的地图，如下：

![image-20231015145634822](image-20231015145634822.png)

我们多制作一个绘制表格的物件，将它作为我们的陷阱：`Firmap`

![image-20231015145708214](image-20231015145708214.png)

一个是正常的地图制件，一个是我们预备的陷阱组件，然后对它们增加一个[刚体](https://docs.unity3d.com/cn/2021.1/Manual/class-Rigidbody2D.html)属性，让它有一定的物理属性（这里最主要的作用是让它可以给我们的脚本操控）：

![image-20231015145846911](image-20231015145846911.png)

> 补充：这个组件是给物体指定的物理属性
>
> `Drag` : 这个的意思是阻力，可以赋予它线性的阻力和角阻力
>
> `Mass`：表示的是我们物体的质量
>
> `Gravity`：物体的重力
>
> `Collision` 碰撞

将刚体组件的重力表示为0，或者将之设置为静态状态：

![image-20231015152115826](image-20231015152115826.png)

这三个属性：动态，运动，静态。不懂？

然后设置[瓦片地图碰撞器](https://docs.unity3d.com/cn/2021.1/Manual/class-TilemapCollider2D.html)：

![image-20231015153628747](image-20231015153628747.png)

设置 :`Delaunay Mesh` 这里只会在显示的范围内附加物体这些属性，可以节省性能。然后做完就是这样的效果：

![image-20231015154843997](image-20231015154843997.png)

## 制作人物动画

将我们之前创建的正方形改为我们的动画人物：

![image-20231015155217683](image-20231015155217683.png)

在[`animation`](https://docs.unity3d.com/cn/2021.1/Manual/class-AnimationClip.html)处制作我们的人物动画（小人移动），使用快捷键`ctrl 6` 打开对应的组件，然后将精灵拖入到这里面形成逐帧动画：

注意，在我们的对应的组件按`ctrl 6`才有这个效果，然后拖入我们两帧的图片，记得把时间间隔调为`6`这样就不会很快了（切换频率）

![image-20231015155717184](image-20231015155717184.png)

然后创建`Run`，原理是一样的拖入我们的组件就行。

![image-20231015161447210](image-20231015161447210.png)

然后配置我们的状态机，状态机是用于切换我们动画的状态。我们这里设置了三种状态，`待机`，`Run`和`Jump`。下面的`Has Exit Time`是过度状态，

![image-20231015201133592](image-20231015201133592.png)

在代码中，我们将控制角色行动的操作封装为一个函数：

```c++
private void SetPlayerState()
    {
        switch (state)
        {
            //静止逻辑
            case PlayerState.Idle:
                animator.SetBool(PlayAnimStateTag.IsRun, false);
                break;
            //运动逻辑
            case PlayerState.Run:
                animator.SetBool(PlayAnimStateTag.IsRun, true);
                break;
            //攻击逻辑
            case PlayerState.Attack:
                break;
        }
    }
```

![image-20231015162321487](image-20231015162321487.png)

## 制作金币动画

同样的在`Animation`处我们创建关于金币的动画，老样子做三个：待机，触发，消失。

制作的流程就是：拉入精灵 - > `ctrl 6` -> 拖入需要的动画图片

![image-20231016153750327](image-20231016153750327.png)

![image-20231016153719104](image-20231016153719104.png)

然后给予我们金币碰撞器，让这个金币可以被我们的小人吃掉。顺便将小人也赋予碰撞器和刚体（记得把重力关掉）：

![image-20231016154411502](image-20231016154411502.png)

![image-20231016154423217](image-20231016154423217.png)

这里还要设置一下`Edit Collider`，就是将碰撞体调整到我们需要的大小。然后我们配置一下金币。

![image-20231016154912645](image-20231016154912645.png)

把金币的选项，[`isTrigger`](https://blog.csdn.net/weixin_40378974/article/details/111282646)点起来，这里是忽略一些物理的属性。(不太懂)

![image-20231016155553585](image-20231016155553585.png)

我们使用标签`Tag`来对物品进行标识，给金币一个`coin`.

![image-20231016160650694](image-20231016160650694.png)

在玩家处，我们新建一个`c#`脚本对于碰撞效果编程：

```c++

```

