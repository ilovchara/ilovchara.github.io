---
title: RPG3
date: 2024-02-24 12:19:03
categories: RPG
typora-root-url: ./RPG3
---



> 在前面发现这个教程代码太乱了，这里就做一些标记

## 玩家相机

玩家相机需要使用`CineMachine`插件，这里在`Package Manager`

![image-20240224125403217](./../../RPG3/image-20240224125403217.png)

然后再`unity`的界面出就会出现`Cinemachine`选项。

![image-20240224125555679](./../../RPG3/image-20240224125555679.png)

但是创建位置是在GameObject中，这里先创建一个虚拟相机

![image-20240224130530740](./../../RPG3/image-20240224130530740.png)

这样在组件的配置中，我们就可以拖入需要观察的物体，来进行相机跟随了。

![image-20240224130626508](./../../RPG3/image-20240224130626508.png)

但是直接拖入角色的话，有些场景就不太容易实现。例如，如果要求当前角色不动但是摄像机需要动，那么就得关闭当前摄像机然后创建一个新的。这里我们可以创建一个空物体，然后让这个空物体和我们的角色组成一个组件。让相机跟随这个物体就行。

![image-20240224141048029](./../../RPG3/image-20240224141048029.png)

然后调整一下摄像头的位置，这里需要注意`Body`的设置，我刚开始没注意就导致移动不了。这里是找的一个[博客](https://blog.csdn.net/zhenghongzhi6/article/details/104340502)的解释。

![image-20240224141153808](./../../RPG3/image-20240224141153808.png)

## 状态机重构

> 因为前面的状态太抽象了，而且使用了反射，这里需要重构。(基本上是全部^ - ^)

这里贴上设计的思想：

![image-20240224213136176](./../../RPG3/image-20240224213136176.png)

唉，这里就开始吧。首先需要重构的是我们的`FSM`。

我们给`FSM`添加一个泛型参数T，这个T相当于占位符，代替我们需要的类型。

```c#
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
/// <summary>
/// 有限状态机控制器
/// 这里需要搭载玩家或者怪物
/// </summary>
public abstract class FSMController<T>: MonoBehaviour
{
    // 获取当前的状态
    public T CurrentState;

    // 当前状态对象 - 获取状态机的对象
    protected StateBase<T> CurrStateObj;

    // 存储全部状态对象 - 对象池 - 改为字典
    private Dictionary<T,StateBase<T>>  stateDic = new Dictionary<T, StateBase<T>>();

    /// <summary>
    /// 判断状态转移的 - 这里封装为方法
    /// </summary>
    public void ChangeState<K>(T newState,bool reCurrState = false) where K : StateBase<T>, new()
    {
        // 如果状态一致 - 同时不需要刷新状态 则什么都不用做
        if(newState.Equals(CurrentState) && !reCurrState) return;
        // 如果状态还存在 - 就结束它（例如播放完成战斗状态之后回到待机状态）
        if(CurrStateObj != null ) { CurrStateObj.OnExit(); }

        // 基于新状态的枚举 - 获取新状态对象
        CurrStateObj = GetStateObj<K>(newState);
        // CurrStateObj.Init(this,newState); - 这里修改了发现了错误
        CurrStateObj.OnEnter();

    }
    /// <summary>
    /// 获取对象池的状态对象
    /// </summary>
    /// <param name="stateType"></param>
    /// <returns></returns>
    private StateBase<T> GetStateObj<K>(T stateType) where K : StateBase<T>,new()
    {

        // 对象池改为字典
        if (stateDic.ContainsKey(stateType)) return stateDic[stateType];

        // 这里取消了反射 - 创建statebase实例
        StateBase<T> state = new K();
        // 初始化
        state.Init(this,stateType);
        // 拉入
        stateDic.Add(stateType,state);
        return state;
    }

    protected virtual void Update()
    {
        // 调用这个Update就重写就行
        if (CurrStateObj != null) CurrStateObj.OnUpdate();
    }

}
```

每一个带T的位置，我们可以指定我们需要的数据类型。例如：原本的`public T CurrentState`的T其实是`Enum`类型，这里修改为泛指。这里可能不太明显，因为这个修改为泛型其实是避免进行引用比较，下面的例子比较准确。

```c#
// 这里的T表示状态，可以填入PlayerState
// 如果是敌人，可以填入EnemyState
protected StateBase<T> CurrStateObj;
```

当然，还有一个泛型参数K，这个K的作用是为了建立约束。

```c#
// 让这个K必须是StateBase<T>类或者它的子类的对象
// 因为K约束只能创建有关于statebase的对象或者其子类的对象
(T stateType)where K : StateBase<T>,new()
```

然后就到，`Player_Controller`，虽然说这里暂时没有用到继承的泛型类

```c#
using System;
using System.Collections;
using System.Collections.Generic;
using Unity.VisualScripting;
using UnityEngine;

/// <summary>
/// 1. 这里采用了enum枚举角色的状态
/// </summary>
public enum PlayerState
{
    // 默认
    Player_None,
    // 移动
    Player_Move,
    // 攻击
    Player_Attack
}

public class Player_Controller : FSMController<PlayerState>
{
    // 获取输入 - 这里需要可读不可写
    public Player_Input input { get; private set; }
    // 获取音效
    public new Player_Audio audio { get; private set; }
    
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
        // 获取Model对应组件
        model = transform.Find("Model").GetComponent<Player_Model>();
        model.Init(this);
        //默认移动状态
        ChangeState<Player_Move>(PlayerState.Player_Move);
    }

}
```

然后就到`StateBase`，这里是指定状态然后初始化的类

```c#
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public abstract class StateBase<T>
{
    // 当前状态对象的枚举状态
    public T StateType;
    // 当前控制的角色
    //public FSMController controller; // 这里没有用到删掉


    // 虚方法 - 首次实例化的初始化
    public virtual void Init(FSMController<T> controller, T stateType)
    {
        this.StateType = stateType;
    }
    
    public abstract void OnEnter();
    public abstract void OnExit() ;
    public abstract void OnUpdate() ;
}
```

## 技能系统设计

![image-20240224202430557](./../../RPG3/image-20240224202430557.png)
