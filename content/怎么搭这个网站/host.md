---
title: 部署
draft: false
tags:
---

## 前言
[官网](https://quartz.jzhao.xyz/hosting)有用其他第三方平台部署的,但我这里只展示使用[GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages)来部署

## 开始
首先确保已经在本地[[build.md | 搭建]]好了项目.然后创建一个名为`<你的github用户名>.github.io`的仓库,**不要使用**`README`**不要添加**`gitignore`也**别选择**开源许可证(license).
创建完毕后复制项目链接回到控制台.
``` shell
#  列出所有远程仓库
git remote -v
# 将REMOTE-URL替换为项目链接
git remote set-url origin REMOTE-URL
# 这个应该是设置更新Quartz的更新地址 
git remote add upstream https://github.com/jackyzha0/quartz.git
```
执行完毕后,开始将本地项目上传到github
``` shell
npx quartz sync --no-pull
```
成功执行后,以后有更新只需要执行```npx quartz sync```即可.

## GitHub Pages

在本地项目中创建一个新文件  `quartz/.github/workflows/deploy.yml`.

```yaml title="quartz/.github/workflows/deploy.yml"
name: Deploy Quartz site to GitHub Pages

on:
  push:
    branches:
      - v4

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0 # Fetch all history for git info
      - uses: actions/setup-node@v4
      - name: Install Dependencies
        run: npm ci
      - name: Build Quartz
        run: npx quartz build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: public

  deploy:
    needs: build
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```
创建完毕后将仓库设置中的`Pages`中的`Build and deployment`的`Source`改为`GitHub Actions`,执行`npx quartz sync`.
等待Actions执行完毕后就可以访问`<你的github用户名>.github.io`查看你部署好的网站了.