---
title: "Hugo-Github"
date: 2024-03-16T20:18:57+08:00
draft: false
tags: ["Github"]
---
把 Hugo 博客托管到 Github Pages 上，可以以几乎为零的成本来搭建一个个人博客。

## 准备

1. 注册并登录验证 [Github](https://github.com/signup) 账号；
2. 安装 [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

## 部署

1. 登录 Github 创建一个 repository；

2. 使用 `Git clone` 命令将仓库克隆至本地；

3. 拷贝 Hugo 项目本地文件夹除`.git`外的所有文件至上一步克隆的文件夹内；

4. 使用下面命令推送到 Github：
   ```git
   git init
   git add README.md
   git commit -m "first commit"
   git branch -M main
   git remote add origin git@github.com:username/xxxxx.git # 替换远程库地址
   git push -u origin main
   ```

5. 网页访问第一步创建的 repository，主菜单选择 **Settings** > **Pages** ，将 **Source** 更改为 `GitHub Actions`；

6. 本地仓库 `.github/workflows` 目录下新建 **hugo.yaml** 文件，并填入下面代码：
   ```yaml
   # Sample workflow for building and deploying a Hugo site to GitHub Pages
   name: Deploy Hugo site to Pages
   
   on:
     # Runs on pushes targeting the default branch
     push:
       branches:
         - main
   
     # Allows you to run this workflow manually from the Actions tab
     workflow_dispatch:
   
   # Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
   permissions:
     contents: read
     pages: write
     id-token: write
   
   # Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
   # However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
   concurrency:
     group: "pages"
     cancel-in-progress: false
   
   # Default to bash
   defaults:
     run:
       shell: bash
   
   jobs:
     # Build job
     build:
       runs-on: ubuntu-latest
       env:
         HUGO_VERSION: 0.123.8
       steps:
         - name: Install Hugo CLI
           run: |
             wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
             && sudo dpkg -i ${{ runner.temp }}/hugo.deb          
         - name: Install Dart Sass
           run: sudo snap install dart-sass
         - name: Checkout
           uses: actions/checkout@v4
           with:
             submodules: recursive
             fetch-depth: 0
         - name: Setup Pages
           id: pages
           uses: actions/configure-pages@v4
         - name: Install Node.js dependencies
           run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
         - name: Build with Hugo
           env:
             # For maximum backward compatibility with Hugo modules
             HUGO_ENVIRONMENT: production
             HUGO_ENV: production
           run: |
             hugo \
               --gc \
               --minify \
               --baseURL "${{ steps.pages.outputs.base_url }}/"          
         - name: Upload artifact
           uses: actions/upload-pages-artifact@v3
           with:
             path: ./public
   
     # Deployment job
     deploy:
       environment:
         name: github-pages
         url: ${{ steps.deployment.outputs.page_url }}
       runs-on: ubuntu-latest
       needs: build
       steps:
         - name: Deploy to GitHub Pages
           id: deployment
           uses: actions/deploy-pages@v4
   ```

7. 提交本地更改至 Github；

8. 在 GitHub 的主菜单中，选择 **Actions **，当 GitHub 完成构建和部署您的站点时，状态指示器的颜色将更改为绿色，如图所示：<img src="C:\Users\benjamin\Desktop\benjamin-20240316081459.png" alt="benjamin-20240316081459" style="zoom:75%;" />

9. 点击上面显示的提交消息。你会看到这个指向站点的链接地址，打开即可访问 Hugo 站点；

{{< alert >}}
**提示**：每当从本地存储库推送更改，GitHub 会将站点并部署更改。
{{< /alert >}}