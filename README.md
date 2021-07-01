# blog

博客工程

## 常用脚本

### 项目脚本

- 构建 yarn build
- 清理 yarn clean
- 发布 yarn deploy
- 调试 yarn server

### hexo脚本

- 添加分类页面 hexo new page categories
- 添加标签页面 hexo new page tags
- 添加文章 hexo new post article

## 编辑文章

### 添加文章分类关联

hexo中有Front-matter这个概念，是文件最上方以 — 分隔的区域，用于指定个别文件的变量。  
在hexo new post article时会生成article.md文件，文件生成好的文章属性。  
以下是文章属性例子

```Markdown

---
title: [标题]
date: Y-m-d H:M:S
tags: [标签]
categories: [分类]
---
[内容简介]
<!-- more -->
<!-- cSpell:disable -->

```

## 常见问题

### 每次部署后都要在github上重新设置域名

在source目录下，创建一个CNAME文件，填入自己域名（本例：blooddot.cool）
