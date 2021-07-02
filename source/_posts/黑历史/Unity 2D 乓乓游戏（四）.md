---
title: Unity 2D 乓乓游戏（四）
date: 2017-02-22 00:00:04
tags: game
categories: 黑历史
---
【非原创2D游戏教程】乓乓游戏
[教程目录地址](https://blooddot.cool/2017/02/22/%E9%BB%91%E5%8E%86%E5%8F%B2/%E3%80%90%E9%9D%9E%E5%8E%9F%E5%88%9B2D%E6%B8%B8%E6%88%8F%E6%95%99%E7%A8%8B%E3%80%91%E4%B9%93%E4%B9%93%E6%B8%B8%E6%88%8F/)
<!-- more -->
<!-- cSpell:disable -->

对了，还要重命名球拍，Unity自动生成的名字看着实在难受

![20210702211005](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702211005.png)

球拍也要加上物理组件Box Collider 2D，别问我为什么，难道你用空气做的球拍打球么

![20210702211041](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702211041.png)

---

接下来是个好玩的知识点了，我们在游戏里面是需要控制球拍上下滑动移动坐标的，但当球拍上下移动到撞墙了，我们怎么去做出一个撞墙的效果，让球拍不会跑出墙外面呢
轮到Rigidbody（刚体）登场了，以下是百度百科：

> 刚体是指在运动中和受力作用后，形状和大小不变，而且内部各点的相对位置不变的物体。绝对刚体实际上是不存在的，只是一种理想模型，因为任何物体在受力作用后，都或多或少地变形，如果变形的程度相对于物体本身几何尺寸来说极为微小，在研究物体运动时变形就可以忽略不计。把许多固体视为刚体，所得到的结果在工程上一般已有足够的准确度。但要研究应力和应变，则须考虑变形。由于变形一般总是微小的，所以可先将物体当作刚体，用理论力学的方法求得加给它的各未知力，然后再用变形体力学，包括材料力学、弹性力学、塑性力学等的理论和方法进行研究。

我们可以给有刚体的物体添加各种属性实现很多效果，比如可以给它添加重力，让物体能够自由落体啦，让物体碰撞到添加了碰撞体的物体后不能继续穿墙移动啦
一般来说，使用了物理组件的话，移动的物体一般都要添加Rigidbody（刚体）组件的，有人要问为什么了，我也不知道，原文没写哈哈哈（我猜是一般物理游戏移动的东西都不能穿墙或者穿地面吧）

我们选中那两块球拍，然后在Inspector（检视）面板里面依次按以下顺序添加Rigidbody 2D组件 AddComponent->Physics 2D->Rigidbody 2D。然后修改Rigidbody 2D属性，把Gravity（重力）设为0，因为我们在平面上控制球拍，并不需要球拍自由落体般下落，然后勾选Freeze Rotation Z（冻结Z轴旋转），为了让球拍碰撞后不旋转，然后设置Collision Dectection（碰撞检测）为Continuous（持续的），并且设置Interpolate为Interpolate（内插值），查了一下，设置后可以基于上一帧的变换来平滑本帧变换，差不多就是移动的时候看起来顺畅一点，没有卡的意思。

![20210702212025](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702212025.png)

好了，接下来开始写代码了，怕不怕━((*′д｀)爻(′д｀*))━!!!!  
我们需要写一个脚本来控制球拍的移动，选中球拍，然后在右边的Inspector（检视）面板里面添加脚本 Add Component->New Script，取名叫MoveRacket，移动球拍的意思，选择CSharp语言

![20210702212131](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702212131.png)

我们在Project（项目）面板双击打开创建的脚本，打开后差不多是这个样子

```csharp
using UnityEngine;
using System.Collections;

public class MoveRacket : MonoBehaviour {
    // Use this for initialization
    void Start () {
    
    }
    
    // Update is called once per frame
    void Update () {
    
    }
}
```

可以看到里面有两个方法，Start方法是这个类初始化的时候运行的，就是你把这个脚本绑定在哪个物体上，当这个物体创建后，这个Start方法就会运行，然后这个Update方法是会一直运行的，大概是每秒运行60次的样子，反正根据设置的帧频来的

但Update方法有个缺点，就是你游戏卡的时候，我们一般叫掉帧，也就掉帧频啦，Update是根据帧频来计算运行的次数的，那么帧频掉了，Update方法当然执行的次数就会不太准，对于我们这种要实时计算碰撞的游戏来说是不太好的  
所以轮到FixedUpdate方法出现了，Fixed是固定的意思，那么这个方法我们也就可以猜到了，就是不管我们游戏帧频怎么样算，这个方法都是在固定的时间点执行的，不会受影响，对于物理游戏来说，这个方法是再好不过的了。

好，我们把Start方法和Update方法删了（反正用不到，占着位置干嘛，我又不是靠代码行数来算工资的），然后写一个FiexdUpdate方法

```csharp
using UnityEngine;
using System.Collections;

public class MoveRacket : MonoBehaviour {
    void FixedUpdate () {
    
    }
}
```

这个名字啊，不要弄错了啊，如果你是用的VS来写代码，可以用Ctrl+Shift+M快捷键来创建一些Unity内置的方法，这样就不用自己手打名字，也就不会搞错啦^o^不要忘记了我们创建这个代码是为了让球拍移动的，由于球拍添加了Rigidbody，所以我们可以用Rigidbody来移动球拍，Rigidbody里面有个属性叫velocity，就是刚体的速度向量，我摘抄一段圣典里面的话
在大多数情况下，你不应该直接修改速度，因为这会导致不真实的行为。在每个物理步，不要再每个物体的速度，这将导致不真实的物理模拟。一个典型的例子，当你在第一人称射击游戏中使用跳跃的时候改变速度，因为你想立即改变速度。  
但我们这个不是真是的物理模拟啊，我只是要让拍子在游戏里上下移动而已，我又不管重力什么的。这玩意里面既然有个向量的单词，那肯定是向量啦，我们来复习一下向量是啥（其实就是XY坐标）

![20210702212344](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702212344.png)

一目了然吧，我们要上下移动，所以要给球拍一个向上或是向下的向量，其实就是Y轴的向量然后有人要问了，我们这个向量哪里可以得到啊  
所以说Unity引擎很好呢，引擎里面有个Input类，里面提供了一系列方法获取用户输入的信息，我们这次就用GetAxisRaw这个方法，直译就是获取原始轴，大致就是用户通过输入控制器提供改变坐标轴的值，然后用这个方法来获取控制器改变的坐标轴的值。  
我们回到编辑器，依照下列顺序打开InputManager，Edit->Project Settings->Input

![20210702212414](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702212414.png)

可以看到一个列表，其实这些都是输入控制器，我们这次就用默认自带的Vertical来控制球拍吧  
写两句代码来移动球拍吧

```csharp
using UnityEngine;
using System.Collections;

public class MoveRacket : MonoBehaviour {
    public float speed = 30;
    public string axis = "Vertical";
    
    void FixedUpdate () {
        float v = Input.GetAxisRaw(axis);
        GetComponent<Rigidbody2D>().velocity = new Vector2(0, v) * speed;
    }
}
```

可以看到我们通过Input.GetAxisRaw来获取了一个输入值，然后复制给Rigidbody2D的速度向量Y坐标上，看了上面的向量图可以知道，赋值为正是向上移动，赋值为负是向下移动。我们再回过头来看输入控制器的截图可以发现“S”键对应的是Negative Button，“W”键对应的是Positve Button，Negative数学里翻译是负的意思，也就是S是输入负值，Positive数学里翻译是正的意思，那么W就是正值了，所以赋值给Rigidbody2D的速度向量后，按“S”键是向下移动，“W”键是向上移动，和我们习惯的操作一样，真方便啊~

然后speed是控制移动的速度，我们先设置为30吧。我们来运行一下游戏，发现可以通过S和W键来控制球拍的移动了，是不是有点小激动啊

给两块球拍添加不同的输入控制器但是我们只是想控制单块球拍的移动啊，如果用一个控制器控制两块球拍，玩个球啊，打着多没意思啊
那现在就要给两块球拍添加不同的控制器了，也很简单
我们在控制器页面输入控制器的数量，多加一位，并把新的控制器命名为Vertical2，设置方向键控制正负按钮

![20210702212540](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702212540.png)

然后再选中右边的板子，在Inspector（检视）面板里，将Script组件里的Axis属性换成Vertical2  
跑一下，哦耶，我们可以左手控制左球拍右手控制右球拍了~

![20210702212650](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702212650.gif)
