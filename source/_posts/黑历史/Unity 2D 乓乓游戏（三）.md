---
title: Unity 2D 乓乓游戏（三）
tags: game
categories: 黑历史
abbrlink: 60159eec
date: 2017-02-23 20:36:00
---
【非原创2D游戏教程】乓乓游戏
[教程目录地址](https://blooddot.cool/posts/dbdef186/)
<!-- more -->
<!-- cSpell:disable -->

给墙壁加上物理属性（让它能够和之后的小球基情的撞击）  
怎么加呢，不用担心啦，Unity就是因为它神奇的物理引擎而出名的，我们只要使用Unity提供的Collider组件就可以给墙壁添加物理碰撞属性啦，让我们先选中四个墙壁

![20210702205833](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702205833.png)

然后我们在右边的Inspector（检视）面板点击Add Component（添加组件），然后依次选择Physics2D->Box Collider2D

![20210702205855](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702205855.png)

不用改参数哦，Unity就是这么智能，默认把碰撞框的大小设置成图片大小

然后来Scene（场景）里面看看吧，每个墙壁是不是都有绿色边框围绕啊，那就是碰撞框

![20210702205955](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702205955.png)

我们再来添加一下中间的虚线

![20210702210258](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702210258.png)

放心老哥，你没瞎，选中下载虚线

老规矩，设置一下Pixels Per Unit为1

![20210702210334](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702210334.png)

调整一下中间虚线的位置，和下图差不多就行了，虚线就不用添加物理组件了，它只是用来装饰的玩意

![20210702210358](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702210358.png)

轮到球拍登场了快下图

![20210702210754](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702210754.png)

选中下载图片吧~  
也是把Pixels Per Unit设置为1，这我就懒得贴图了  
拖两个球拍的图片放到一左一右，作为我们控制的两个玩家，强迫症记得对其哟~

![20210702210538](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702210538.png)
