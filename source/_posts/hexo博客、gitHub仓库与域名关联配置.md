---
title: hexo博客、gitHub仓库与域名关联配置
date: 2017-01-17 21:57:47
---
今天把博客配好了……
可能是因为经常被打断，遇到了很多傻问题，绕了不少弯。首先是 博客其实早早就配好了，只是没有跟个人域名绑定起来，输入github对应域名可以访问。不知道看了一个什么教程，我新建了一个叫testBlog的文件夹，把github上的代码pull下来了，然后开始各种折腾，不知道为什么，里面少了很多文件，比如说_config.yml文件都没有，而且不能用hexo的命令。本来都已经把个人域名关联好了，这一下子就傻眼了，testBlog下有一个叫EstherStarry.github.io的文件夹，里面才是博客相关的内容。于是要重新把原配（Blog文件夹）关联github了，git init、git add .、git commit -m "message"、git remote add origin https://XXX.github.io、git push -u origin master，最后还强制了一下，把-u换成-f了。再把CNAME文件复制过来再更新了一下代码，就可以在个人域名正常访问了。

写得比较草率，细节也不清楚。实际上是很简单的，只是我浪费了不少时间，干活的时候真的是不能被打断…

#### 另外hexo建博客看这个就好了 #### 
[HEXO搭建个人博客](http://baixin.io/2015/08/HEXO%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)
[HEXO进阶](http://baixin.io/2016/06/HEXO_Advanced/)
#### 常用的hexo命令 ####
hexo new "my blog"
hexo s -p 4001
hexo clean
hexo g
hexo deploy
喜欢用的markdown编辑器Mou在MacOS10.12.2上用不了…
#### 常用MarkDown语法 ####
分段：两个回车
换行：两个空格 + 回车
标题：# ~ ###### 井号的个数表示几级标题，即Markdown可以表示一级标题到六级标题
引用：>
列表：* ， + ， - ， 1. ，选其中之一，注意后面有个空格
代码区块：四个空格 开头
链接：[文字](链接地址)
图片：![图片说明](图片地址) ，图片地址可以是本地路劲，也可以是网络地址
强调：**文字** ， __文字__ ， _文字_ ， * 文字 *
代码：``，``
#### 本站主题&文件配置 ####
里面讲得很清楚: [next](http://theme-next.iissnan.com/getting-started.html)
文件配置说明: [Blog/_config.yml](http://www.isetsuna.com/hexo/install-config/)
[主题推荐@zhihu](https://www.zhihu.com/question/24422335)
#### 11:18 #### 
有个小问题啊…我hexo clean/hexo g/hexo deploy和git的命令不能同时用？？

#### 11:32 #### 
神奇的事。我在进入setting下的custom domain在文本框中填写whyyue.com，不光报404的问题解决了，之前好的时候主题显示不正确的问题也好了…之前以为只是加了CNAME文件就好了，没想到custom domain也要改…

#### 20170121 ####
本地改好后，hexo clean/hexo g/hexo deploy之后在localhost仍能正常看，在网站上打开就会崩404，而如果用git提交修改就可以…不知道为什么。