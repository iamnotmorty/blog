---
title: 使用travis ci持续集成hexo博客，实现自动化部署
date: 2019-09-29 11:05:06
tags: 
- Travis CI
- Hexo
index_img: https://s2.ax1x.com/2019/09/29/u8JmHx.jpg
---
最近用Hexo从新搭建博客，结果涨了一波见识，认识了Travis CI这个东西。
<!-- more -->
## Travis CI是什么？
### 什么是CI？
CI（持续集成）：只要有代码变更，就会触发CI服务器，对项目进行自动化构建，测试以及最终的自动化部署。

Travis CI也就是一个在线的CI服务。傻瓜式操作，与github是好基友，只要你的github项目是public，你就可以免费使用。

## 集成实现自动化部署
记得很早之前搭建hexo博客的时候，还没有这个操作，当时有个很大的问题，就是换了电脑会非常麻烦。现在有了这个操作，真的是如有神助。

以下是官网文档：
***
1.新建一个 repository。如果你希望你的站点能通过 <你的 GitHub 用户名>.github.io 域名访问，你的 repository 应该直接命名为 <你的 GitHub 用户名>.github.io。
2.将你的 Hexo 站点文件夹推送到 repository 中。默认情况下不应该 public 目录将不会被推送到 repository 中，你应该检查 .gitignore 文件中是否包含 public 一行，如果没有请加上。
3.将 Travis CI 添加到你的 GitHub 账户中。
4.前往 GitHub 的 Applications settings，配置 Travis CI 权限，使其能够访问你的 repository。
5.你应该会被重定向到 Travis CI 的页面。如果没有，请 手动前往。
6.在浏览器新建一个标签页，前往 GitHub 新建 Personal Access Token，只勾选 repo 的权限并生成一个新的 Token。Token 生成后请复制并保存好。
7.回到 Travis CI，前往你的 repository 的设置页面，在 Environment Variables 下新建一个环境变量，Name 为 GH_TOKEN，Value 为刚才你在 GitHub 生成的 Token。确保 DISPLAY VALUE IN BUILD LOG 保持 不被勾选 避免你的 Token 泄漏。点击 Add 保存。
8.在你的 Hexo 站点文件夹中新建一个 .travis.yml 文件：
```yml
sudo: false
language: node_js
node_js:
  - 10 # use nodejs v10 LTS
cache: npm
branches:
  only:
    - master # build master branch only
script:
  - hexo generate # generate static files
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GH_TOKEN
  keep-history: true
  on:
    branch: master
  local-dir: public
```
9.将 .travis.yml 推送到 repository 中。Travis CI 应该会自动开始运行，并将生成的文件推送到同一 repository 下的 gh-pages 分支下
10.在 GitHub 中前往你的 repository 的设置页面，修改 GitHub Pages 的部署分支为 gh-pages。
11.前往 https://<你的 GitHub 用户名>.github.io 查看你的站点是否可以访问。这可能需要一些时间。
***
直接按照官网的方法，存在一定的问题。

1.用 <你的 GitHub 用户名>.github.io 命名的项目，Travis CI 无法给我们自动新建 gh-pages 分支。
2.无法修改 GitHub Pages 的展示分支，默认是 master。也就达不到我们用 master 来管理博客（跟换电脑只需拉代码即可），用 gh—pages 部署静态博客以展示的目的。

个人实践后建议，新建项目的时候不要用 <你的 GitHub 用户名>.github.io 这样的方式命名，直接用别的名称命名。这样的话上面两个问题直接解决。

有人会说，那我怎么用 <你的 GitHub 用户名>.github.io 这个地址访问博客呢。其实很简单 <你的 GitHub 用户名>.github.io/<项目名称> 这个地址接可以访问。可以看看项目的 Setting 里的 GitHub Pages 相关的配置就很直观了。

![GitHub Pages](https://s2.ax1x.com/2019/09/29/u8rM1x.md.jpg)

最后在注意一个问题，这样修改以后有些主题的静态资源就无法正常访问了，注意很重要的一个配置，根目录下的_config.yml里面的url和root配置需要更改
```yml
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: https://iamnotmorty.github.io/blog
root: /blog/
permalink: :year/:month/:day/:title/
permalink_defaults:
```
这里还会有个坑在，就是即使这样修改了，静态资源诸如css/js之类的还是无法加载，当然本地是没有问题的，部署上去以后就会有问题。这时就要看看主题里面的页面是如何获取这些资源的路径的，找到相应的配置或者代码中进行修改，所以会有一定的差异。这里本人也有个疑问，就是官方说的会添加根路径，难道不是配置文件里的？有待求证，简单暴力的做法就是找到相应位置补全路径，有一些会有配置文件，改一下就好里。

后续会继续补充Hexo遇到的坑，以及Travis CI和持续集成相关的内容。


