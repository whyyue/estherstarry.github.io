---
title: 如何上传本地代码到github上
date: 2017-01-17 22:01:01
---
##第一步：建立git仓库
cd到你的本地项目根目录下，执行git命令，此命令会在当前目录下创建一个.git文件夹。
git init
##第二步：将项目的所有文件添加到仓库中
git add .
这个命令会把当前路径下的所有文件，添加到待上传的文件列表中。
如果想添加某个特定的文件，只需把.换成特定的文件名即可
##第三步：将add的文件commit到仓库
git commit -m "注释语句"

##第四步：将本地的仓库关联到github上
git remote add origin https://自己的仓库url地址

##第五步，上传代码到github远程仓库
git push -u origin master
若出现异常(reject)，则使用git push -f origin master 强制推送
执行完后，如果没有异常，等待执行完就上传成功了，中间可能会让你输入Username和Password，你只要输入github的账号和密码就行了.
