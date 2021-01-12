---
title: electron-builder使用简介
date: 2021-01-8 00:59:45
tags:
categories: electron
---
使用electron, 打包必不可免, 官方推荐electron-builder库进行应用打包. 本篇主要就工作中用到的electron-builder打包功能进行总结.

## 安装

项目目录执行命令:

``` bash
yarn add -D electron-builder
```

## [构建配置](https://www.electron.build/configuration/configuration)

可以直接在package.json中配置,也可以添加electron-builder.yml(yml,json5,toml,js)文件进行配置,以json配置为例  

### 基本配置

``` json
"build": {
  "productName": "xxxx",        //项目名,这也是生成的exe文件的前缀名
  "appId": "com.example.app",   //应用程序 id
  "copyright":"xxxx",           //版权信息
  "files": [                    //打包包含文件 默认包含项目根目录文件
      "xxxx",
      "xxxx"
  ],
  "directories": {              //目录配置
  "buildResources": "xxxx",   //构建用的资源目录（不会包含在打包后的资源中, 例如nsis要用到的构建配置文件）
  "output": "xxxx",           //输出文件夹, 默认输出到dist文件夹
  "app": "xxxx",              //应用程序目录, 默认是app,www或工作空间
  },
  
  "buildDependenciesFromSource": false,   //boolean, 
  "nodeGypRebuild": false,        //boolean, 是否每次打包前都重新构建node-gyp
  "npmArgs": ["xxx"],        //Array<String> | String, 安装应用程序时,额外
}
```

### MacOS平台配置

- mac: MacOS平台构建选项, [详见](https://www.electron.build/configuration/mac>)  
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

## [命令及参数](https://www.electron.build/cli)

package.json文件中添加执行脚本(以windows 64位包为例)  

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

## 参考资料

<https://www.electron.build/>  
<https://github.com/electron-userland/electron-builder>  
<https://github.com/QDMarkMan/CodeBlog/blob/master/Electron/electron-builder%E6%89%93%E5%8C%85%E8%AF%A6%E8%A7%A3.md>
<https://stackoverflow.com/questions/54978918/what-is-the-purpose-of-buildresources-folder-in-electron-builder-building-proces>
