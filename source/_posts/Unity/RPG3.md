---
title: 3D(RPG)3
date: 2024-02-24 12:19:03
categories: RPG
typora-root-url: ./RPG3
---



> 在前面发现这个教程代码太乱了，这里就做一些标记

## 玩家相机

玩家相机需要使用`CineMachine`插件，这里在`Package Manager`

![image-20240224125403217](./image-20240224125403217-1733659438312-1.png)

然后再`unity`的界面出就会出现`Cinemachine`选项。

![image-20240224125555679](./image-20240224125555679-1733659438313-2.png)

但是创建位置是在`GameObject`中，这里先创建一个虚拟相机

![image-20240224130530740](./image-20240224130530740-1733659438313-3.png)

这样在组件的配置中，我们就可以拖入需要观察的物体，来进行相机跟随了。

![image-20240224130626508](./image-20240224130626508-1733659438313-4.png)

但是直接拖入角色的话，有些场景就不太容易实现。例如，如果要求当前角色不动但是摄像机需要动，那么就得关闭当前摄像机然后创建一个新的。这里我们可以创建一个空物体，然后让这个空物体和我们的角色组成一个组件。让相机跟随这个物体就行。

![image-20240224141048029](./image-20240224141048029-1733659438313-5.png)

然后调整一下摄像头的位置，这里需要注意`Body`的设置，我刚开始没注意就导致移动不了。这里是找的一个[博客](https://blog.csdn.net/zhenghongzhi6/article/details/104340502)的解释。

![image-20240224141153808](./image-20240224141153808-1733659438313-6.png)

## 状态机重构

> 因为前面的状态太抽象了，而且使用了反射，这里需要重构。(基本上是全部^ - ^)

这里贴上设计的思想：

![image-20240224213136176](./image-20240224213136176-1733659438313-9.png)

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

![image-20240224202430557](./image-20240224202430557-1733659438313-7.png)

### 玩家攻击

在之前创建过玩家移动的脚本，这里攻击其实是同样的。配置攻击动画，打开模型类的动画控制器。添加攻击动画

![image-20240225162442992](./image-20240225162442992-1733659438313-8.png)

创建一个`bool`值，用于触发攻击

![image-20240225163331645](./image-20240225163331645-1733659438313-11.png)

> 这里的Has Exit Time是过度时间，取消勾选因为需要在任意时刻都可以切换为攻击模式

因为是模型在攻击，这里在模型脚本中创建一个攻击函数.

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

// 动画 - 武器层 刀光效果
public class Player_Model : MonoBehaviour
{
    private Player_Controller player;
    private Animator animator;


    public void Init(Player_Controller player)
    {
        this.player = player;
        animator = GetComponent<Animator>();
    }


    // 更新移动相关参数
    public void UpdateMovePar(float x,float y)
    {
        animator.SetFloat("左右", y);
        animator.SetFloat("前后", x);

    }
    // 3.攻击函数
    public void Attack()
    {
        // 播放攻击动画
        animator.SetBool("攻击", true);
    }
}
```

然后和移动一样，创建脚本`player_Attack`

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Player_Attack : StateBase<PlayerState>
{
    public Player_Controller player;

    public override void Init(FSMController<PlayerState> controller, PlayerState stateType)
    {
        base.Init(controller, stateType);
        player = controller as Player_Controller;
    }

    public override void OnEnter()
    {
        // 1.攻击
        Attack();
    }

    public override void OnExit()
    {
        
    }

    public override void OnUpdate()
    {
       
    }

    public void Attack()
    {
        player.model.Attack();

    }
}
```

在移动的时候，如果我们按攻击键，那么模型也会攻击，这里就在`Player_Move`中添加一个`if`语句来判断

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
       //  Debug.Log(moveSpeed);
    }
    
    Move(h, v+runTimesition);

    // 4.检测攻击 - 需要注意cd
    if (player.CheckAttack())
    {
        player.ChangeState<Player_Attack>(PlayerState.Player_Attack);
    }

}
```

然后就可以了，但是不会触发动画的回归，这里需要设置一些动画事件

![image-20240226121340584](./image-20240226121340584-1733659438313-10.png)

在动画状态机中，有关于`Events`的选项在这里进行事件的适配

![image-20240226121828373](./image-20240226121828373-1733659438313-24.png)

在动画结束的末尾，添加一个动画结束的事件。

![image-20240226122623934](./image-20240226122623934-1733659438313-12.png)

在代码中添加一个函数用于结束`攻击`动画

![image-20240226124038514](./image-20240226124038514-1733659438313-22.png)

既然攻击，也就是要有攻击的时刻。设置两个事件时刻来检测攻击。

- `StartSkillHit`

![image-20240226125043607](./image-20240226125043607-1733659438313-13.png)

- `StopSkillHit`

![image-20240226125158690](./image-20240226125158690-1733659438313-14.png)

在`Player_Model`脚本中，添加动画事件

![image-20240226125537579](./image-20240226125537579-1733659438313-30.png)

### 武器层和武器拖尾

在人物模型出，为武器添加特效。这里和骨骼有关，但是我没接触过。

![image-20240226130318476](./image-20240226130318476-1733659438313-15.png)

这里调整红线和武器对齐，防止碰撞的时候出现问题

![image-20240226132920014](./image-20240226132920014-1733659438313-16.png)

创建武器的子对象，制作特效。

![image-20240226133710070](./image-20240226133710070-1733659438313-17.png)

然后调整[特效](https://blog.csdn.net/NCZ9_/article/details/84189155)的大小

![image-20240226135127056](./image-20240226135127056-1733659438313-18.png)

刀光特效就出现了

![image-20240226135051471](./image-20240226135051471-1733659438313-19.png)

先创建一个材质

![image-20240226140118057](./image-20240226140118057-1733659438313-20.png)

针对于这个刀光，赋予其一个材质

![image-20240226140157559](./image-20240226140157559-1733659438313-21.png)

然后再特效处加上[特效材质](https://www.cnblogs.com/qitanzhideyu/p/14370300.html)

![image-20240226140056107](./image-20240226140056107-1733659438313-23.png)

在剑的位置构造碰撞体，后面可以制作打击效果

![image-20240226142302502](./image-20240226142302502-1733659438313-25.png)

我们需要挥刀的时候才显示这些特效，而且挥刀的时候才会显示碰撞效果

- `WeaponCollider` 特效碰撞类

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class WeaponCollider : MonoBehaviour
{
    public BoxCollider BoxCollider;
    public TrailRenderer TrailRenderer;


    public void Init()
    {
        StartSkillHit();
        StopSkillHit();
    }

	// 开始挥剑
    public void StartSkillHit()
    {
        BoxCollider.enabled = true;
        TrailRenderer.emitting = true;
    }
	// 结束挥剑
    public void StopSkillHit()
    {
        BoxCollider.enabled = false;
        TrailRenderer.emitting = false;
    }
}
```

- `Init` 初始化，这里是在`Player_Model`中初始化

```c#
// 创建对象
public WeaponCollider WeaponCollider;    

public void Init(Player_Controller player)
    {
        this.player = player;
        animator = GetComponent<Animator>();
        WeaponCollider.Init();
    }
```

- 在`Player_Model`中编辑动画事件，调用`WeaponCollider`方法。这里的`StopSkillHit`和`StopSkillHit`是调用动画事件的"时间戳"

```c#
#region 动画事件
private void StartSkillHit()
{
    // 开启刀光的拖尾
    // 伤害检测触发器
    WeaponCollider.StartSkillHit();
}

private void StopSkillHit()
{
    // 关闭刀光
    // 关闭伤害检测
    WeaponCollider.StopSkillHit();
}

public void StopSkillHit()
{
    animator.SetBool("攻击", false);
    player.ChangeState<Player_Move>(PlayerState.Player_Move);
}

#endregion
```

这里还要挂载一下组件，要不然控制不了。（不过好像可以用子对象搜索名字的方法）

![image-20240226152934151](./image-20240226152934151-1733659438313-26.png)

然后就可以了

![image-20240226153752860](./image-20240226153752860-1733659438313-27.png)

接下来处理碰撞问题，在剑的位置加入刚体。刚体的作用是检测碰撞（不太懂），这里需要对其`XYZ`的位置进行约束，这样就不会乱动了。

![image-20240226154258993](./image-20240226154258993-1733659438313-29.png)

在脚本中配置打击怪物的效果函数，为后续做准备

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class WeaponCollider : MonoBehaviour
{
    public BoxCollider BoxCollider;
    public TrailRenderer TrailRenderer;
    // 怪物列表 - 记录被攻击的的怪物 - 保证只被攻击一次
    private List<GameObject> monsterList;

    public void Init()
    {
        monsterList = new List<GameObject>();
        StopSkillHit();
        
    }

    public void StartSkillHit()
    {
        BoxCollider.enabled = true;
        TrailRenderer.emitting = true;
    }

    public void StopSkillHit()
    {
        BoxCollider.enabled = false;
        TrailRenderer.emitting = false;
        // 清空怪物列表
        monsterList.Clear();
    }

    private void OnTriggerEnter(Collider other)
    {
        // 保证一段伤害只击中一次怪物
        // 对方是怪物，并且此次攻击第一次碰到
        if(other.tag == "Monster" && !monsterList.Contains(other.gameObject))
        {
            // 输出伤害
            monsterList.Add(other.gameObject);
        }
    }

}
```

### 怪物和给怪物一刀

导入怪物素材

![image-20240227112447673](./image-20240227112447673-1733659438313-28.png)

将怪物拉入到场景中，这里和玩家一样将怪物模型独立出来

![image-20240227112627155](./image-20240227112627155-1733659438313-33.png)

给怪物赋予碰撞体，这里适合胶囊体。

![image-20240227112903618](./image-20240227112903618-1733659438313-34.png)

加个`tag`，名称为`monster`。这里是武器攻击的索引

![image-20240227134919497](./image-20240227134919497-1733659438313-31.png)

创建一个动画控制器，塞入到怪物模型的`Animator`中

![image-20240227124503767](./image-20240227124503767-1733659438313-32.png)

打开控制机，我们制作受击-待机的动画状态

![image-20240227124559129](./image-20240227124559129-1733659438314-35.png)

- 左侧创建三个触发器变量，用于控制状态的转移
- 右侧的状态转移将过度事件取消勾选，这里需要受击秒更新

编写脚本，让怪物达到受击效果。这里和玩家一样编写两个脚本

- `Monster_Model`

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Monster_Model : MonoBehaviour
{
    private int currHurtAnimationIndex = 1;
    private Animator animator;

    public void Init()
    {
        animator = GetComponent<Animator>();
    }

    public void PlayerHurtAnimation()
    {
        // 这里是用字符串算的
        animator.SetTrigger("受伤" + currHurtAnimationIndex);
        if (currHurtAnimationIndex == 1) currHurtAnimationIndex = 2;
        else currHurtAnimationIndex = 1;
    }

    public void StopHurtAnimation()
    {
        animator.SetTrigger("受伤结束");
    }    
}
```

- `Monster_Controller`

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Monster_Controller : MonoBehaviour
{
    private Monster_Model model;
    private int hp = 100;

    private void Start()
    {
        model = transform.Find("Model").GetComponent<Monster_Model>(); 
        model.Init();
    }

    public void Hurt()
    {
        //受击动画
        model.PlayerHurtAnimation();
        Invoke("HurtOver", 1);// 1秒硬直事件
        //击退

        //hp down
    }

    private void HurtOver()
    {

    }

}
```

击打效果就实现了，但是没有硬直而且转化生硬

![image-20240227135541374](./image-20240227135541374-1733659438314-36.png)

接着来优化这些问题，顺便制作怪物的击飞.

```c#
public void Hurt()
{
    //受击动画
    model.PlayerHurtAnimation();

    CancelInvoke("HurtOver");
    // 延迟调用
    Invoke("HurtOver", 1);// 1秒硬直事件
    

    //hp down
}
```

击飞的本质是改变物体的位置，所以说这里创建一些变量

- `Monster_Controller`

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Monster_Controller : MonoBehaviour
{
    private Monster_Model model;
    private int hp = 100;
    // 获取当前的碰撞
    private CharacterController characterController;

    // 是否受击
    private bool isHurt;
    // 受击力量
    private Vector3 hurtVelocity;
    // 受击过度
    private float hurtTime;
    // 当前时间
    private float currHurtTime;

    private void Start()
    {
        model = transform.Find("Model").GetComponent<Monster_Model>(); 
        characterController = GetComponent<CharacterController>();
        model.Init();
    }

    private void Update()
    {
        if(isHurt)
        {
            currHurtTime += Time.deltaTime;
            // 用hurtTime的时间 移动了hurtVelocity的距离
            characterController.Move(hurtVelocity * Time.deltaTime / hurtTime);
            if(currHurtTime >= hurtTime) isHurt = false;
        }
        else
        {
            characterController.Move(new Vector3(0,-9f,0) * Time.deltaTime);
        }
    }


    public void Hurt()
    {
        // 先实现功能
        //受击动画
        model.PlayerHurtAnimation();

        CancelInvoke("HurtOver");
        // 延迟调用
        Invoke("HurtOver", 1);// 1秒硬直事件
        

        // 击退 击飞
        isHurt = true;
        hurtVelocity = new Vector3(0,1,1);
        hurtTime = 0.2f;
        currHurtTime = 0;

        // 生命减少
        hp -= 10;
    }

    private void HurtOver()
    {

    }
    
}
```

这里需要注意的是怪物需要添加碰撞体和物理组件。我们是通过这两个来操控物体的

![image-20240227145102482](./image-20240227145102482-1733659438314-37.png)

然后就可以达成击飞了：

![image-20240227145242366](./image-20240227145242366-1733659438314-38.png)

> 总结：这里感觉代码逻辑好乱。但是实现的原理很简单。
>
> - 通过`animator.SetTrigger("")`调用动画机中的变量，来实现动画切换（这里将切换的语句封装成了函数）
>
> - 移动是通过 `Vector3()`转化位置达到想要的效果。
>
>   先弃坑一段时间。
