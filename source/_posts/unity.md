---
title: unity - 拾荒者
date: 2023-08-24 11:11:19
tags: 工程
categories: 工程
swiper_index: 5
typora-root-url: ./unity
---

# 拾荒者

在做过坦克大战之后，简单地熟悉了`unity`这个软件的功能，但是还是不明白一些动画的制作和脚本函数的命令，这些东西太多太杂。在这之前已经做了一半这个拾荒者这个项目了，但是感觉没总结，感觉没学。所以说打算重新做这个，然后记录一下笔记。因为是`2d`的，所以说比较简单，下面就具体的实现一下吧。

## 前期准备

也不需要什么准备，只需要获取相应的游戏素材即可，对应的`unity`操作需要一步一步做。比较重要的就是记住几个文件夹的作用：

![image-20230911195121591](image-20230911195121591.png)

这几个文件夹分别是：`动画`，`音效`，`字体`，`预制件（生成的物体）`,`场景`，`脚本代码`,`素材`。

在存储物品的时候一定要理清顺序，排列清楚。

## 制作主角

先切割我们的画布，也就是我们的地图素材。在`unity2d`中，所有的物品之类的东西都是一个画，可以这样子理解。

点击我们的素材

![image-20230911195903112](image-20230911195903112.png)

![image-20230911195948615](image-20230911195948615.png)

右边就会出现对应的参数，点击`sprite editor`就可以对图片预览切割，我这里已经切割好了，可以查看预览效果:
![image-20230911200422551](image-20230911200422551.png)

这就是我们接下来要使用的图片素材。构造主角就是绿色衣服的这个小伙子，我们需要给他个物体。

只需要按住`shift`,框选连续的图片拉入到我们的场景框也就是这个：

![image-20230911200820039](image-20230911200820039.png)

就可以创建对应的物体了，创建完成之后就是这样子：一个小人孤零零的在摄像机前面

![image-20230911200933120](image-20230911200933120.png)

> 这里补充一下，我们游戏的画面是在摄像机这里的，我们只能看到摄像机所涉及到的地方，别的地方我们是看不到的(`2D`)

然后，我们会发现伴随着小人创建的时候，也创建了两个文件：![image-20230911201140547](image-20230911201140547.png)

我这里已经归纳完毕了：![image-20230911201206569](image-20230911201206569.png)

![image-20230911201229909](image-20230911201229909.png)

按照中文的翻译，一个是动画控制器(Animator)，一个是动画(Animation)

> Animator：动画控制器，控制`Mecanim`动画系统的接口，用来管理多个动画；
>
> Animation：用于播放动画，老版中单独的一个`Animation`也可以完成动画的播放和切换，不过状态切换之类的需要程序猿代码控制。在新版中，状态管理部分交给了`Animator`；

控制器的画面展开如下：

![image-20230911202925253](image-20230911202925253.png)

动画的画面展开如下：

![image-20230911203125549](image-20230911203125549.png)

> 这里的动画很好理解：几张图片凑在一起，然后以一定的时间间隔进行播放，这就完成了动画的制作。![录制_2023_09_11_20_59_43_471](录制_2023_09_11_20_59_43_471.gif)
>
> 然后对应的，可以设置动画播放的速度也即是`张数/时间`

--------

在控制器中，我们需要修改角色的动画播放速度：

![image-20230911210603835](image-20230911210603835.png)

> 当然可以对文件改名，让自己和别人知道这个动画是干啥的

对于控制器的每一个框框，都对应着一种动画，例如这个`PlayerDamage`就是控制角色受伤的动画，控制器和动画的制作前面已经提到了，如果是我们需要在同一个物体创建不同的动画，只需要将符合动作的图片拉入到我们这个物体即可，这样只会产生动画文件而不会产生多余的动画控制器。做好的结果我们可以在`game`窗口中观看。下面是演示的`gif`

![录制_2023_09_11_21_17_58_350](录制_2023_09_11_21_17_58_350.gif)

![录制_2023_09_11_21_13_52_450](录制_2023_09_11_21_13_52_450.gif)

在对动画修缮之后，我们可以将在场景中的物体拖入下来，存储在一个文件夹中，方便我们之后生成物体。这下面就是我创建设置好动画的一些物体，具体操作看`gif`

![image-20230911212455592](image-20230911212455592.png)

![录制_2023_09_11_21_26_56_782](录制_2023_09_11_21_26_56_782.gif)

这样我们的主角就基本创建完成啦。

## 制作敌人

制作敌人其实是和主角一样的：![录制_2023_09_11_21_32_55_337](录制_2023_09_11_21_32_55_337.gif)

也同样会出现两个文件：![image-20230911213354815](image-20230911213354815.png)

同样的，我们把它们整理起来，然后打开状态机修改名字：

![image-20230911213540002](image-20230911213540002.png)

![录制_2023_09_11_21_36_45_427](录制_2023_09_11_21_36_45_427.gif)

这就是敌人的动画了(可能有点扁)，创建完成敌人1，我们可以如法炮制敌人2。但是这样有点无趣且有点傻。通过观察，我们发现敌人1和敌人2的状态机的逻辑是一样的，只是他们的画是不一样的，这样我们就可以使用敌人1的状态机来实现敌人2的动画。具体步骤如下：

![录制_2023_09_11_21_44_00_781](录制_2023_09_11_21_44_00_781.gif)

我们只需要创建一个`Animator Override Controller`来重写我们的敌人1的状态机把它变为敌人2的。可以在`gif`中看到，将我们敌人1的状态机拖入到重写的控制机中，就可以继承我们的敌人1的状态。但是这个时候，动画的图片还是敌人1的，我们还是需要去修改图片。

在前面我们知道，可以将动画拖入物体的时候不会产生新的状态机，只会产生动画，我们将敌人2的图片拉入到我们的敌人1这个创建好的物品，这样会产生一个敌人2的动画。

![录制_2023_09_11_21_50_45_598](录制_2023_09_11_21_50_45_598.gif)

在此之前，请确保我们已经将敌人1创建成为了预制件（记得每次创建物品并且设置完成动画之后都需要保存）。我们将敌人2动画拉入了敌人1的物体，然后保存我们敌人二的动画，就可以物品的敌人二的动画删除了

然后再重写的状态机上的动画区块加上敌人二的动画就行:

![image-20230911220939830](image-20230911220939830.png)

记得要在敌人2的物体上加入我们的状态机：这里图片会不一样，这个我倒是不知道

![image-20230911221305660](image-20230911221305660.png)

下面是具体操作：

![录制_2023_09_11_22_00_49_661](录制_2023_09_11_22_00_49_661.gif)

下面展示一下做好的两个敌人：

![录制_2023_09_11_22_20_37_365](录制_2023_09_11_22_20_37_365.gif)

这样敌人就做完了。

> 注：如果逻辑是一样的，我们就可以使用同样的状态机重写进行创建角色
>
> 别人的[小寄巧](https://blog.csdn.net/qq_36374904/article/details/118109737)：
>
> **同一套（即动画剪辑之间的关系一致）动画控制器。举个例子，有两个角色，他们都有走路、攻击、正在站立的状态，每个对应一个动画剪辑，他们之间的转换条件都是一样的，但是动画剪辑不一样，我们通常的做法是，`ctrl+D`再拷贝一份动画控制器**
>
> ![img](20210622162013182.png)
>
> 然后逐一替换里面的动画剪辑
>
> 这是我们通常用的一个技巧
>
> 现在我们来介绍另一种方法，可以保留对应的关系。
>
> 首先选择原动画控制器，右键`creat`找到`override`动画控制器
>
> ![img](watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2Mzc0OTA0,size_16,color_FFFFFF,t_70.png)
>
> **创建之后，我们可以看到，他们的图标是不一样的**
>
> ![img](2021062216284812.png)
>
> 此时点击`Enemy_Type3`
>
> ![img](watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2Mzc0OTA0,size_16,color_FFFFFF,t_70-1694491553025-3.png)
>
> **为其指定要重写的动画控制器**
>
> ![img](watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2Mzc0OTA0,size_16,color_FFFFFF,t_70-1694491553025-4.png)
>
> **为其一一替换就行了，操作比之前方便。**
>
> **如果需要修改关系的话，只需要修改最初的一个就可以了，省事很多。**
>
> PS:就是我们上面的步骤

## 创建障碍物和地图

要使得障碍物和地板可以随机生成，我们就需要制作这几个物体。和前面一样，我们只需要将我们切割的图片素材拉入界面即可，下面是我做好的物体资源。

![image-20230912121901248](image-20230912121901248.png)

单单拉取一张图片，只会生成物体不会生成动画，这点还是很智能的。

对于地图的生成，我们需要创建一个物体起到管理我们地图界面的作用，这里我已经创建好了，然后解释一下：

![image-20230912122641851](image-20230912122641851.png)

`Script`是我们创建的脚本文件，脚本文件一般用`c#`来编写（安全且高效）,在脚本文件我们可以看到许多的变量，这都是我们之后需要添加的物体属性，现在我们先编写这个地图的脚本程序。

![image-20230912123358113](image-20230912123358113.png)

> 这里记得设置unity的默认编译器为我们的`vs`要不然会出现代码补全失效的情况，很痛苦的。点击`edit`然后跳转到下图的`Preferences`
>
> ![image-20230912123959932](image-20230912123959932.png)
>
> 修改为你的`vs`即可。
>
> ![image-20230912124045673](image-20230912124045673.png)

在编写之前，先看一眼我们地图样式应该长什么样：

![image-20230912123650662](image-20230912123650662.png)

大概就是这个样子，地板，障碍物，边界，物品和敌人。这张地图我们可以用一个二维数组实现，然后用坐标定位每一个物品的位置。下面是创建完成`Map_Manager`类的样例

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Map_Manager : MonoBehaviour
{
    //游戏内物品
    public GameObject[] outWallArray;
    public GameObject[] floorArray;
    public GameObject[] wallArray;
    public GameObject[] foodArray;
    public GameObject[] enemyArray;
    public GameObject exitPrefab;

    public int rows = 10;
    public int cols = 10;

    public int minCountWall = 2;
    public int maxCountWall = 8;

    private Transform mapHolder;
    private List<Vector2> positionList = new List<Vector2>();

    //关卡变量
    private Game_Manager gameManager;



    // Start is called before the first frame update
    void Awake()
    {
        gameManager = this.GetComponent<Game_Manager>();
        InitMap();
    }

    // Update is called once per frame
    void Update()
    {
        
    }

    //初始化地图
    private void InitMap()
    {

        mapHolder = new GameObject("Map").transform;
        //创建外墙 - 和  - 地板
        for(int x = 0; x < cols; x++)
        {
            for(int y = 0; y < rows; y++)
            {
                if(x == 0 || y == 0 || x == cols-1 || y == rows - 1)
                {
                    int index = Random.Range(0, outWallArray.Length);
                    //打包
                    GameObject go0 =  GameObject.Instantiate(outWallArray[index],new Vector3(x,y,0),Quaternion.identity);
                    go0.transform.SetParent(mapHolder);
                }
                else
                {
                    int index = Random.Range(0, outWallArray.Length);
                    //打包
                    GameObject go0 =  GameObject.Instantiate(floorArray[index], new Vector3(x, y, 0), Quaternion.identity);
                    go0.transform.SetParent(mapHolder);
                }
            }
        }

        //创建障碍物 - 
        positionList.Clear();
        for(int x = 2; x < cols - 2; x++)
        {
            for(int y = 2; y < rows - 2; y++)
            {
                positionList.Add(new Vector2(x, y));
            }
        }

        //创建障碍物 食物 敌人
        //创建障碍物
        int wallCount = Random.Range(minCountWall, maxCountWall+1); //障碍物个数
        InstantiateItems(wallCount, wallArray);

        //创建食物 2 - level*2
        int foodCount = Random.Range(2, gameManager.level * 2 + 1);
        InstantiateItems(foodCount, foodArray);

        //创建敌人 // level/2 - 生成人数敌人/2
        int enemyCount = gameManager.level/2;
        InstantiateItems(enemyCount,enemyArray);
        //创建出口
        GameObject go4 =  Instantiate(exitPrefab, new Vector2(cols - 2, rows - 2), Quaternion.identity) as GameObject;
        go4.transform.SetParent(mapHolder);


    }


    //生成物品函数
    private void InstantiateItems(int count, GameObject[] prefabs)
    {
        for (int i = 0; i < count; i++)
        {
            //随机取得位置
            Vector2 pos = RandomPosition();
            //声明的变量 - ？
            GameObject enemyPrefab = RandomPrefab(prefabs);
            GameObject go = Instantiate(enemyPrefab, pos, Quaternion.identity) as GameObject;
            go.transform.SetParent(mapHolder);
        }


    }



    //取得位置
    private Vector2 RandomPosition()
    {
        int positionIndex = Random.Range(0, positionList.Count);
        Vector2 pos = positionList[positionIndex];
        positionList.RemoveAt(positionIndex);
        return pos;
    }
    //
    private GameObject RandomPrefab(GameObject[] prefabs)
    {
        int index = Random.Range(0, prefabs.Length);
        return prefabs[index];

    }




}

```

在这个`c#`文件中创建的变量，会显示在`unity`的界面之上。像下面添加完成的物体数组，我们可以将`unity`中的资源拖入到其中。

```c#
	public GameObject[] outWallArray;
    public GameObject[] floorArray;
    public GameObject[] wallArray;
    public GameObject[] foodArray;
    public GameObject[] enemyArray;
    public GameObject exitPrefab;
```

![image-20230912124851770](image-20230912124851770.png)

具体流程是这样的：

![录制_2023_09_12_12_50_05_538](录制_2023_09_12_12_50_05_538.gif)

---------

在解释前，先介绍一下脚本的构造

```c++
//地图制作
using System.Collections; // 集合类
using System.Collections.Generic;//用于使用C#的泛型集合类
using UnityEngine;

public class Map_Manager : MonoBehaviour
{
    private void Start()
    {
        // 在 Start 方法中可以初始化地图，加载资源等
    }

    private void Update()
    {
        // 在 Update 方法中可以处理地图的更新逻辑
    }
    
    
}
```

我们现在的目的是要构成一个地图，所用的地图资源我们已经做成了物体了，接下来需要我们创建两个数组用于存储地图的资源：

```c++
//地图制作
using System.Collections; // 集合类
using System.Collections.Generic;//用于使用C#的泛型集合类
using UnityEngine;

public class Map_Manager : MonoBehaviour
{
    //物体数组，可以在unity中拖入物体插入数据
    public GameObject[] outWallArray;
    public GameObject[] floorArray;
    
    private void Start()
    {
        // 在 Start 方法中可以初始化地图，加载资源等
    }

    private void Update()
    {
        // 在 Update 方法中可以处理地图的更新逻辑
    }
    
    
}
```

![录制_2023_09_12_12_50_05_538](录制_2023_09_12_12_50_05_538.gif)

然后我们需要存储地板，敌人，食物，边界障碍物，那就多开几个数组就行。

```c++
//地图制作
using System.Collections; // 集合类
using System.Collections.Generic;//用于使用C#的泛型集合类
using UnityEngine;

public class Map_Manager : MonoBehaviour
{
    //物体数组，可以在unity中拖入物体插入数据
    public GameObject[] outWallArray;
    public GameObject[] floorArray;
    public GameObject[] wallArray;
    public GameObject[] foodArray;
    public GameObject[] enemyArray;
    
    /*
    private void Start()
    {
        // 在 Start 方法中可以初始化地图，加载资源等
    }*/

    //
    void Awake()
    {

    }

    
    private void Update()
    {
        // 在 Update 方法中可以处理地图的更新逻辑
    }
    
    
}
```

> 1. `Awake` 函数：
>    - `Awake` 是在对象实例被创建时调用的，即在 `Start` 函数之前。
>    - `Awake` 用于对象的初始化，通常用于设置变量、获取引用和执行一些早期的初始化操作。
>    - `Awake` 在脚本附加的对象被实例化时立即调用，即使对象处于非活动状态（被禁用），也会调用。
> 2. `Start` 函数：
>    - `Start` 在对象的 `Awake` 被调用后，以及在对象首次被启用时调用。
>    - `Start` 通常用于在对象启动后执行初始化操作，例如设置初始状态、启动协程、访问其他对象等。
>    - 如果一个对象在游戏启动后才被激活，那么 `Start` 会在激活后被立即调用。

然后就是利用数组的资源构造地图，也就是上面的一大串代码

```c++
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Map_Manager : MonoBehaviour
{
    //游戏内物品
    public GameObject[] outWallArray;
    public GameObject[] floorArray;
    public GameObject[] wallArray;
    public GameObject[] foodArray;
    public GameObject[] enemyArray;
    public GameObject exitPrefab;

    //地图的范围
    public int rows = 10;
    public int cols = 10;

    //生成障碍物的最小 - 最大
    public int minCountWall = 2;
    public int maxCountWall = 8;

    //存储位置的信息
    private Transform mapHolder;
    //存储生成的物品 - 存储障碍物
    private List<Vector2> positionList = new List<Vector2>();

    //关卡变量 - 这里是决定物资刷新的
    private Game_Manager gameManager;



    // Start is called before the first frame update
    void Awake()
    {
        gameManager = this.GetComponent<Game_Manager>();
        InitMap();
    }

    // Update is called once per frame
    void Update()
    {
        
    }

    //初始化地图
    private void InitMap()
    {
        //创建一个父对象 - 用于承担整个地图
        mapHolder = new GameObject("Map").transform;
        //创建外墙 10*10
        for(int x = 0; x < cols; x++)
        {
            for(int y = 0; y < rows; y++)
            {
                //如果在边界就生成我们的外墙
                if(x == 0 || y == 0 || x == cols-1 || y == rows - 1)
                {
                    //随机在外面的外墙数组中找到一个墙按上去
                    int index = Random.Range(0, outWallArray.Length);
                    //创建子对象用于维护外墙
                    GameObject go0 =  GameObject.Instantiate(outWallArray[index],new Vector3(x,y,0),Quaternion.identity);
                    //将子对象与父对象关联
                    go0.transform.SetParent(mapHolder);
                }
                //不在边界就生成我们的地板
                else
                {
                    //随机安地板
                    int index = Random.Range(0, outWallArray.Length);
                    //创建子对象维护地板
                    GameObject go0 =  GameObject.Instantiate(floorArray[index], new Vector3(x, y, 0), Quaternion.identity);
                    //承接到我们的父对象上
                    go0.transform.SetParent(mapHolder);
                }
            }
        }

        //因为创建完成地图之后，数组存在值 - 复用清零
        positionList.Clear();
        for(int x = 2; x < cols - 2; x++)
        {
            for(int y = 2; y < rows - 2; y++)
            {
                //随机添加一个位置
                positionList.Add(new Vector2(x, y));
            }
        }

        //下面的升级了

        //随机生成障碍物
        int wallCount = Random.Range(minCountWall, maxCountWall+1);
        InstantiateItems(wallCount, wallArray);

        //随机生成食物 - 按照关卡的标号增加
        int foodCount = Random.Range(2, gameManager.level * 2 + 1);
        InstantiateItems(foodCount, foodArray);

        //创建敌人 // level/2 - 生成人数敌人/2
        int enemyCount = gameManager.level/2;
        InstantiateItems(enemyCount,enemyArray);
        //创建出口
        GameObject go4 =  Instantiate(exitPrefab, new Vector2(cols - 2, rows - 2), Quaternion.identity) as GameObject;
        go4.transform.SetParent(mapHolder);


    }


    //生成物品函数 - 引入的第二个变量是物品对应的数组
    private void InstantiateItems(int count, GameObject[] prefabs)
    {
        for (int i = 0; i < count; i++)
        {
            //随机取得位置
            Vector2 pos = RandomPosition();
            //随机从物品数组中取得一个变量，按在地图上
            GameObject enemyPrefab = RandomPrefab(prefabs);
            //Instantiate 函数，其作用是在场景中实例化（创建）一个新的游戏对象
            //as 用于执行类型转换（或称为强制类型转换）
            GameObject go = Instantiate(enemyPrefab, pos, Quaternion.identity) as GameObject;
            //Instantiate返回的值是Object
            go.transform.SetParent(mapHolder);
        }


    }

    //随机在地图取得一个位置
    private Vector2 RandomPosition()
    {
        int positionIndex = Random.Range(0, positionList.Count);
        Vector2 pos = positionList[positionIndex];
        positionList.RemoveAt(positionIndex);
        return pos;
    }
    //随机选择一个物体，在数组中
    private GameObject RandomPrefab(GameObject[] prefabs)
    {
        int index = Random.Range(0, prefabs.Length);
        return prefabs[index];

    }
}
```

这里刷新敌人和物质的逻辑是方框中的区域：

![image-20230912193521583](image-20230912193521583.png)

也就是这个代码：

```c#
 //因为创建完成地图之后，数组存在值 - 复用清零
      	positionList.Clear();
        for(int x = 2; x < cols - 2; x++)
        {
            for(int y = 2; y < rows - 2; y++)
            {
                //随机添加一个位置
                positionList.Add(new Vector2(x, y));
            }
        }
```

--------

两个随机函数：

```c#
//随机在地图取得一个位置
    private Vector2 RandomPosition()
    {
        int positionIndex = Random.Range(0, positionList.Count);
        Vector2 pos = positionList[positionIndex];
        positionList.RemoveAt(positionIndex);
        return pos;
    }
    //随机选择一个物体，在数组中
    private GameObject RandomPrefab(GameObject[] prefabs)
    {
        int index = Random.Range(0, prefabs.Length);
        return prefabs[index];

    }
```

然后用两个随机函数封装成这个生成物品函数

```c#
    //生成物品函数 - 引入的第二个变量是物品对应的数组
    private void InstantiateItems(int count, GameObject[] prefabs)
    {
        for (int i = 0; i < count; i++)
        {
            //随机取得位置
            Vector2 pos = RandomPosition();
            //随机从物品数组中取得一个变量，按在地图上
            GameObject enemyPrefab = RandomPrefab(prefabs);
            //Instantiate 函数，其作用是在场景中实例化（创建）一个新的游戏对象
            //as 用于执行类型转换（或称为强制类型转换）
            GameObject go = Instantiate(enemyPrefab, pos, Quaternion.identity) as GameObject;
            //Instantiate返回的值是Object
            go.transform.SetParent(mapHolder);
        }


    }
```

然后就是一些函数的介绍了：

> `Instantiate` 是 `Unity `引擎中的一个函数，其主要作用是在游戏场景中创建（实例化）一个新的游戏对象`（GameObject）`。这个函数的用途非常广泛，通常用于在游戏中生成、复制或克隆游戏对象。
>
> 具体来说，`Instantiate` 的作用如下：
>
> 1. **创建游戏对象的实例：** 通过调用 `Instantiate`，你可以基于一个已存在的游戏对象或预制体（Prefab）创建一个全新的实例。这个新实例是该游戏对象或预制体的一个独立拷贝，可以在场景中独立存在和操作。
> 2. **指定位置和旋转：** 你可以传递一个位置（通常是一个 `Vector3` 或 `Vector2`）和旋转信息（通常是一个 `Quaternion`）作为参数，以确定新实例在场景中的初始位置和方向。
> 3. **返回新实例的引用：** `Instantiate` 函数会返回新创建游戏对象的引用（通常是 `GameObject` 类型的引用），你可以将这个引用保存到变量中，以后对该游戏对象进行操作、管理或修改。
> 4. **设置父子关系：** 你可以选择将新创建的游戏对象设置为其他游戏对象的子对象，从而建立父子关系。这对于组织场景中的游戏对象非常有用。
>
> 示例用法：
>
> ```c#
> // 创建一个名为 "MyObject" 的空游戏对象
> GameObject myObject = Instantiate(emptyGameObjectPrefab, new Vector3(0, 0, 0), Quaternion.identity);
> 
> // 创建一个名为 "MyClone" 的游戏对象，作为另一个游戏对象的子对象
> GameObject myClone = Instantiate(clonePrefab, parentObjectTransform);
> 
> // 创建一个名为 "MyPrefabClone" 的游戏对象，位置和旋转与另一个游戏对象相同
> GameObject myPrefabClone = Instantiate(prefabToClone, originalGameObject.transform.position, originalGameObject.transform.rotation);
> ```
>
> 总之，`Instantiate` 是 Unity 中用于动态创建游戏对象实例的重要函数，它允许你在运行时生成新的游戏对象，并控制它们的位置、旋转以及与其他游戏对象的关系。这对于动态生成场景中的元素非常有用，例如生成障碍物、敌人、子弹等。
>
> ---------
>
> `as` 是 C# 中的一个关键字，用于执行类型转换（或称为强制类型转换）。它的主要作用是尝试将一个表达式的值转换为指定的类型，如果成功则返回转换后的值，否则返回一个空引用（`null`）或默认值，而不会引发异常。
>
> 提到的代码中，`as` 用于将 `Instantiate` 函数返回的结果（通常是一个 `UnityEngine.Object` 类型）转换为 `GameObject` 类型。具体示例：
>
> ```c#
> GameObject go = Instantiate(enemyPrefab, pos, Quaternion.identity) as GameObject;
> ```
>
> 这行代码中，`Instantiate` 返回的对象是基类 `UnityEngine.Object` 类型，但你希望将其转换为更具体的 `GameObject` 类型，因此使用了 `as GameObject`。如果转换成功，`go` 将包含一个有效的 `GameObject` 引用，否则它将为 `null`。
>
> 这种使用 `as` 进行类型转换的方式比较安全，因为它不会引发异常，即使转换失败也不会导致程序崩溃。它通常用于检查对象是否具有特定类型，并根据需要采取相应的操作。如果对象无法转换为指定的类型，`as` 将返回 `null`，可以根据这个结果来进行错误处理或其他逻辑。
>
> ------------
>
> `SetParent` 是 Unity 引擎中 `Transform` 组件的一个方法，它用于设置游戏对象的父对象。这个方法的主要作用是改变游戏对象在场景中的层次结构，将其放置在另一个游戏对象的下方，从而创建父子关系。
>
> 具体来说，`SetParent` 的用途如下：
>
> 1. **建立父子关系：** 通过调用 `SetParent`，可以将一个游戏对象设置为另一个游戏对象的子对象。这意味着子对象将成为父对象的下级，其变换（位置、旋转、缩放）将相对于父对象进行计算。
> 2. **改变层次结构：** 游戏对象的层次结构表示它们在场景中的排列顺序和关系。通过设置父对象，你可以更改游戏对象在层次结构中的位置。
> 3. **控制局部坐标系：** 子对象的位置、旋转和缩放通常是相对于父对象的局部坐标系计算的。这允许你轻松地将子对象相对于父对象进行移动、旋转和缩放，而不必考虑全局坐标。
>
> 示例用法：
>
> ```c#
> // 将 myObject 设置为 parentObject 的子对象
> myObject.transform.SetParent(parentObjectTransform);
> 
> // 将 myChild 设置为 myParent 的子对象，并保持局部坐标不变
> myChild.transform.SetParent(myParentTransform, worldPositionStays: false);
> ```
>
> 总之，`SetParent` 方法是在 Unity 中用于管理游戏对象层次结构的重要工具。它使你能够创建复杂的场景组织，将游戏对象嵌套在其他游戏对象下方，以便更轻松地管理它们的相对位置和关系。

然后将我们的物体插入到：

![image-20230912183214073](image-20230912183214073.png)

运行结果如下：

![录制_2023_09_12_18_37_04_267](录制_2023_09_12_18_37_04_267.gif)

> `    private Game_Manager gameManager;` 这个是自建的对象。

## 完成状态机

地图物品和角色生成完成之后，我们需要完善人物和怪物的动作，例如被打有受击，打人有动作，怪物也是如此。

![image-20230912200336584](image-20230912200336584.png)

这是玩家的动画状态机，图中已经链接完成了，分别是我们上面创建的两个动画，受击动画和攻击动画。动画的创建操作按照前面制作主角的操作就行，将受击和攻击的图片连续拖入物品即可。

状态机是用于切换状态的，链接的线就是切换的方式（人物可以从静止到动其实就是状态机的作用）。可以在左边创建几个，触发器来触发我们的动作。

> 参数有`Float`，`Int`，`Bool`，`Trigger`。
>
> ![20210903162325143](20210903162325143.gif)
>
> `Float`、`Int`用来控制一个动画状态的参数，比如速度方向等可以用数值量化的东西，
> `Bool`用来控制动画状态的转变，比如从走路转变到跑步，
> `Trigger`本质上也是`bool`类型，但它默认为`false`，且当程序设置为`true`后，它会自动变回`false`。
>
> 如下这里创建一个`Int`类型的参数`AnimState`
>
> ![在这里插入图片描述](20210903162326144.png)

![image-20230912201521128](image-20230912201521128.png)

> [**`Animator Controller`**](https://www.jb51.net/article/221837.htm):
>
> 每个`Animator Controller`都会自带三个状态：`Any State`, `Entry`和 `Exit`。
>
> ![在这里插入图片描述](/20210903162325136.png)
>
> 1、`Any State`状态
>
> 表示任意状态的特殊状态。例如我们如果希望角色在任何状态下都有可能切换到死亡状态，那么`Any State`就可以帮我们做到。当你发现某个状态可以从任何状态以相同的条件跳转到时，那么你就可以用`Any State`来简化过渡关系。
>
> 2、`Entry`状态
>
> 表示状态机的入口状态。当我们为某个`GameObject`添加上`Animator`组件时，这个组件就会开始发挥它的作用。
> 如果`Animator Controller`控制多个`Animation`的播放，那么默认情况下`Animator`组件会播放哪个动画呢？ 由`Entry`来决定的。
> 但是`Entry`本身并不包含动画，而是指向某个带有动画的状态，并设置其为默认状态。被设置为默认状态的状态会显示为 橘黄色。
>
> ![在这里插入图片描述](/20210903162325137.png)
>
> 当然，你可以随时在任意一个状态上通过 `鼠标右键->Set as Layer Default State`更改默认状态。
>
> ![在这里插入图片描述](/20210903162325138.png)
>
> 记住， `Entry`在`Animator`组件被激活后 无条件 跳转到默认状态，并且每个`Layer`有且仅有一个默认状态。
>
> 3、`Exit`状态
>
> 表示状态机的出口状态，以红色标识。如果你的动画控制器只有一层，那么这个状态可能并没有什么卵用。但是当你需要从子状态机中返回到上一层(`Layer`)时，把状态指向`Exit`就可以了。
>
> ![在这里插入图片描述](/20210903162325139.png)

下面的是线段的属性，也就是控制切换动画的功能。

![image-20230914121208511](image-20230914121208511.png)

> 我们可以选中某个自定义状态，并在`Inspector`窗口下观察它具有的属性
>
> ![在这里插入图片描述](/20210903162325140.png)
>
> | 属性名        | 描述                                                         |
> | :------------ | :----------------------------------------------------------- |
> | Motion        | 状态对应的动画。每个状态的基本属性，直接选择已定义好的动画（Animation Clip）即可 |
> | Speed         | 动画播放的速度。默认值为1，表示速度为原动画的1.0倍。         |
> | Mutiplier     | 勾选右侧的Parameter后可用，即在计算Speed的时考虑 区域1 中定义的某个参数。若选择的参数为smooth, 则动画播放速度的计算公式为 smooth * speed * fps(animation clip中指定) |
> | Mirror        | 仅适用于humanoid animation(人型机动画)                       |
> | Cycle Offset  | 周期偏移，取值范围为0-1.0，用于控制动画起始的偏移量。把它和正弦函数的offset进行对比就能够理解了，只会影响起始动画的播放位置。 |
> | Foot IK       | 仅适用于humanoid animation(人型机动画)                       |
> | Write Default | 最好保持默认，感兴趣可以参考官方手册                         |
> | Transitions   | 该状态向其他状态发起的过渡列表，包含了Solo和Mute两个参数，在预览状态机的效果时起作用 |
> | Add Behaviour | 用于向状态添加“行为”                                         |

其中，`exit time`播放完整度，满的就是播放完全，没满就是播放部分。然后我们还需要配置一下对应的`conditions`控制器。

然后制作完成的状态过度就是这个样子：

![录制_2023_09_14_12_46_41_452](录制_2023_09_14_12_46_41_452.gif)

![录制_2023_09_14_12_49_29_314](录制_2023_09_14_12_49_29_314.gif)

> **状态间的过渡关系`(Transitions)`**:
>
> 直观上说它们就是连接不同状态的有向箭头
>
> ![在这里插入图片描述](20210903162325141.png)
>
> 要创建一个从`状态A`到`状态B`的过渡，直接在`状态A`上 鼠标`右键 - Make Transition`并把出现的箭头拖拽到`状态B`上点击鼠标左边即可。
>
> ![在这里插入图片描述](20210903162325142.gif)

然后完善剩下怪物的状态机就可以啦：



--------------

> **补充：**
>
> 添加状态控制参数（）
>
> 参数有`Float`，`Int`，`Bool`，`Trigger`。
>
> ![在这里插入图片描述](20210903162325143-1694675043266-21.gif)
>
> `Float`、`Int`用来控制一个动画状态的参数，比如速度方向等可以用数值量化的东西，
> `Bool`用来控制动画状态的转变，比如从走路转变到跑步，
> `Trigger`本质上也是`bool`类型，但它默认为`false`，且当程序设置为`true`后，它会自动变回`false`。
>
> 如下这里创建一个`Int`类型的参数`AnimState`
>
> ![在这里插入图片描述](20210903162326144-1694675043267-23.png)

> **编辑切换状态的条件**
>
> 点击连线，在`Inspecter`窗口中可以进行设置，在`Conditions`栏下可以添加条件，如下图表示当参数
> `AnimState`为`0`时会执行这个动画`Any State`到`New Animation2`的过渡
>
> 必须在Parameters面板中添加了参数才可以在这里查看到，其次添加的条件为&&”与”关系，即必须同时满足。
>
> ![在这里插入图片描述](20210903162326145.png)

## 控制主角的运动

我们需要控制我们的角色运动，需要赋予物体一些物理特征，让其能够碰撞移动之类的。

>刚体是用于模拟物理效果的：
>
>当一个游戏对象被赋予刚体组件之后，游戏引擎就会对其进行物理效果的计算和模拟。同时我们也可以给这个对象施加各种作用力，让它运动起来。
>另外如果要实现重力的效果，那么相应的游戏物体都必须附上刚体组件。
>
>![在这里插入图片描述](20210520090756488.png)
>
>使用方法，右键在需要附加刚体的物体，加上`Rigidbody`即可
>
>![在这里插入图片描述](20210520091143625.png)
>
>![在这里插入图片描述](2021052009122631.png)
>
>| 属性                    | 作用                                                         |
>| ----------------------- | ------------------------------------------------------------ |
>| **Mass**                | 质量 质量越大，惯性越大。建议场景中的物体质量最好不要相差100倍率以上。防止两个质量相差太大的物体碰撞后会产生过大的速度，从而影响游戏性能及呈现的效果。 |
>| **Drag**                | 阻力（摩擦力） 这里指的是空气阻力，属性数值影响阻碍此物体对象的直线运动的速度效果。当游戏物体受到某个作用力的时候，这个值越大越难移动。如果设置成无限的话，物体会立即停止移动 |
>| **Angular Drag**        | 角阻力（旋转摩擦力） 同样指的是空气阻力，只不过是用来阻碍物体旋转的。如果设置成无限的话，物体会立即停止旋转 |
>| **Use Gravity**         | 使用重力效果 不勾选，则不会受到重力影响。                    |
>| **Is Kinematic**        | 是否符合运动学的（是否受到物理引擎的驱动） 勾选后，变成不再受物理引擎的影响，改为受Transform的影响。即不再有重力，不再被碰撞等，只会呆在Transform规定的位置上不动，物体撞击时候像一堵墙一样不会倒，位置不会因碰撞而发生改变 |
>| **Interpolate**         | 差值类型 如果看到刚体移动的时候运动的不是很平滑，可以选择一种平滑方式。即：平滑物体运动的曲线 None（无差值）：不使用差值平滑 Interpolate（差值）：根据上一帧来平滑移动 Extrapolate（推算）：根据推算下一帧物体的位置来平滑移动 |
>| **Collision Detection** | 碰撞侦测。用来改变物体碰撞检测的精度 Discrete（离散）：默认的碰撞检测方式。但若当物体A运动很快的时候，有可能前一帧还在B物体的前面，后一帧就在B物体后面了，这种情况下不会触发碰撞事件，所以如果需要检测这种情况，那就必须使用后两种检测方式 Continuous（连续）：这种方式可以与有静态网格碰撞器的游戏对象进行碰撞检测。 可以避免因物体移动速度过快而穿过另一个物体的情况 Continuous Dynamic（动态连续）：这种方式可以与所有设置了2或3方式的游戏对象进行碰撞检测 |
>| **Constraints**         | 约束 约束位置或旋转时的x/y/z坐标,使其Freeze(冻结)。比如想控制游戏对象人物上台阶不会摔倒，或者高速碰到一个墙壁物体时不会胡乱转动的话,则要冻结x，y和z轴的旋转 `centerOfMass`:相对于变换原点的质心 `angularVelocity` 刚体的角速度向量,修改它可以使刚体进行旋转 |
>
>**[引用](https://bbs.huaweicloud.com/blogs/298023)**

![录制_2023_09_14_15_53_22_790](录制_2023_09_14_15_53_22_790.gif)

对于`2d`的物体，我们不需要它的重力(如果有重力会往下掉)，接着为了我们主角和其他物体有碰撞需要给主角添加碰撞器`Box Collider 2D`

![image-20230914162022215](image-20230914162022215.png)

注意在设置碰撞箱的时候，需要将角色的碰撞箱设计小一点，这样子相邻格子就不会发生碰撞了。

![image-20230914163138955](image-20230914163138955.png)

> [碰撞器](https://blog.csdn.net/xiegy_august/article/details/106491229#fn1)：
>
> 碰撞器`(Collider)`是组件，加了碰撞器的游戏物体才可能实现碰撞效果。在Unity内部提供了许多碰撞器，通过`Add Component -> Physics`可以添加`3D`碰撞器组件。Unity提供的组件有：`BoxCollider(盒碰撞器), SphereCollider(球碰撞器), CapsuleCollider(胶囊碰撞器), MeshCollider(网格碰撞器), WheelCollider(轮子碰撞器，用来创建交通工具), TerrainCollider(地形碰撞器), CharacterController(角色控制器)。`
>
> 其中前三种为**基元碰撞器**，`MeshCollider`为常用碰撞器（后几种碰撞器有着特殊作用）。其中`BoxCollider`结构最简单，消耗最小，而`MeshCollider`最为复杂，消耗很高，且需要提供网格模型。
>
> **当两个碰撞体`(Collider)`发生了碰撞`(Collision)`，且其中至少一个附加了刚体`(Rigibody)`，会向它们附加的对象发射三条碰撞信息，可以在脚本里处理这些信息**。
>
> ```CSharp
> //其中collision为本次碰撞信息集合，从中可以读取与之发生碰撞的碰撞体，碰撞的冲击力等信息
> private void OnCollisionEnter(Collision collision) { }    //当两个碰撞体刚互相接触时
> private void OnCollisionStay(Collision collision) { }    //当两个碰撞体持续保持接触状态时
> private void OnCollisionExit(Collision collision) { }    //当两个碰撞体停止互相接触时
> 1234
> ```
>
> 这里要详细介绍一下网格碰撞体`(MeshCollider)`，其采用网格资源并基于该网格构建其碰撞体：
> ![网格碰撞器的Inspector视图](2020060311035887.png)
> *碰撞网格使用背面剔除，如果对象与图形方式背面剔除的网格碰撞，则不会以物理方式与它碰撞（故两个网格碰撞体无法碰撞）*。但若是该网格碰撞体标记为凸体(Convex)，则可以和其它网格碰撞体发生碰撞，**标记为凸体的网格碰撞体网格限制为255个三角形**。

为了使我们可以操控角色，需要在角色这里编写一些脚本，这里就创建一个`c#`的脚本代码。

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEditor;
using UnityEngine;

public class Player : MonoBehaviour
{
    //移动速度
    public float smoothing = 1;
    //休息时间
    public float restTime = 1;
    //计时器
    public float restTimer = 0;
    //起始位置
    private Vector2 targetPos = new Vector2(1,1);
    
    //命名的刚体和碰撞器
    private Rigidbody2D Rigidbody;
    private BoxCollider2D Collider;

    // Start is called before the first frame update
    void Start()
    {
        Rigidbody = GetComponent<Rigidbody2D>();
        Collider = GetComponent<BoxCollider2D>();
    }

    // Update is called once per frame
    void Update()
    {
        //物体运动逻辑
        Rigidbody.MovePosition(Vector2.Lerp(transform.position, targetPos, smoothing * Time.deltaTime));

        //休息时间间隔计算
        restTimer += Time.deltaTime;
        if (restTimer < restTime) return;

        //控制移动 - 垂直+水平
        float h = Input.GetAxisRaw("Horizontal");
        float v = Input.GetAxisRaw("Vertical");

        //水平优先
        if (h > 0)
        {
            v = 0;
        }


        if (h!=0 || v!=0)
        {
            //检测
            Collider.enabled = false;
            RaycastHit2D hit = Physics2D.Linecast(targetPos,targetPos + new Vector2(h,v));
            Collider.enabled = true;

            if(hit.transform == null)
            {
                targetPos += new Vector2(h, v);
                
            }
            else
            {
                switch (hit.collider.tag)
                {
                    case "OutWall":
                        break;
                    case "Wall":
                        hit.collider.SendMessage("TakeDamage");
                        break;
                }
            }

            restTimer = 0; //攻击和移动都需要休息
        }
      
    }
}

```

> 物体移动的逻辑是靠这个代码:`Rigidbody.MovePosition(Vector2.Lerp(transform.position, targetPos, smoothing * Time.deltaTime));`
>
> 1. `Rigidbody.MovePosition`：这是Unity中的一个函数，用于移动Rigidbody组件附加的物体的位置。Rigidbody是用于模拟物理行为的组件，例如重力和碰撞。
> 2. `Vector2.Lerp(transform.position, targetPos, smoothing * Time.deltaTime)`：这是一个矢量插值（Lerp）操作。它在两个矢量之间进行插值，产生一个介于它们之间的新矢量。在这里，它在物体当前位置（`transform.position`）和目标位置（`targetPos`）之间进行插值。`smoothing`是一个控制插值速度的参数，`Time.deltaTime`是每一帧的时间间隔，用于使移动平滑。
> 3. 整个表达式作为参数传递给`Rigidbody.MovePosition`，它告诉Rigidbody在每一帧中移动物体，使其逐渐接近目标位置，从而创建平滑的移动效果。

