---
title: 更换Github账号后'git push'和'hexo d -g'遇到的问题总结
date: 2017-10-22 09:55:46
tags: 
- Github 
- git 
- Hexo
categories: 
- 问题总结 
---
遇到问题多思考，多用搜索引擎，问题总归会解决的，不要轻易放弃
<!-- more -->
## 问题起因
由于之前Github账号过于随便的问题，重新注册了Github账号。
## 场景回顾
既然账号都换了，那就博客也重新来吧，之前的反正也是测试用的。
于是乎，做好所有准备工作，开干。
``` bush
$ git config --global user.name "xxx" 
$ git config --global user.email "xxxx@xxx.com"
```
正常
***
查看git配置
``` bush 
$ git config --lis
```
好像也没有问题
***
生成SSH密钥
``` bush
$ ssh-keygen -t rsa -C "xxxxx@xxx.com"
```
生成密钥
``` bush
$ cat ~/.ssh/id_rsa.pub
```
出现
``` bush
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDJPPudl9XqGxHvhs144OIIigWTy3MHD9jkM4bMm7HROEhj/qPUsin/td/YcsXx2HQI4JWTWmQwWD6YqlCx/rwltsYFkmm7OoNqe6kmnt
5QRyWyUHX0EAFb/jz7rhA4+B0INHlcIfWlEI8ccnz28UFTwdHbx8JDWOn86ex/nNshDbz+Okc9LREKHt7uamo7WFCO30iCAKQY1bfvqOckTpNnQbFetl+8IJOCa/+q4ISghMB8GzAvdYw5
e/SnJPDgMmjx8WO1FBhcpG2eiP4ij1kxeR01zl4B3EXG1o9vtWFXmNYTH7Y64b0tlkynCK1N/DIn9+iQ7wVEJg7AyvUpBhdz xxxxxxxx@xx.com
```
让后把这段内容复制进Github里相应位置
***
然后测试有没有链接成功
``` bush 
$ ssh git@github.com
```
根据输出的内容发现好像也成功了
#### 至此一切正常
***
最后省略Hexo的相关操作，假设你也都已经完成，来到最后一步发布
``` bush
$ hexo d -g 
```
出人意料的出现一大串的内容，当然是报错了。其中包含关键的一段
``` bush
remote: Permission to new-name/practice.git denied to old-name.
fatal: unable to access 'https://github.com/new-name/origin.git/': The requested URL returned error: 403
```
然后也是各种Goole各种百度，就是没有找到相关的解决办法，乱试一通结果还是不行，包括Github官方给出的解答，好像也没有解决问题，最后功夫不负有心人，我翻到一篇博客，看了一下，大概是本地凭证的问题，我受挫的小心脏那么一琢磨，哎呀，好像有点对路。试试把
> 控制面板-用户账户-凭据管理器-Windows凭据
#### 可以看到 git:https://github.com
#### 点开可以看到一个账号，那个是旧的账户。点击编辑，修改为现在的账户和密码就可以了
***
最后再次执行 hexo d -g 就成功了，同样的也解决了git push失败的问题。