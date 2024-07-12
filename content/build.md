---
title: 使用Quartz4搭建个人网站
draft: false
tags:
---

[Quartz](https://quartz.jzhao.xyz/)是一个快速的、包含电池的(batteries-included)的静态网站生成器,可以将Markdown的内容转换为[网站](https://quartz.jzhao.xyz/showcase).
官网还提到了[数字花园](https://jzhao.xyz/posts/networked-thought)的概念,感兴趣可以了解一下.
## **开始**
Quartz需要至少[Node](https://nodejs.org/)v18.14和``` npm ```v9.3.1才能正常运行,请确保安装对应版本.
然后再任意目录下执行以下命令
``` bash
git clone https://github.com/jackyzha0/quartz.git
cd quartz
npm i
npx quartz create
```
执行完毕后在content目录下即可开始编写文档.
## 运行
初始化完后执行以下命令即可在本地运行
``` bash
npx quartz build --serve
```
到这一步基本就搭建完毕了,记下来是[[host.md | 部署]].  