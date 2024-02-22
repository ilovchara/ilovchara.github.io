---
title: RPG2
date: 2024-02-21 16:01:26
categories: RPG
typora-root-url: ./RPG2
---

# RPG2

> 这一节我们开始导入美术素材，并且完善之前的状态机

接着上节课的内容，对于FSM进行重构。

```c#
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
/// <summary>
/// 有限状态机控制器
/// 这里需要搭载玩家或者怪物
/// </summary>
public abstract class FSMController: MonoBehaviour
{
    // 获取当前的状态
    public abstract Enum CurrentState { get; set; }

    // 当前状态对象 - 获取状态机的对象
    protected StateBase CurrStateObj;

    // 存储全部状态对象 - 对象池
    private List<StateBase>  stateList = new List<StateBase>();

    /// <summary>
    /// 判断状态转移的 - 这里封装为方法
    /// </summary>
    public void ChangeState(Enum newState,bool reCurrState = false)
    {
        // 如果状态一致 - reCurrState是判断是否为同一状态的
        if(newState == CurrentState && !reCurrState) return;
        // 如果状态还存在 - 就结束它（例如播放完成战斗状态之后回到待机状态）
        if(CurrStateObj != null ) { CurrStateObj.OnExit(); }

        // 基于新状态的枚举 - 获取新状态对象
        CurrStateObj = GetStateObj(newState);
        // CurrStateObj.Init(this,newState); - 1.这里修改了发现了错误因为在下面函数中已经初始化了
        CurrStateObj.OnExit();

    }
    /// <summary>
    /// 获取对象池的状态对象
    /// </summary>
    /// <param name="stateType"></param>
    /// <returns></returns>
    private StateBase GetStateObj(Enum stateType)
    {

        // 查找对象池 - Count获取数量
        for(int i = 0; i < stateList.Count; i++)
        {
            if(stateList[i].StateType == stateType){
                return stateList[i];
            }
        }
        // 说明库里没有对象 - 需要实例化对象
        StateBase state = Activator.CreateInstance(Type.GetType(stateType.ToString())) as StateBase;
        // 初始化
        state.Init(this,stateType);
        // 拉入
        stateList.Add(state);
        return state;
    }

}
```

需要在这里添加一个`Update`函数，因为是挂载在物体上的嘛，由于需要让子类可以重写这个`Update`方法，所以说声明其为虚方法

```c#
    protected virtual void Update()
    {
        // 调用这个Update就重写就行
        if (CurrStateObj != null) CurrStateObj.OnUpdate();
    }
```

## 玩家结构

这节课就要构造玩家角色的结构了，还有导入美术素材

![image-20240221204631742](./../../RPG2/image-20240221204631742.png)

拉入课程资源

![image-20240221205704287](./../../RPG2/image-20240221205704287.png)

![image-20240221205714424](./../../RPG2/image-20240221205714424.png)

## 玩家输入和音效层

涉及到玩家输入，所以说需要给玩家一个脚本。这里给玩家一个独立的文件夹存储脚本文件。为了命名规范，统一使用Player_作为脚本命名的前缀。我们制作两个脚本

- 玩家的输入
- 音效

这里配置一下玩家输入的时候的坐标，unity中内置了输入的一些参数，我们直接调用其中的参数就行

![image-20240222095832372](./../../RPG2/image-20240222095832372.png)

下面是具体代码：

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngineInternal;

// 根据结构图 - 我们这里只是一个单纯的类，用于实现在controler中的方法
public class Player_Input
{
    private KeyCode runKeyCode = KeyCode.LeftShift;
    private KeyCode attackKeyCode = KeyCode.J;

    //获取数轴
    public float Horizontal{ get => Input.GetAxis("Horizontal"); }
    public float Vertical { get => Input.GetAxis("Vertical"); }

    // 按键持续按下的状态
    public bool GetKey(KeyCode key)
    {
        return Input.GetKey(key);
    }
    // 按键按下瞬间
    public bool GetKeyDown(KeyCode key)
    {
        return Input.GetKeyDown(key);
    }
    // 获取持续按下run键
    public bool GetRunKey()
    {
        return GetKey(runKeyCode);
    }
    // 获取持续按下attack键
    public bool GetAttackKey()
    {
        return GetKeyDown(attackKeyCode);
    }

}

```

> `GetAxis` 是一个用于获取输入设备轴向（如键盘、鼠标、手柄等）输入的函数。它返回一个介于 -1.0 到 1.0 之间的浮点数，表示输入设备轴向的当前状态。

接下来来制作Audio的功能，目的是获取需要播放的音乐。

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Player_Audio
{
    private AudioSource audioSource;

    public Player_Audio(AudioSource audioSource)
    {
        this.audioSource = audioSource;
    }

    // 播放指定的音效
    public void PlayAudio(AudioClip audioClip)
    {
        audioSource.PlayOneShot(audioClip);
    }
}
```

然后为了让代码看起来不那么抽象，这里实现一下Controller类，在上面的结构图中Controller类继承于FSM这个控制器类。

```c#
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Player_Controller : FSMController
{
    public override Enum CurrentState { get => throw new NotImplementedException(); set => throw new NotImplementedException(); }
    // 调用结构图中的类，声明对象
    private Player_Input input;
    private new Player_Audio audio;


    private void Start()
    {
        input = new Player_Input();
        audio = new Player_Audio(GetComponent<AudioSource>());  
    }

}
```

在unity中，设计一下结构

![image-20240222105232061](./../../RPG2/image-20240222105232061.png)

将文件和组件配置在Player中

![image-20240222105401864](./../../RPG2/image-20240222105401864.png)

## 角色移动动画

> 使用动画混合树制作

本来说是想学习动画制作的，但是这里资源已经给好了，我们就使用这个资源就行。在前面我们已经导入资源到unity中了，这里需要对动画资源进行一些修改，下面是打开资源的图片。

![image-20240222114523694](./../../RPG2/image-20240222114523694.png)

为了节省资源，这里将左右制作成一种动画状态机，我们将之拆分开来，原动画的参数是向左边移动的，这里修改一些参数，让其变的更左

![image-20240222121952964](./../../RPG2/image-20240222121952964.png)

- 这里修改offset，表示方位偏移量。这里修改为正值表示向右偏移。这里可以稍微修大一些

同样的修改右侧的动画，让它变得更右

![image-20240222122051363](./../../RPG2/image-20240222122051363.png)

保存为两个动画

![image-20240222122333243](./../../RPG2/image-20240222122333243.png)

然后制作这个动画控制器，开始使用混合树来制作。先创建一个动画控制器，然后改个名称

![image-20240222124148183](./../../RPG2/image-20240222124148183.png)

打开这个控制器点击我们的人物物体，右键为其添加一个Blend Tree

![image-20240222124445647](./../../RPG2/image-20240222124445647.png)

改个名字，改为Move

![image-20240222124507351](./../../RPG2/image-20240222124507351.png)

打开这个混合树开始制作动画

![image-20240222141155557](./../../RPG2/image-20240222141155557.png)

在这里面，我们可以添加预设的动画，但是首先得设计一下 Blend Type

![image-20240222141133169](./../../RPG2/image-20240222141133169.png)

这里几个选项的意思是：

- **Simple Directional（简单方向）**：适用于动画在一个简单方向上进行混合的情况，例如角色的行走动画，根据输入的方向进行过渡。

- **Freeform Directional（自由方向）**：适用于动画在多个方向上进行混合的情况，例如角色的站立转身动画，可以在不同的方向之间自由混合。

- **Freeform Cartesian（自由笛卡尔）**：适用于动画在二维平面上进行混合的情况，比如角色的站立转身和跳跃，可以在平面上的任意位置进行混合。

- **Direct（直接）**：直接使用动画之间的过渡，没有额外的插值或者混合。

这里选用`Freeform Directional`，因为我们要过度多方向移动。

![image-20240222141932320](./../../RPG2/image-20240222141932320.png)

Parameters 是用来控制动画混合的参数。在 Freeform Directional Blend Type 中，这些参数通常包括 Direction（方向）和 Speed（速度）。

![image-20240222142204393](./../../RPG2/image-20240222142204393.png)

然后创建对应的触发变量，这里使用float的变量类型

![image-20240222143340429](./../../RPG2/image-20240222143340429.png)

![image-20240222143501620](./../../RPG2/image-20240222143501620.png)

但是现在还是看不出这两个变量的作用是什么，那就拉入动画来测试一下。在右侧点击+，拖入我们素材的动画

![image-20240222144010495](./../../RPG2/image-20240222144010495.png)

然后拖入动画配置参数之后就是这样子：

![image-20240222145935669](./../../RPG2/image-20240222145935669.png)

测试之后就可以发现，这里创建的float变量其实就是控制红色圆点的坐标方位。在代码中，获取的的就是竖直和横向的数据，它们的数据范围就是[-1,1]嘛

```c#
public float Horizontal{ get => Input.GetAxis("Horizontal"); }
public float Vertical { get => Input.GetAxis("Vertical"); }
```

控制红点的移动，就可以实现动画的融合。这就是混合树的基本构造了。

## 角色移动状态
