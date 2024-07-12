---
title: 部署
draft: false
tags:
---

## 前言
[官网](https://quartz.jzhao.xyz/hosting)有用其他第三方平台部署的,但我这里只展示使用[GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages)来部署

## 开始
[[使用Quartz4搭建自己的个人网站]]
首先确保已经在本地[[搭建]]好了项目.然后创建一个名为``` <你的github用户名>.github.io ```的仓库,**不要使用**```README```**不要添加**```gitignore```也**别选择**开源许可证(license).
创建完毕后复制项目链接回到控制台.
``` bash
#  列出所有远程仓库
git remote -v
# 将REMOTE-URL替换为项目链接
git remote set-url origin REMOTE-URL
# 这个应该是设置更新Quartz的更新地址 
git remote add upstream https://github.com/jackyzha0/quartz.git
```
执行完毕后,开始将本地项目上传到github
``` bash
npx quartz sync --no-pull
```
成功执行后,以后有更新只需要执行```npx quartz sync ```即可.
