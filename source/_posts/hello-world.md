---
title: 实现Hexo自动化部署及在线编辑功能
tags: [hexo]
category: others
index_img: https://photo-1302571810.cos.ap-guangzhou.myqcloud.com/4%20.jpg
---
利用Github Action功能实现Hexo的自动化部署，不需要自己在本地进行代码的构建打包，以及实现在线编辑功能，从而方便Markdown文本后续的修改和优化
<!-- more -->

## 利用Github Action功能实现Hexo的自动化部署
下面完成的是通过github的Action完成自动化部署，就是提交代码后自动发布，类似webhook
1.创建一个myblog的分支
2.创建 .github\workflows\deploy.yml 
3.向deploy文件中写入以下代码，大致意思是进行检查与相关部署（其中api文档在github的每个项目里action选项当中有）
4.提交代码修改后，push my-blog分支，然后到github中查看actions是否成功运行。

```
name: Build and Deploy
on: [push]
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout 🛎️
        uses: actions/checkout@v2 # If you're using actions/checkout@v2 you must set persist-credentials to false in most cases for the deployment to work correctly.
        with:
          persist-credentials: false
      - name: Install and Build 🔧 # This example project is built using npm and outputs the result to the 'build' folder. Replace with the commands required to build your project, or remove this step entirely if your site is pre-built.
        run: |
          npm install
          npm run build
        env:
          CI: false
      - name: Deploy 🚀
        uses: JamesIves/github-pages-deploy-action@releases/v3
        with:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          BRANCH: master # The branch the action should deploy to.
          FOLDER: public # The folder the action should deploy.
```

## 实现在线编辑功能
### 方法一 
利用对主题文件下的ejs模板进行修改，利用github网址的规律，添加一个指向该篇文章的超链接
代码如下：
```
<div>
  <a target="_blank" href="你的项目编辑页面链接不要后缀<%- page.source %>">编辑/a>
</div>
```

### 方法二
直接在vscode上下载.markdown-all-in-one 插件,可以实现在线预览，写好后直接提交代码进行更新
