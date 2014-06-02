---
layout: post
title: "git管理多个key"
date: 2014-06-02 10:58:29 +0800
comments: true
categories: backend
description: "git中生成多个key以及配置的解决方法"
keywords: "git, ssh-keygen, multiple keys, config"
---

在github或者git.oschina.net这样子的代码托管平台上，一个账户只对应一个ssh key。当你在github上创建第二个账户或者在github上同时有个人和公司的账户的时候，在多个账户中添加同一个ssh key就会失败。解决方案是在本地计算机上生成多个key，然后创建一个config配置文件，在配置文件中指定哪个账户使用哪个私有key进行验证。

<!-- more -->

<div>
{% codeblock lang:bash %}
#生成key，指定唯一的文件名，比如sec_rsa，然后一路回车
gly@gly:~$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/gly/.ssh/id_rsa): sec_rsa

#生成config配置文件
cd ~/.ssh
touch config

#写config文件
vi config

#写入以下内容
Host sec
HostName github.com
IdentityFile ~/.ssh/sec_rsa
{% endcodeblock %}
</div>

以后我们就可以用sec代替github.com进行远程克隆或提交：

<div>
{% codeblock lang:bash %}
#只有一个key时添加远程仓库
git remote add sec git@github.com:user/repo.git
#有多个key时添加远程仓库
git remote add sec git@sec:user/repo.git

#只有一个key时克隆
git clone git@github.com:user/repo.git
#有多个key时克隆
git clone git@sec:user/repo.git
{% endcodeblock %}
</div>




