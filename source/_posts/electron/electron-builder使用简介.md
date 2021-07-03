---
title: electron-builder使用简介
tags: web
categories: electron
abbrlink: '1867545'
date: 2021-01-08 00:59:45
---
使用electron, 打包必不可免, 官方推荐electron-builder库进行应用打包.  
本篇主要就工作中用到的electron-builder打包功能进行总结.
<!-- more -->
<!-- cSpell:disable -->

## 一、安装

项目目录执行命令:

``` bash
yarn add -D electron-builder
```

## [二、构建配置](https://www.electron.build/configuration/configuration)

可以直接在package.json中配置,也可以添加electron-builder.yml(yml,json5,toml,js)文件进行配置,以package.json配置为例  

### 基础配置

``` json
"build": {
  "productName": "xxxx",  //String - 项目名,这也是生成的exe文件的前缀名
  "copyright":"Copyright © year ${author}", //String - 版权信息

  //目录配置
  "directories": {              
    "buildResources": "build",  //String - 构建用的资源目录（不会包含在打包后的资源中, 例如nsis要用到的构建配置文件）
    "output": "dist", //String - 输出文件夹, 默认输出到dist文件夹
    "app": "app", //String  - 应用程序目录, 默认是app,www或工作空间
  },
  
  //
  "buildDependenciesFromSource": false,   //Boolean - 是否用源编译开发依赖项
  "nodeGypRebuild": false,        //Boolean - 是否每次打包前都重新构建node-gyp
  "npmArgs": ["xxx"],         //Array<String> | String - 直译: 安装应用程序本地依赖（native deps） 时添加的额外命令行参数, 原文: Additional command line arguments to use when installing app native deps. 
  "npmRebuild": true,         //Boolean - 是否在打包应用程序之前重新构建本地依赖

  //
  "buildVersion": "xxxx",     //String - 构建的版本, 对应于MacOS 的CFBundleVersion 和 Windows 元数据属性，默认对应Version, 如果已经定义TRAVIS_BUILD_NUMBER 、 APPVEYOR_BUILD_NUMBER 、 CIRCLE_BUILD_NUM 、 BUILD_NUMBER 、 bamboo.buildNumber 这些环境变量，那么将会被用作 build Version（version.build_number）
  "electronCompile": true,    //Boolean - 是否使用 electron-compile 来编译应用程序, 注:electronCompile已废弃
  "electronDist": "~/electron/out/R", //String - 自定义electron构建路径
  "electronDownload": { //electron-download 选项  详见:<https://github.com/electron/get#usage>
    "version": "xxx", //String - 版本
    "cache": "xxxx",  //String - 缓存位置
    "mirror": "xxxx", //镜像
    "strictSSL": false, //Boolean
    "isVerfyChecksum": false, //Boolean
    "platform":"xxx", //“darwin” | “linux” | “win32” | “mas”
    "arch":"xxxx" //String
  },
  "electronVersion":  "xxx",    //String - 打包用的electron版本, 默认为electron electron-prebuilt electron-prebuilt-compile 依赖版本
  "extends": "xxx", //String - 内置预设配置或配置文件的路径（相对于项目目录）当前只支持react-cra,如果安装了react-scripts依赖, react-cra会被自动设置，设置为null可以禁用自动检测
  "extraMetadata": "xxx", //any - 注入额外属性到package.json中
  "readonly": false, //Boolean - 应用签名失败时,是否构建失败(用于停止构建签名失败的应用程序) 
  "nodeVersion": "current", //String - 仅限于libui-based frameworks ，打包所用的NodeJS版本，设置current表示当前运行的NodeJS版本
  "launchUiVersion": "", //Boolean | String - 仅限于libui-based frameworks, 你所要打包的 LaunchUI 版本. 仅仅针对于Windows, 默认为适合框架使用的版本
  "framework": "electron", //String - 框架名称，electron proton-native libui 默认为electron

  //Hooks 
  "afterPack": "xxxx",  //打包后(签名前)执行的函数(文件或模块id的路径)
  "afterSign": "xxxx",  //签名后(打包成发行版之前)执行的函数(文件或模块id的路径)
  "artifactBuildStarted": "xxxx",   //artifact build(待理解)开始时执行的函数(文件或模块id的路径)
  "artifactBuildCompleted": "xxxx",   //artifact build(待理解)完成时执行的函数(文件或模块id的路径)
  "afterAllArtifactBuild": "xxxx",    //所有artifact build(待理解)运行完后执行的函数(文件或模块id的路径)
  "onNodeModuleFile": "xxxx",         //要在每个节点模块文件上运行的函数(文件或模块id的路径)
  "beforeBuild": "xxxx",              //只有npmRebuild配置设置为true时生效, 依赖库被安装或重新编译后执行的函数(文件或模块id的路径).

  //
  "remoteBuild": true,          //Boolean - 当前操作系统不支持构建时,使用Electron远程服务构建
  "includePdb": false,          //Boolean - 是否包含PDB文件(<https://en.wikipedia.org/wiki/Program_database>)
  "removePackageScripts": true,   //Boolean - 是否从package.json中移除scripts项
}
```

除了上述的基本配置,对应不同的平台,还可以有不同的配置参数,如下:  

### MacOS平台配置

- mac: MacOS平台构建选项, [详见](https://www.electron.build/configuration/mac)  
- mas: Mac应用程序商店构建选项, [详见](https://www.electron.build/configuration/mas)  
- dmg: Mac dmg包构建选项, [详见](https://www.electron.build/configuration/dmg)  
- pkg: Mac pkg包构建选项, [详见](https://www.electron.build/configuration/pkg)  

### Window平台配置

- win: Windows平台构建选项, [详见](https://www.electron.build/configuration/win)  
- nsis: nsis 构建选项, [详见](https://www.electron.build/configuration/nsis)  
- nsisWeb: nsis web安装包构建选项, 继承自nsis构建选项, [详见](https://www.dazhuanlan.com/2019/10/10/5d9ef51ae2c29/)  
- appx: Windows应用程序商店构建选项, [详见](https://www.electron.build/configuration/appx)  
- squirrelWindows: 已废弃

### Linux平台配置

- linux: Linux平台构建选项, [详见](https://www.electron.build/configuration/linux)  
- deb: Linux deb包构建选项, [详见](https://www.electron.build/configuration/linux#de)  
- snap: Linux snap包构建选项, [详见](https://www.electron.build/configuration/snap)  
- appImage: Linux appImage包构建选项, [详见](https://www.electron.build/configuration/linux#appimageoptions)  
- rpm: Linux rpm包构建选项, [详见](https://www.electron.build/configuration/linux#LinuxTargetSpecificOptions)  
- freebsd: Linux freebsd包构建选项, [详见](https://www.electron.build/configuration/linux#LinuxTargetSpecificOptions)  
- p5p: Linux p5p包构建选项, [详见](https://www.electron.build/configuration/linux#LinuxTargetSpecificOptions)
- apk: Linux apk包构建选项, [详见](https://www.electron.build/configuration/linux#LinuxTargetSpecificOptions)

### 既能配置在build项, 又能在每个平台中复写的配置

- **appId** = com.electron.${name} String - 应用程序id
- **artifactName** String - 构建生成的文件名字模板, 默认为 ${productName}-${version}.${ext} (有些平台会有不同的默认值,具体查看各自平台的配置)  
- **compression** = normal "store" | "normal" | "maximum" - 压缩等级 如果要快速测试构建，store 能够显著地缩短构建时间，maximum 不会导致明显的尺寸差异，但是会增加构建时间。

- **files** Array\<String | FileSet\> | String | FileSet - 打包包含文件 默认包含项目根目录文件
- **extraResources** Array\<String |FileSet\> | String | FileSet - 外部资源路径,项目目录相对路径,拷贝匹配的目录或文件到应用程序资源目录下(对于MacOS是Contents/Resources, 对于Linux和Windows是resources)  

```json
FileSet = {
  "from": "./extraResources/",    //来源路径
  "to": "extraResources"          //目标路径
}
```

- **extraFiles** Array\<String | FileSet\> | String | FileSet - 和extraResources类似,只是拷贝目标目录是应用程序内容目录(对于MacOS是Contents,对于Linux和Windows是根目录)
- **asar** AsarOptions | Boolean - 是否打包程序源代码为Electron的压缩包格式

```json
AsarOptions = {
  "smartUnpack": true,        //是否自动不打包可执行文件
  "ordering": "xxxx",         //string
  "asarUnpack": [             //Array<String>|String 相对于项目目录,指定哪些文件不需要打包进压缩包内
    "xxxx"
  ]
}
```

- **fileAssociations** Array\<FileAssociation\> | FileAssociation - 关联文件(特定格式的文件,双击后用此应用程序打开)

```json
FileAssociation = {
  "ext": "xxxx", //文件扩展名, 例如png
  "name": "xxxx", //名称, 例如PNG, 默认为ext同配置
  "description": "xxxx", //只支持Windows操作系统, 文件描述
  "mimeType": "xxxx",    //只支持Linux操作系统, 媒体类型
  "icon": "xxxx",   //icon路径(MacOS为icns格式, Windows为ico格式), 构建用的资源目录的相对路径, Linux不支持
  "role":"Editor",  //只支持MacOS The app’s role with respect to the type 可选值: Editor, Viewer, Shell, None.
  "isPackage": false,   //只支持MacOS Whether the document is distributed as a bundle. If set to true, the bundle directory is treated as a file. Corresponds to LSTypeIsPackage.
  "protocols":["xxxx"], // Array<Protocol> | Protocol - The URL protocol schemes.
  "schemes":["xxxx"], // Array<String> - The schemes. e.g. ["irc", "ircs"].
}
```

- **forceCodeSigning** Boolean - 当应用程序签名失败时, 是否打包失败
- **electronUpdaterCompatibility** = ">=2.15" String - electronUpdater兼容版本, e.g. >= 2.16, >=1.0.0.
- **publish** 发布设置 [详见](https://www.electron.build/configuration/publish)
- **detectUpdateChannel** = true Boolean - (待理解) Whether to infer update channel from application version pre-release components. e.g. if version 0.12.1-alpha.1, channel will be set to alpha. Otherwise to latest.
- **generateUpdatesFilesForAllChannels** = false Boolean - (待理解) Please see Building and Releasing using Channels.
- **releaseInfo** releaseInfo - 发布信息. 用于命令行使用:  

```bash
c.releaseInfo.releaseNotes="new features"
 ```

 ```json
releaseInfo = {
  "releaseName":"xxxx", //String - 发行名称
  "releaseNotes":"xxxx", //String - 发行说明
  "releaseNotesFile":"xxxx", //String - 发行说明文件, 默认为release-notes-${platform}.md (platform值为当前平台 — mac, linux or windows) 或者 release-notes.md 保存在build resources目录下面.
  "releaseDate":"xxxx", //String - 发行时间
  "target":""   //String | TargetConfiguration 测试后,发现已不存在该字段

}
 ```

## [三、命令及参数](https://www.electron.build/cli)

package.json文件中添加打包执行脚本(以windows 64位包为例)  

``` bash
electron-builder --win --x64
```

### 命令
  
electron-builder build                    构建应用 [默认值]  
electron-builder install-app-deps         下载app依赖  
electron-builder node-gyp-rebuild         重建自己的本机代  
electron-builder create-self-signed-cert  为Windows应用程序创建自签名代码签名证书  
electron-builder start                    使用electronic-webpack在开发模式下运行应用程序(需要electron-webpack模块支持)  

### 构建参数

--mac, -m, -o, --macos   为macOS平台构建 [array]  
--linux, -l              为linux平台构建 [array]  
--win, -w, --windows     为windows平台构建 [array]  
--x64                    构建64位安装包 [boolean]  
--ia32                   构建32位安装包 [boolean]  
--armv7l                 构建armv7l安装包 [boolean]  
--arm64                  构建arm64安装包 [boolean]  
--dir                    只构建未打包目录. 对于测试特别有用 [boolean]  
--prepackaged, --pd      预打包应用程序的路径（以可分发的格式打包）(这个参数有点难以理解, [详见](https://www.electron.build/#pack-only-in-a-distributable-format))  
--projectDir, --project  项目目录的路径。 默认为当前工作目录。  
--config, -c             配置文件路径。 默认为`electron-builder.yml`（或`js`, 或`js5`), [详见](https://goo.gl/YFRJOM)  

### 发布

--publish, -p  发布到GitHub Releases [选项: "onTag", "onTagOrDraft", "always", "never", undefined]

### 其他

--help     显示帮助 [boolean]  
--version  显示当前版本号 [boolean]  

### 例子

electron-builder --win --x64    构建windows 64位版本  
electron-builder -mwl           为macOS, Windows和Linux构建（同时构建）

## 四、参考链接

<https://www.electron.build/>  
<https://github.com/electron-userland/electron-builder>  
<https://github.com/QDMarkMan/CodeBlog/blob/master/Electron/electron-builder%E6%89%93%E5%8C%85%E8%AF%A6%E8%A7%A3.md>
<https://stackoverflow.com/questions/54978918/what-is-the-purpose-of-buildresources-folder-in-electron-builder-building-proces>
<https://blog.csdn.net/qq_38830593/article/details/89843722>
<https://forum.snapcraft.io/t/how-do-i-make-the-polarr-snap-associate-with-image-files/7316>
