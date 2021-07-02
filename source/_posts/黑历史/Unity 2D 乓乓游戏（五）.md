---
title: Unity 2D 乓乓游戏（五）
date: 2017-02-22 00:00:05
tags: game
categories: 黑历史
---
【非原创2D游戏教程】乓乓游戏
[教程目录地址](https://blooddot.cool/2017/02/22/%E9%BB%91%E5%8E%86%E5%8F%B2/%E3%80%90%E9%9D%9E%E5%8E%9F%E5%88%9B2D%E6%B8%B8%E6%88%8F%E6%95%99%E7%A8%8B%E3%80%91%E4%B9%93%E4%B9%93%E6%B8%B8%E6%88%8F/)
<!-- more -->
<!-- cSpell:disable -->

做个球  
先把下面这个球的图片下载一下

![20210702212829](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702212829.png)

然后设置一下我们刚才导入的图片

![20210702212855](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702212855.png)

把球的图片拖到场景里面来吧，看下面

![20210702212915](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702212915.png)

给球添加一下Collider（碰撞器）组件  
Add Component->Physics 2D->Box Collider 2D

![20210702212934](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702212934.png)

我们的球是要在碰到墙的时候回弹回来的，举个栗子，当面对面碰到墙的时候，是直接回弹回来，当以45度朝着墙碰的时候，它会以-45度回弹回来

听起来有些复杂，涉及到了数学相关的知识，但Unity关于这方面提供了很完善的支持，用它自带的Physics Material，就能够实现回弹的效果，不用管其他杂七杂八的事情

让我们来创建一个Physics2D Material吧  
在Project Area（项目区域）里面一次选择Create->Physics2D Material，然后命名为BallMaterial  
在Inspector（检视）面板里面设置一下

![20210702213010](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702213010.png)

可以看到我们将Friction（摩擦力）设置为0 Bounciness（反弹力）设置为1

然后在球的Collider的Material设置一下刚才创建的Material

![20210702213036](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702213036.png)

给球添加一下Rigidbody  
Add Component->Physics 2D->Rigidbody 2D  
然后设置一下参数，像下面这样子

![20210702213056](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702213056.png)

好了，我们再来给球添加一个脚本，让它动起来吧  
Add Component->New Script，并命名为Ballusing  
看下面的代码

```csharp
UnityEngine;
using System.Collections;

public class Ball : MonoBehaviour {
public float speed = 30;

    void Start() {
        // Initial Velocity
        GetComponent<Rigidbody2D>().velocity = Vector2.right * speed;
    }
}
```

然后再加一个speed的变量，用来控制球速，并在初始化的时候，给球一个向右的速度，让它动起来  
点击调试，可以看到球在两个球拍之间回弹

![20210702213307](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702213307.gif)

好了，直线的回弹已经有效果了，斜着的回弹我们要实现下面这种回弹的方向

![20210702213337](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702213337.png)

我们要给Ball脚本添加一个OnCollisionEnter2D方法，用来处理碰撞到物体时的回弹角度

![20210702213407](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702213407.png)

上面这个图是展示各个方向的向量值  
我们要确定的是，弹到球拍上方，中间和下方的向量值，比较发现，上中下是根据Y轴的值来区分的，而且都在-1到1的区间内，我们通过下面的方式来计算  
|| 1 <- at the top of the racket
||
|| 0 <- at the middle of the racket
||
|| -1 <- at the bottom of the racket

那我们通过什么来确认球的Y轴值呢，这里的Y轴值不是指Y轴的坐标，而是只计算回弹方向的Y轴值，很明显，我们是球打在球拍上，球相对与球拍的位置，就是确认回弹方向的Y轴值，所以我们可以通过球拍的高度来计算Y轴值  
代码如下：

```csharp
float hitFactor(Vector2 ballPos, Vector2 racketPos,
float racketHeight) {
    // ascii art:
    // || 1 <- at the top of the racket
    // ||
    // || 0 <- at the middle of the racket
    // ||
    // || -1 <- at the bottom of the racket
    return (ballPos.y - racketPos.y) / racketHeight;
}
```

然后在碰撞方法里面分别判断左球拍，和右球拍的回弹计算

```csharp
void OnCollisionEnter2D(Collision2D col) {
    // Note: 'col' holds the collision information. If the
    // Ball collided with a racket, then:
    // col.gameObject is the racket
    // col.transform.position is the racket's position
    // col.collider is the racket's collider
    
    // Hit the left Racket?
    if (col.gameObject.name == "RacketLeft") {
        // Calculate hit Factor
        float y = hitFactor(transform.position,
        col.transform.position,
        col.collider.bounds.size.y);
        
        // Calculate direction, make length=1 via .normalized
        Vector2 dir = new Vector2(1, y).normalized;
        
        // Set Velocity with dir * speed
        GetComponent<Rigidbody2D>().velocity = dir * speed;
    }

    // Hit the right Racket?
    if (col.gameObject.name == "RacketRight") {
        // Calculate hit Factor
        float y = hitFactor(transform.position,
        col.transform.position,
        col.collider.bounds.size.y);
        
        // Calculate direction, make length=1 via .normalized
        Vector2 dir = new Vector2(-1, y).normalized;
        
        // Set Velocity with dir * speed
        GetComponent<Rigidbody2D>().velocity = dir * speed;
    }
}

```

好了，试试看吧，现在已经可以让球自由的弹来弹去了

---

总结♂  
在这个教程里，我们学会了安装和使用Unity，能够创建场景，添加文理，运用基本的Unity物理系统和写一些简单的脚本

记住啊，Unity用起来很简单，我们大多时候只需要用鼠标操作，以及些一些简单的脚本，就能够实现很多牛逼的效果，所以不用太害怕去创造新东西。  
下面列出一堆，你可以继续为这个游戏添加的有趣功能  
1.给小球添加追踪的效果  
2.添加音效  
3.添加一个显示分数  
4.让球动的越来越快  
5.加一个AI，现在我们只能左手和右手玩。。。  
6.添加一个初始菜单和结算界面7.发挥你的想象吧~
