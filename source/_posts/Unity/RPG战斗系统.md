---
title: 3D(RPG)1
date: 2024-02-17 20:10:45
categories: RPG
typora-root-url: ./RPG战斗系统
---



# RPG01

> 跟着`joker`做的第一个`3D`项目

## 有限状态机

![image-20240220232516829](image-20240220232516829.png)

简单理解这个东西就是控制动画播放顺序的。状态机制作玩家AI和NPC的动作。首先先换一下游戏场景名称。

![image-20240220232842314](image-20240220232842314.png)

开始不涉及到美术资源，这里先创建文件夹，然后创建一个状态机基类和一个有限状态机类

![image-20240220233503858](image-20240220233503858.png)

编写状态机脚本

![image-20240220234202169](image-20240220234202169.png)

需要注意的是：

- 这个脚本不需要搭载在别的物体上，所以说不需要继承Mono
- 这个脚本只作为父类继承，所以说使用抽象类

既然是状态机，那肯定会有状态转移，这里就写几个抽象出来的函数（之后在指定的位置给他实现）。主题逻辑是，状态的进入，状态的更新和状态的退出

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
// 抽象类
public abstract class StateBase
{

	// 进入 退出 更新
    public abstract void OnEnter();
    public abstract void OnExit() ;
    public abstract void OnUpdate() ;
}
```

然后通过一个枚举变量来体现当前状态对象表示的状态。

```c#
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public abstract class StateBase
{
    // 当前状态对象的枚举状态
    public Enum StateType;
    // 当前控制的角色
    public FSMController controller;


    // 虚方法 - 首次实例化的初始化
    public virtual void Init(FSMController controller, Enum stateType)
    {
        this.controller = controller;
        this.StateType = stateType;
    }
    
    public abstract void OnEnter();
    public abstract void OnExit() ;
    public abstract void OnUpdate() ;
}
```

接下来来构造抽象状态机的代码，当然这个脚本也应该是抽象的。它需要挂载在指定的物体上，所以说需要继承Mono。这个类可以理解为角色控制机，就是用来控制角色的状态。需要在这个类完成的有

- 记录当前状态
- 根据不同逻辑转移状态 

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
        CurrStateObj.Init(this,newState);

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

- 这里的对象池简单理解：就是创建多个对象，在需要调用的时候将它显示。对象池最简单的方法可以用列表来实现
- 这里创建的方法是为了对象转移而做准备，虽然说还么怎么看懂

总结有点抽象，等后期的我来解释把。
