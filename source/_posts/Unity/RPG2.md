---
title: 3D(RPG)2
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

![image-20240221204631742](./image-20240221204631742-1733659389805-1.png)

拉入课程资源

![image-20240221205704287](./image-20240221205704287-1733659389806-37.png)

![image-20240221205714424](./image-20240221205714424-1733659389805-5.png)

## 玩家输入和音效层

涉及到玩家输入，所以说需要给玩家一个脚本。这里给玩家一个独立的文件夹存储脚本文件。为了命名规范，统一使用Player_作为脚本命名的前缀。我们制作两个脚本

- 玩家的输入
- 音效

这里配置一下玩家输入的时候的坐标，unity中内置了输入的一些参数，我们直接调用其中的参数就行

![image-20240222095832372](./image-20240222095832372-1733659389805-2.png)

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

![image-20240222105232061](./image-20240222105232061-1733659389805-3.png)

将文件和组件配置在Player中

![image-20240222105401864](./image-20240222105401864-1733659389805-6.png)

## 角色移动动画

> 使用动画混合树制作

本来说是想学习动画制作的，但是这里资源已经给好了，我们就使用这个资源就行。在前面我们已经导入资源到unity中了，这里需要对动画资源进行一些修改，下面是打开资源的图片。

![image-20240222114523694](./image-20240222114523694-1733659389805-4.png)

为了节省资源，这里将左右制作成一种动画状态机，我们将之拆分开来，原动画的参数是向左边移动的，这里修改一些参数，让其变的更左

![image-20240222121952964](./image-20240222121952964-1733659389805-7.png)

- 这里修改offset，表示方位偏移量。这里修改为正值表示向右偏移。这里可以稍微修大一些

同样的修改右侧的动画，让它变得更右

![image-20240222122051363](./image-20240222122051363-1733659389805-8.png)

保存为两个动画

![image-20240222122333243](./image-20240222122333243-1733659389805-9.png)

然后制作这个动画控制器，开始使用混合树来制作。先创建一个动画控制器，然后改个名称

![image-20240222124148183](./image-20240222124148183-1733659389805-10.png)

打开这个控制器点击我们的人物物体，右键为其添加一个Blend Tree

![image-20240222124445647](./image-20240222124445647-1733659389805-11.png)

改个名字，改为Move

![image-20240222124507351](./image-20240222124507351-1733659389805-12.png)

打开这个混合树开始制作动画

![image-20240222141155557](./image-20240222141155557-1733659389805-13.png)

在这里面，我们可以添加预设的动画，但是首先得设计一下 Blend Type

![image-20240222141133169](./image-20240222141133169-1733659389805-14.png)

这里几个选项的意思是：

- **Simple Directional（简单方向）**：适用于动画在一个简单方向上进行混合的情况，例如角色的行走动画，根据输入的方向进行过渡。

- **Freeform Directional（自由方向）**：适用于动画在多个方向上进行混合的情况，例如角色的站立转身动画，可以在不同的方向之间自由混合。

- **Freeform Cartesian（自由笛卡尔）**：适用于动画在二维平面上进行混合的情况，比如角色的站立转身和跳跃，可以在平面上的任意位置进行混合。

- **Direct（直接）**：直接使用动画之间的过渡，没有额外的插值或者混合。

这里选用`Freeform Directional`，因为我们要过度多方向移动。

![image-20240222141932320](./image-20240222141932320-1733659389805-17.png)

Parameters 是用来控制动画混合的参数。在` Freeform Directional Blend Type` 中，这些参数通常包括 Direction（方向）和 Speed（速度）。

![image-20240222142204393](./image-20240222142204393-1733659389805-15.png)

然后创建对应的触发变量，这里使用float的变量类型

![image-20240222143340429](./image-20240222143340429-1733659389805-16.png)

![image-20240222143501620](./image-20240222143501620-1733659389805-19.png)

但是现在还是看不出这两个变量的作用是什么，那就拉入动画来测试一下。在右侧点击+，拖入我们素材的动画

![image-20240222144010495](./image-20240222144010495-1733659389805-18.png)

然后拖入动画配置参数之后就是这样子：

![image-20240222145935669](./image-20240222145935669-1733659389805-22.png)

测试之后就可以发现，这里创建的float变量其实就是控制红色圆点的坐标方位。在代码中，获取的的就是竖直和横向的数据，它们的数据范围就是[-1,1]嘛

```c#
public float Horizontal{ get => Input.GetAxis("Horizontal"); }
public float Vertical { get => Input.GetAxis("Vertical"); }
```

控制红点的移动，就可以实现动画的融合。这就是混合树的基本构造了。

## 角色移动状态

![image-20240222192350631](./image-20240222192350631-1733659389806-36.png)

在实现本节课的效果之前，先来搭建场地。先创建一个地板，这样就有走路的地方了

![image-20240222202910864](./image-20240222202910864-1733659389805-20.png)

![image-20240222203012022](./image-20240222203012022-1733659389805-21.png)

接下来给地板赋予材质，简单立即为涂色。先创建一个材质

![image-20240222204259290](./image-20240222204259290-1733659389805-24.png)

创建完成的材质直接拖入到对应需要材质的物体上

![image-20240222204329302](./image-20240222204329302-1733659389805-23.png)

改变一下颜色，我这里填涂上蓝黑色

![image-20240222204334802](./image-20240222204334802-1733659389806-25.png)

这里可以配置一下摄像机的角度问题，按住`ctrl+shift+f`就可以定格你的摄像机视角

![image-20240223135552367](./image-20240223135552367-1733659389806-28.png)

然后开始配置角色的移动，首先创建一个Player_Move脚本，用于控制角色的移动

![image-20240222211143891](./image-20240222211143891-1733659389806-26.png)

这个脚本的作用是角色的移动逻辑，这里就继承我们之前创建的`StaeBase`。然后重写里面的代码

然后给角色加入一个组件，Character Controller是一种用于控制角色移动和碰撞的组件。

![image-20240222211615588](./image-20240222211615588-1733659389806-38.png)

![image-20240222212316610](./image-20240222212316610-1733659389806-27.png)

当然，在设置的时候这个icon有点扰人，这里可以点击右上角的小地球来取消对应的icon

![image-20240222212358183](./image-20240222212358183-1733659389806-29.png)

然后配置这个碰撞体的大小

![image-20240222212644058](./image-20240222212644058-1733659389806-30.png)

然后开始制作角色移动，写入`Player_Move`脚本中

```c#
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Player_Move : StateBase
{
    // 获取挂载
    public Player_Controller player;
    private float moveSpeed = 90;
    //旋转
    private float rotateSpeed = 90;
    // 重写
    public override void Init(FSMController controller, Enum stateType)
    {
        base.Init(controller, stateType);
        // 有限状态机
        player = controller as Player_Controller;
    }
    public override void OnUpdate()
    {
        float h = player.input.Horizontal;
        float v = player.input.Vertical;
        Move(h, v);
    }

    private void Move(float h,float v)
    {
        Vector3 dir = new Vector3(0, 0, v);
        player.characterController.SimpleMove(moveSpeed * dir);
    }

    public override void OnEnter()
    {
        throw new System.NotImplementedException();
    }

    public override void OnExit()
    {
        throw new System.NotImplementedException();
    }  
}
```

这里还需要在`Player_Controller`获取对应的组件，这个脚本是挂载在player上的。

```c#
    private void Start()
    {
        input = new Player_Input();
        audio = new Player_Audio(GetComponent<AudioSource>());  
        // 获取组件
        characterController = GetComponent<CharacterController>();
        //实现移动
        ChangeState(PlayerState.Player_Move);
    }
```

当然为了封装性，`Player_Controller`中的变量也替换为了属性

```c#
using System;
using System.Collections;
using System.Collections.Generic;
using Unity.VisualScripting;
using UnityEngine;

public enum PlayerState
{
    // 移动
    Player_Move,
}

public class Player_Controller : FSMController
{
    private PlayerState playerState;

    // 重写抽象类
    public override Enum CurrentState { get => playerState; set => playerState = (PlayerState)value; }
    // 获取输入
    public Player_Input input { get; private set; }
    public new Player_Audio audio { get; private set; }

    // 获取组件 - 这个是碰撞的
    // 属性的获取必须要比声明其的修饰词封装性高
    public CharacterController characterController { get; private set; }

    private void Start()
    {
        input = new Player_Input();
        audio = new Player_Audio(GetComponent<AudioSource>());  
        // 获取运动状态
        characterController = GetComponent<CharacterController>();
        //实现移动
        ChangeState(PlayerState.Player_Move);
    }

}
```

这里获取输入的`Player_Input`代码是这个：

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

然后测试就可以移动了。

![image-20240222231656073](./image-20240222231656073-1733659389806-31.png)

![image-20240222231707556](./image-20240222231707556-1733659389806-32.png)

但是这种移动效果没有包含动画，而且对应的转角也没有实现对应的移动。接下来就为了解决这个问题。

现在在`Player_Move`，加入有关旋转的代码

```c#
//先实现对应的旋转
    private void Move(float h,float v)
    {
        // dir需要优化
        Vector3 dir = new Vector3(0, 0, v);
        dir = player.transform.TransformDirection(dir);
        player.characterController.SimpleMove(dir*moveSpeed);

        // 旋转
        Vector3 rot = new Vector3(0, h, 0);
        player.transform.Rotate(rot*rotateSpeed* Time.deltaTime);
    }
```

> 补充一下这个`Vector3`这个函数：这个函数是结构体，里面有三个变量x,y,z。可以代表向量，坐标，旋转，缩放。也就是我们调整第二个参数就可以改变当前挂载物体的旋转。

然后就可以实现旋转，但是没有动画。

![image-20240223145017231](./image-20240223145017231-1733659389806-33.png)

为了实现动画，我们给`model`创建一个新的脚本，记录需要添加的东西例如： 动画，武器层，刀光效果

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

// 动画 - 武器层 刀光效果
public class Player_Model : MonoBehaviour
{
    // 这里的controller是为了调名字
    private Player_Controller player;
    private Animator animator;


    public void Init(Player_Controller player)
    {
        this.player = player;
        animator = GetComponent<Animator>();
    }


    // 更新移动相关参数 - 播放动画
    public void UpdateMovePar(float x,float y)
    {
        animator.SetFloat("左右", y);
        animator.SetFloat("前后", x);

    }

}
```

同时在`player_controler`中要初始化这个`model`

```c#
using System;
using System.Collections;
using System.Collections.Generic;
using Unity.VisualScripting;
using UnityEngine;

public enum PlayerState
{
    // 移动
    Player_Move
}

public class Player_Controller : FSMController
{
    // 重写抽象类
    public override Enum CurrentState { get => playerState; set => playerState = (PlayerState)value; } // 这里需要转为枚举需要类型
    private PlayerState playerState;

    // 获取输入 - 这里需要可读不可写
    public Player_Input input { get; private set; }
    // 获取音效
    public new Player_Audio audio { get; private set; }
    // 
    public Player_Model model { get; private set; }

    // 获取组件 - 这个是碰撞的
    // 属性的获取必须要比声明其的修饰词封装性高
    public CharacterController characterController { get; private set; }

    private void Start()
    {
        input = new Player_Input();
        audio = new Player_Audio(GetComponent<AudioSource>());  
        // 获取运动状态
        characterController = GetComponent<CharacterController>();
        // 获取Model对应组件 - 基类中获取脚本是根据名字
        model = transform.Find("Model").GetComponent<Player_Model>();
        model.Init(this);
        //默认移动状态
        ChangeState(PlayerState.Player_Move);
    }

}

```

然后优化一下Move函数

```c#
    private void Move(float h,float v)
    {
        // dir需要优化
        Vector3 dir = new Vector3(0, 0, v);
        // 角色的相对位移 - 角色视角而不是世界视角
        dir = player.transform.TransformDirection(dir);
        player.characterController.SimpleMove(dir * moveSpeed); 

        // 旋转
        Vector3 rot = new Vector3(0, h, 0);
        player.transform.Rotate(rot * rotateSpeed * Time.deltaTime);

        //同步模型动画
        player.model.UpdateMovePar(h, v);

    }
```

然后修改一下速度的值，就可以正常启动了。

![GIF30](https://blog-1324437773.cos.ap-nanjing.myqcloud.com/blog/GIF30.gif)

然后制作一下冲刺。在前面状态树中，我们把冲刺需要的参数调整为2。所以说我们添加一个参数判断我们是否进入冲刺就行

![image-20240223154443270](./image-20240223154443270-1733659389806-34.png)

在移动类中创建一个属性

```c#
 // 判断是否在奔跑
 private bool isRun
 {
     get
     {
         //按动run键加速
         bool temp = player.input.GetRunKey() && player.input.Vertical > 0;
         if (temp) moveSpeed = 5f;
         else moveSpeed = 1f;
         return temp;
     }

 }

```

然后再`update`中加入与这个参数相关的值来控制`movespeed`

```c#
    public override void OnUpdate()
    {
        float h = player.input.Horizontal;
        float v = player.input.Vertical;

        if (v >= 0)
        {   
            // 这里是要加到1 - 才能冲刺
            if (isRun && runTimesition < 1) runTimesition += Time.deltaTime / 2;
            else if (!isRun && runTimesition > 0) runTimesition -= Time.deltaTime / 2;
        }
        else if (runTimesition > 0) runTimesition -= Time.deltaTime / 2;

        if (isRun)
        {
            Debug.Log(moveSpeed);
        }
        // 在v这里加参数就可以控制了
        Move(h, v+runTimesition);
    }
```

然后就可以实现了。

![GIF13](./GIF13-1733659389806-35.gif)
