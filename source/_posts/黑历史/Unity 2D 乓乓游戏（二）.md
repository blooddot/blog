---
title: Unity 2D 乓乓游戏（二）
date: 2017-02-22 00:00:00
tags: game
categories: 黑历史
---
【非原创2D游戏教程】乓乓游戏
[教程目录地址](https://blooddot.cool/2017/02/22/%E9%BB%91%E5%8E%86%E5%8F%B2/%E3%80%90%E9%9D%9E%E5%8E%9F%E5%88%9B2D%E6%B8%B8%E6%88%8F%E6%95%99%E7%A8%8B%E3%80%91%E4%B9%93%E4%B9%93%E6%B8%B8%E6%88%8F/)
<!-- more -->
<!-- cSpell:disable -->

来设置一下摄像头先吧  
我们在Hierarchy（层级）面板中选择Main Camera，如下图

![20210702114640](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702114640.png)

然后我们把目光焦点放到右侧的Inspector（检视）面板，将Background（背景）颜色改成黑色，设置图中的Size等等参数（你照着图中抄就好了）

![20210702114712](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702114712.png)

---

开始创建上下左右的墙壁了~  
我们要添加四面墙壁到游戏里面，在游戏里面这些墙壁的对象叫做Sprite（精灵），为什么叫这个英文名和中文翻译名，鬼才知道，或者我们也可以叫另一个稍微明白点的名字Texture（贴图）  
快把下面的图片下载下来

![3940f603918fa0ec622c309a2f9759ee3f6ddbca](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/3940f603918fa0ec622c309a2f9759ee3f6ddbca.png)

你眼睛没有瞎，上面图片是白的，你选中看看就知道了  
把上面的图片放到项目的Assets文件夹里，我习惯在Assets里创建一个Sprites文件夹，把图片资源放在这个文件夹里面  
如下图所示

![20210702115315](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702115315.png)

Materials文件夹：用于存放Unity里面创建的Material  
Prefabs文件夹：用于存放Unity里面创建的Prefab，预制体（暂时听不懂没关系）
Scenes文件夹：用于存放Unity里面创建的Scene，场景其实就是游戏场景，你可以保存一下当前的项目，保存的后缀名是.unity文件，这个其实就是当前的Unity场景，我们保存到Scenes文件夹里面  
Scripts文件夹：用于存放Unity里面创建的Script，也就是代码  
Sprites文件夹：用于存放图片素材  

---

创建游戏里面用到的墙壁吧我们选择Sprites文件夹，选中里面的两个墙壁图片，然后将Inspector（检视）面板中的属性设置为下图

![20210702115412](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702115412.png)

其实就只要设置Pixels Per Unit这个参数为1啦，这个参数是干嘛用的呢，从字面意思看是像素每单位，其实差不多就是这个意思了，就是多少单位占1个像素的意思，当我们设置为1的时候，那么墙壁的就是1个单位对应1个像素了（其实一般游戏框架里面都是默认这样，并没有这个参数可以设置的）

把刚才的墙壁加入到游戏场景中先在Sprites文件夹里选中一个竖着的墙，然后拖拽到Scene（场景）面板中，如下图

![20210702115412](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702115412.png)

然后再拖两根横着的墙和一根竖着的墙，强迫症患者赶快对齐这四根棒子

![20210702115708](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702115708.png)

用鼠标拖肯定对不齐的，在左边的Hierarchy（层级）面板一根选中墙壁，然后在右边的Inspector（检视）面板输入坐标来对齐吧

记得把墙壁重命名啊

![20210702115738](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210702115738.png)
