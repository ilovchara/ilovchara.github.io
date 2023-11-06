---
title: RPG
date: 2023-10-30 22:56:26
tags: 工程
categories: 工程
typora-root-url: ./RPG
---

# RPG

之前犯了一个错误，搞得清零了又得重新做，很烦。

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
