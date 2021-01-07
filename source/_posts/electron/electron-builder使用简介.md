---
title: electron-builder使用简介
date: 2021-01-8 00:59:45
tags:
categories: electron
---
# Electron-builder使用简介
使用electron，打包必不可免，官方推荐electron-builder库进行应用打包. 本篇主要就工作中用到的electron-builder打包功能进行总结.

安装 项目目录执行命令:
>yarn add -D electron-builder

package.json文件中添加执行脚本(以windows为例):
>electron-builder --win --x64

命令和参数一览（https://www.electron.build/cli）  
命令:  
  electron-builder build                    构建应用    [默认值]  
  electron-builder install-app-deps         下载app依赖  
  electron-builder node-gyp-rebuild         重建自己的本机代  
  electron-builder create-self-signed-cert  为Windows应用程序创建自签名代码签名证书  
  electron-builder start                    使用electronic-webpack在开发模式下运行应用程序(需要electron-webpack模块支持)  

构建参数:
  --mac, -m, -o, --macos   为macOS平台构建    (https://goo.gl/5uHuzj)   [array]  
  --linux, -l              为linux平台构建    (https://goo.gl/4vwQad)   [array]  
  --win, -w, --windows     为windows平台构建  (https://goo.gl/jYsTEJ)   [array]
  --x64                    构建64位安装包                               [boolean]
  --ia32                   构建32位安装包                               [boolean]
  --armv7l                 构建armv7l安装包                             [boolean]
  --arm64                  构建arm64安装包                              [boolean]
  --dir                    Build unpacked dir. Useful to test.         [boolean]
  --prepackaged, --pd      The path to prepackaged app (to pack in a
                           distributable format)
  --projectDir, --project  The path to project directory. Defaults to current
                           working directory.
  --config, -c             The path to an electron-builder config. Defaults to
                           `electron-builder.yml` (or `json`, or `json5`), see
                           https://goo.gl/YFRJOM

Publishing:
  --publish, -p  Publish artifacts (to GitHub Releases), see
                 https://goo.gl/tSFycD
                [choices: "onTag", "onTagOrDraft", "always", "never", undefined]

Other:
  --help     显示帮助                                           [boolean]
  --version  显示当前版本                                       [boolean]

## 参考资料
https://www.electron.build/  
https://github.com/electron-userland/electron-builder  
https://github.com/QDMarkMan/CodeBlog/blob/master/Electron/electron-builder%E6%89%93%E5%8C%85%E8%AF%A6%E8%A7%A3.md