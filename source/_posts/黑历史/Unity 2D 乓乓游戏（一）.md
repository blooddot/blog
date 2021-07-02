---
title: Unity 2D 乓乓游戏（一）
date: 2017-02-22 00:00:00
tags: game
categories: 黑历史
---
【非原创2D游戏教程】乓乓游戏
[教程目录地址](https://blooddot.cool/2017/02/22/%E9%BB%91%E5%8E%86%E5%8F%B2/%E3%80%90%E9%9D%9E%E5%8E%9F%E5%88%9B2D%E6%B8%B8%E6%88%8F%E6%95%99%E7%A8%8B%E3%80%91%E4%B9%93%E4%B9%93%E6%B8%B8%E6%88%8F/)
<!-- more -->
<!-- cSpell:disable -->

![20210702094615](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702094615.png)

这是一个只用38行代码就能够做出2d乒乓游戏的教程哟~ 当然，肯定是用unity游戏引擎来做的，安心啦，会一步步教的。  
给你个gif图，自己看游戏最终效果

![20210702094651](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702094651.png)

---

一点用都没有的前言  
这个游戏灵感来源于1972年的原始乒乓游戏（tm不就是抄的一样么），中文介绍只找到了互动百科，将就着看看咯（<https://baike.baidu.com/item/pong/22463728?fr=aladdin>） (已更新百度百科)  
先来讨论一下游戏的规则，顺便再学习♂一下Unity的基本知识<(￣︶￣)/  
就叫游戏的说明好了

![20210702094734](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702094734.png)

游戏规则会和原本的非常相（yi）像（yang），像控制左边和右边的挡板让球弹来弹去，弹到左边的墙，右边的人得分啦，反过来球弹到右边的墙，左边的人得分啦，但是球打到上下的墙肯定不会得分的，只会让球上下弹来弹去，然后弹到左右咯。所以要控制左右的板子接住球并回弹就哦了。其实这些都好废话，又不是zz，看gif图都懂的好伐。  
来看一下这个球打到板子上的回弹角度设定：

![20210702094801](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702094801.png)

如果球打到顶部的边角，就是那个绿色箭头，那么往上弹  
达到中间，看红色箭头，往回弹  
达到下面，蓝色箭头，自己看  
规则扯完了，其实不讲大家也都知道规则的，只是强迫症要把这段弄一弄

---

开始讲Unity相关的东西咯  
为了做游戏，快用unity，学好unity，走遍天下都有利。  
首先，自己百度unity去下载和安装，不会装的，那怪我咯(๑_•̀ω•́)_

关于unity的简介，我就借（chao）鉴（xi）一下百度百科了  
> Unity3D是由Unity Technologies开发的一个让玩家轻松创建诸如三维视频游戏、建筑可视化、实时三维动画等类型互动内容的多平台的综合型游戏开发工具，是一个全面整合的专业游戏引擎。Unity类似于Director,Blender game engine, Virtools 或 Torque Game Builder等利用交互的图型化开发环境为首要方式的软件。其编辑器运行在Windows 和Mac OS X下，可发布游戏至Windows、Mac、Wii、iPhone、WebGL（需要HTML5）、Windows phone 8和Android平台。也可以利用Unity web player插件发布网页游戏，支持Mac和Windows的网页浏览。它的网页播放器也被Mac widgets所支持。

顺便吐槽一下，百度百科中文翻译Unity3d竟然叫优美缔3D，瞎了  
再说一下，Unity虽然做游戏很方便，但是还是要写代码的，可以用C#，Javascript和Boo三种语言来写，选C#吧，微软大法好啊，虽然我因为大学时的C#老师太丑，这些年对C#都很嫌弃，但说实话，C#用起来的确很爽

---

装完了Unity没，装完了就赶紧来开启Unity的学习之路吧

![20210702095337](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702095337.png)

激活你的Unity啦，选个人版，不要钱的，当然你有钱你随意  
不过5.0以前的Unity是要收钱的，但天朝一般都用破解版

然后你自己注册或登录一下Unity的个人账号就可以进入到以下界面了，不知道怎么注册的，出门左转，问一下路人怎么用浏览器打开百度输入注册Unity账号  
看图，选下面的New Project，差不多就是新创建一个项目的意思

![20210702095422](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702095422.png)

然后取名字，按照官方的来吧，叫pong  
自己选项目的路径，最后记得选择2D项目  
点击Create project创建项目

![20210702095904](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702095904.png)

接下去泡一下泡面，等它loading完吧

---

介绍一下Unity基本的操作界面啦看下面(￣y▽￣)╭

![20210702095904](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702095904.png)

Hierarchy（层级视图）：Hierarchy视图是主要放于游戏场景中具体的游戏对象，比如摄像机平面贴图、3D贴图、光源、箱子、球体、胶囊体、平面和地形等。任何一个全新的的游戏工程创建完毕后，默认都会创建一个游戏场景并且将主摄像机添加在该场景的Hierarchy视图中。对于3D游戏来说。摄像机可以让我们以不同的视角观察游戏世界。  
上面的是百度百科的，我不是安利百度百科，只是我没翻墙

Project Area（项目区域）：其实就是放我们游戏素材的地方，像Textures（贴图）啦，3D Models（3D模型）啦，Scripts（脚本代码）啦，等等。我们可以直接把这里的素材拖拽到层级视图上面实例化（不过代码不应该叫绑定，不叫实例）

Scene（场景）：就是显示我们游戏的地方，可以用鼠标拖拽或者用键盘控制场景的上下左右移动，也可以在层级视图双击里面的摄像头或者其他的对象定位到双击的目标

Inspector（检视视图）：显示当前选择对象的属性。举个栗子，当我们选择Main Camera（主摄像头）的时候，我们能够看到Position（位置），Rotation（旋转角度），Name（名称）等等全部关于摄像头的全部信息

对于Unity这种组件式的游戏引擎来讲，刚创建的对象什么都没有（包括头发），我们要自己给场景里面添加组件。像Light（光效），Position（位置），Texture（贴图），3D Model（3D模型）等等。我们点击默认创建的Main Camera，可以从Inspector（检视视图）上看到它绑定的初始组件，Transform，Camera，GUI Layer，Flare Layer，Audio Listener。具体干什么用的，等之后用到了再讲吧。简单来说Unity的这种结构就有点像拼机器人，Main Camera就类似机器人的头，头里面有眼睛，耳朵，鼻子，嘴巴等等（这些就类似于刚才绑定在Main Camera上面的组件），把这些组件组合起来就是一个物体，然后游戏是由很多个物体组成的，就和机器人是由头，手，胸，脚等等组合起来的差不多（Main Camera：我来组成头部）

变换工具（上图中的左上角，具体看下图）：

![20210702100051](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702100051.png)

从左到右依次是  
手型工具（快捷键Q）：按住鼠标左键拖动视角  
移动工具（快捷键W）：选择物体后，物体会出现方向轴，拖动方向轴可以移动物体  
旋转工具（快捷键E）：选择物体后，物体会出现旋转轴，拖动旋转轴可以旋转物体  
缩放工具（快捷键R）：选择物体后，物体会出现缩放轴，拖动缩放轴可以缩放物体  
我反正没用快捷键，不过用多了撸啊撸放技能速度应该能提升吧（笑）  

播放暂停步进工具条：

![20210702100159](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702100159.png)

从左到右依次是  
播放：开始游戏，点击以后开始调试游戏，并且该按钮变成停止按钮，点击停止按钮来停止调试游戏  
暂停：当在调试游戏的时候，可以暂停游戏，修改一些数据，资源，脚本等来查看修改后的表现，不过在停止播放后，暂停游戏中修改的东西就会还原回播放之前的状态哦  
步进：前进到下一帧，关于帧频这个东西，如果不知道的话，可以自行百度以上是Unity编辑器的简单介绍，下面要进入游戏主体教程了哟吼吼吼吼
