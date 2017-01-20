---
title: hexo博客、gitHub仓库与域名关联配置
date: 2017-01-17 21:57:47
---
今天把博客配好了……
可能是因为经常被打断，遇到了很多傻问题，绕了不少弯。首先是 博客其实早早就配好了，只是没有跟个人域名绑定起来，输入github对应域名可以访问。不知道看了一个什么教程，我新建了一个叫testBlog的文件夹，把github上的代码pull下来了，然后开始各种折腾，不知道为什么，里面少了很多文件，比如说_config.yml文件都没有，而且不能用hexo的命令。本来都已经把个人域名关联好了，这一下子就傻眼了，testBlog下有一个叫EstherStarry.github.io的文件夹，里面才是博客相关的内容。于是要重新把原配（Blog文件夹）关联github了，git init、git add .、git commit -m "message"、git remote add origin https://XXX.gthub.io、git push -u origin master，最后还强制了一下，把-u换成-f了。再把CNAME文件复制过来再更新了一下代码，就可以在个人域名正常访问了。

写得比较草率，细节也不清楚。实际上是很简单的，只是我浪费了不少时间，程序媛是不能被打断的啊啊啊啊！！！

11:18
有个小问题啊…我hexo clean/hexo g/hexo deploy和git的命令不能同时用？？

11:32
神奇的事。我在进入setting下的custom domain在文本框中填写whyyue.com，不光报404的问题解决了，之前好的时候主题显示不正确的问题也好了…之前以为只是加了CNAME文件就好了，没想到custom domain也要改…