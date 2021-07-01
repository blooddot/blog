---
title: EgretPro 入门与踩坑
date: 2021-07-01 23:38:48
tags:
categories: electron
---
<!-- cSpell:disable -->
## 安装开发环境

参考：[https://docs.egret.com/egretpro/docs/guide/getting-started-install](https://docs.egret.com/egretpro/docs/guide/getting-started-install)

确保引擎版本：egret engine版本是5.3.7+，egret pro版本是1.6.0+

## 创建运行3D示例项目

### 3D项目-步骤

1. 打开EgretPro，点击右边新建按钮，打开创建项目页面
![20210701232255](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210701232255.png)
2. 选择项目目录路径后，点击右下角新建按钮创建项目
3. 在egret Pro编辑器中点击顶部选择内置或者浏览器选项运行预览

### 3D项目-坑点

* 编辑器提示3000端口已被占用
![20210701232556](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210701232556.png)
    **问题原因**：因为EgretPro打开后会默认启动一个3000端口的http服务器，所以当3000端口被占用时会打开项目失败  
    **解决方案**：一般造成这种原因是因为我们打开了egret项目进行调试。
  * 如果是没有用webpack方式打包的egret项目可以在egret run命令后加上--port参数指定端口号运行，避免冲突。
  * 如果是webpack方式打包的egret项目，可以在scripts/config.ts文件中搜索port关键字修改启动时的默认端口号。
  * 可否修改EgretPro编辑器的默认端口？EgretPro是在Egret3D引擎源码的toolkit/compiler/compiler.js文件中设置的，搜索web: 3000可以查询到，但是修改后编辑器会有其他问题，暂时没有找正确的修改方式

## 创建运行2D嵌套3D示例项目

### 2D项目-步骤

1. 创建2D项目
2. 将3D项目拷贝到2D项目根目录
3. 进入到3D项目，运行npm install 或者 yarn 安装依赖库
4. 修改3D项目中的package.json中的build:library命令

    ```json
    "build:library": "egret-pro build . --target pro-library"
    ```

5. 3D项目中的egretpro.json添加resourceRoot属性

    ```json
    "resourceRoot": "[3D项目目录名]/resource/",
    ```

6. 3D项目中的egretpro.json文件targets对象添加如下属性

    ```json
    {
     "name": "pro-library",
     "projectRoot": "./pro-library"
    }
    ```

7. 3D项目中运行 build:library命令，将3D项目代码打包成pro-library库供2D项目使用
8. 将3D项目中libs/modules的oimo和box2d文件夹库拷贝到2D项目中的libs目录中
9. 2D项目的index.html和template/index.html中egret.runEgret方法添加pro: true选项

    ```typescript
    egret.runEgret({
        renderMode: "webgl", audioType: 0, calculateCanvasScaleFactor: function (context) {
            var backingStore = context.backingStorePixelRatio ||
                context.webkitBackingStorePixelRatio ||
                context.mozBackingStorePixelRatio ||
                context.msBackingStorePixelRatio ||
                context.oBackingStorePixelRatio ||
                context.backingStorePixelRatio || 1;
            return (window.devicePixelRatio || 1) / backingStore;
        },
        pro: true
    });
    ```

10. egretProperties.json文件中添加以下选项，用于编译3D项目生成的代码到2D项目中

    ```json
    {
      "name": "pro-library",
      "path": "./egret-3D-demo/pro-library"
    }
    ```

11. 修改Main.ts中的onButtonClick方法，用于显示3D内容到2D项目中

    ```typescript
    private async onButtonClick(e: egret.TouchEvent) {
        const texture = await egret.pro.createTextureFrom3dScene("assets/scenes/box2ds/test.scene.json", 640, 1136);
        const bitmap = new egret.Bitmap(texture);
        this.addChild(bitmap);
    }
    ```

12. 2D项目运行egret build -e 编译库
13. 2D项目运行egret run查看3D项目在2D中的表现

### 2D项目-坑点

* 运行build:library时报错
![20210701232703](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210701232703.png)
**问题原因**：macOS使用的换行符LF，安装的egret库中的运行脚本是CRLF格式，导致运行node命令时解析出错  
**解决方案**：用vscode打开3D项目中的node_modules/.bin/egret-pro文件，右下角将换行符格式改为LF并保存
![20210701232724](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210701232724.png)

* 编译库报错
![20210701232817](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210701232817.png)
**问题原因**：2D项目用的Typescript版本太低了，InstanceType是Typescript2.8+的功能  
**解决方案**：只是Box2D.d.ts描述文件用了这个特性，可以先将报错处的InstanceType\<T>改成any或者new ()=> T

### 常见问题

* egretProUtil为空
![20210701232835](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210701232835.png)
index.html 没有开启pro选项导致，参考步骤9解决

* 3D场景加载失败
![20210701232854](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210701232854.png)
场景资源路径错误，参考步骤5和11解决

* OIMO，Box2D库找不到
![20210701232909](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210701232909.png)
![20210701232924](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210701232924.png)
2D工程没有添加对应库，参考步骤8解决

## 2D项目与3D项目通信

## 2D发消息到3D

* 2D项目发送消息代码

    ```typescript
    /**
    * 发送消息方法
    * @param command 消息名称
    * @param target 目标对象，填写一个数字即可，保证发送到对象和接受对象一致
    * @param args 参数
     */
    egret.pro.dispatch(command: string, target: any, ...args: any[]): void;
    ```

* 3D项目接收消息代码

    ```typescript
    /**
    * 监听消息方法
    * @param eventType 消息名称，对应2D发送到消息名
    * @param target 目标对象，对应2D发送的对象
    * @param func 执行方法
    * @param thisObject 方法作用对象
     */
    Application.instance.egretProUtil.addEventListener(eventType: string, target: any, func: (...args: any[]) => void, thisObject: any): void;
    
    /**
    * 执行方法
    * @param args 对应发送消息的参数
     */
    func(...args: any[]): void
    ```

## 3D发消息到2D

* 3D项目发送消息代码

    ```typescript
    /**
    * 发送消息方法
    * @param command 消息名称
    * @param target 目标对象，填写一个数字即可，保证发送到对象和接受对象一致
    * @param args 参数
     */
    Application.instance.egretProUtil.dispatch(command: string, target: any, ...args: any[]): void;
    ```

* 2D项目接收消息代码

    ```typescript
    /**
    * 监听消息方法
    * @param eventType 消息名称，对应2D发送到消息名
    * @param target 目标对象，对应2D发送的对象
    * @param func 执行方法
    * @param thisObject 方法作用对象
     */
    egret.pro.addEventListener(eventType: string, target: any, func: (...args: any[]) => void, thisObject: any): void;
    
    /**
    * 执行方法
    * @param args 对应发送消息的参数
     */
    func(...args: any[]): void
    ```

## 发布项目

### 发布项目-步骤

运行egret publish命令进行打包

### 发布项目-坑点

* 提示box2d.min.js文件找不到
![20210701232943](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210701232943.png)
**问题原因**：白鹭打包实用的库文件是min.js文件，由于我们拷贝过来的box2d库只有box2d.js，并没有box2d.min.js文件  
**解决方案**：在libs/box2d目录下生成min.js文件，最快速简单的解决方法是拷贝一份box2d.js重命名为box2d.min.js

* 提示pro-library.min.js文件找不到
![20210701233044](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210701233044.png)
**问题原因**：同上  
**解决方案**：在 [3D项目目录名]/pro-library/文件夹下，生成pro-library.min.js。但要注意的是，这个文件包含了3D项目的代码，每次3D项目改完后，要执行build:library命令打包代码，如果要打包2D项目，要每次都拷贝覆盖一份pro-library.min.js文件保证此文件是最新的

## 创建运行 iOS工程

### 说明

* egret的iOS原生项目运行3D的原理实际上是创建了一个webview，用于渲染web服务器上的3D场景，3D项目的渲染本质上还是走的H5形式，并非原生渲染的形式
* 所以我们要有一个web服务器用于存放3D资源进程调试

### 步骤

1. 修改3D项目中egretpro.json文件的resourceRoot属性，注意前面的IP和端口号为你的本地IP和2D项目本地调试时候的端口号

    ```json
    "resourceRoot": "<http://192.168.31.28:8888/[3D>项目目录名]/resource/",
    ```

2. 运行2D项目调试，会启动一个web服务器，确保3D项目的资源可以通过web访问到
3. 打开egret launcher软件，选择2D项目，点击发布设置，填写应用名称和应用包名
![20210701233128](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210701233128.png)
4. 点击确认后会在2D项目的同目录生成iOS工程项目
5. 实用xcode打开该项目
6. 打开AppDelegate.mm文件，将_native.config.diableNativerender设为true
![20210701233149](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210701233149.png)
7. 选择模拟器，运行调试查看效果

## 真机运行

1. 确保模拟器运行正确的情况下，连接iPhone到Mac并信任
2. Xcode选择左上角Xcode -> Preferences -> Accounts界面，登录Apple ID
3. 选择右下角Manage Certificates按钮
![20210701233208](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210701233208.png)
4. 打开证书界面后，点击左下角添加证书
![20210701233226](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210701233226.png)
5. 打开项目设置界面，选择对应开发组
![20210701233242](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210701233242.png)
6. 点击顶部选择 iPhone设备进行调试
![20210701233258](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210701233258.png)
7. 首次调试时，iPhone设备要信任证书文件，访问允许网络，可访问web的3D资源（调试时在同一个局域网下即可）

## 使用Egret3DExportTools

### Egret3DExportTools-说明

egret官方有提供Egret3DExportTools工具用于Unity3D中的场景资源导出

github地址：[https://github.com/egret-labs/egret3d-unityplugin](https://github.com/egret-labs/egret3d-unityplugin)

### Egret3DExportTools-步骤

安装和使用，参考：[https://docs.egret.com/egretpro/docs/spec/unity-assets](https://docs.egret.com/egretpro/docs/spec/unity-assets)

## Egret3DExportTools-坑点

* 导出资源名中有_#加载会失败

    **问题原因**：可能和egret资源配置加载有关，具体原因未知

    **解决方案**：在Unity中先重命名后再导出

* Unity项目设置的模型Shader类型，导出时请确保设置为Unity的格式

## 附加问题

### 动画播放完毕回调

* **问题描述**：在做项目时，发现实体添加了Animation组件后，播放动画时，想要监听动画完成的事件官方文档中没有提供
* **解决思路**
  * 查看官方api说明，并且查看引擎源码，发现有一个AnimationEventType的定义
  * 全局搜索AnimationEventType没有发现用法，只发现了该对象的枚举值定义
  * 精简搜索关键字，只搜索AnimationEvent，发现官方示例的AnimationHelper类中有一个onAnimationEvent方法，猜测可能是用于动画状态的回调
  * 搜索onAnimationEvent方法，发现在Animation源码目录中会派发相应的onAnimationEvent消息出来
  * 测试后发现果然onAnimationEvent方法是绑定在实体上面的动画回调方法
  * 该方法的参数类型为AnimationEvent，其中的type属性就是对应的AnimationEventType的枚举值（不过定义文件却是string类型，匪夷所思）

### EgretPro编辑器添加tag

* **问题描述**：添加多个实体后，我想给不同的实体添加tag，发现只能使用导出时预制的那几个tag，不能像Unity那样添加
* **解决思路**
  * 最简单的解决方法，在Unity中添加tag，再重新导出
  * 但是导出后会把我原本的EgretPro场景覆盖，又要重新搭建，效率不高
  * 猜测tag应该是作为某个配置文件保存在项目本地的，所以项目全局搜索一个tag值，并没有搜索到有用的信息
  * 猜测tag值可能保存在编辑器中，而非项目中
  * 编辑器目录中搜索一个tag值，发现有用信息
    ![20210701233319](https://raw.githubusercontent.com/blooddot/FigureBed/master/blog/20210701233319.png)
  * 原来tag值是配置在引擎库packages/engine目录下的文件
  * 分别在index.d.ts和index.js文件中添加我们想要的tag值
  * 重启EgretPro，发现我们添加的tag值果然生效了

## onCollisionEnter方法不生效

* **问题描述**：因为EgretPro设计思路很多都是借鉴Unity，所以碰撞代码我就直接用onCollisionEnter方法去实现，而且egret中也确实声明了onCollisionEnter方法，但是使用时却没有生效
* **解决思路**
  * 检查代码，并没有写错的感觉
  * 查看引擎源码，发现虽然d.ts中声明了onCollisionEnter，但是js源码中却并未实现，还是TODO状态，有点坑呀
  * 看来碰撞检测不能用简单的onCollisionEnter方式做了
  * 查看示例和源码发现有CollisionManager类，用于专门管理碰撞
  * 该类附带了raycast，raycastSingle，raycastAll三个方法，分别用于检测碰撞组件返回boolean值，检测单个实体对象，检测所有碰撞组件并返回精确信息
  * raycast是我想要的方法，但我想要获取到碰撞后的实体，不只是要一个boolean值，查看方法发现最后一个参数RaycastInfo传入后，可以赋值碰撞信息
  * 所以传入了后面两个附加参数maxDistance和RaycastInfo
  * 结果却出现了传入参数后和传入参数前结果不一致的情况
  * 查看源码发现传入数maxDistance和RaycastInfo后会多走一些逻辑
  * 其实源码里面已经找到碰撞的实体了，只是返回的是boolean值而已
  * 我修改了源码返回值，直接把实体返回，达到想要的效果
  * 并且比传入maxDisatcne和RaycastInfo效率还高

## 参考资料

* [安装 Egret Pro](https://docs.egret.com/egretpro/docs/guide/getting-started-install)
* [Egret Pro视频教程](https://docs.egret.com/egretpro/video)
* [Egret Pro 将3D项目发布成Native](https://mp.weixin.qq.com/s/XZ3jSxY9tTFWRLMzbVY5JQ)
* [EgretPro——入门全踩坑](https://blog.csdn.net/jiuyuefenglove/article/details/117695587)
