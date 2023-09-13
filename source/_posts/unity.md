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
> ![img](/2021062216284812.png)
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

![image-20230912201521128](image-20230912201521128.png)



