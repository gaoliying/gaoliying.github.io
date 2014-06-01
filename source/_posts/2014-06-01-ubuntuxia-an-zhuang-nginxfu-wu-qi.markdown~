---
layout: post
title: "ubuntu下安装nginx服务器"
date: 2014-06-01 21:28:08 +0800
comments: true
categories: backend
description: "本文介绍如何在ubuntn系统下安装nginx服务器，并解决依赖关系"
keywords: "ubuntu, nginx, pcre, openssl"
---

###nginx服务器简介

nginx,读音('engine x'),是一款轻量级的http服务器／反向代理服务器，同时也是邮件(IMAP/POP3/SMTP)代理服务器。由俄罗斯程序工程师Igor Sysoev开发，以epoll和kqueue作为开发模型。其特点是占用内存少，高并发性能优异，能够支持高达50,000个并发连接数的响应。目前世界上大部分排名前1,000的网站采用nginx。

<!-- more -->

###安装nginx前的准备

为了确保在nginx中能够使用正则表达式进行更灵活的配置，安装nginx之前必须确定系统已经安装了PCRE(Perl Compatible Regular Expressions)包。也可以安装openssl。

<div>
{% codeblock lang:bash %}
#切换到PCRE的保存路径，我把它保存在/usr/local/pcre路径下
sudo mkdir /usr/local/pcre
cd /usr/local/pcre

#下载最新版的PCRE
wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.33.tar.gz

#解压
tar -zxvf pcre-8.33.tar.gz

#安装openssl
sudo apt-get install openssl
sudo apt-get install libssl-dev
{% endcodeblock %}
</div>

###安装nginx

<div>
{% codeblock lang:bash %}
#切换到nginx保存路径
mkdir /usr/local/www
mkdir /usr/local/www/nginx
cd /usr/local/www/nginx

#下载稳定版nginx
wget http://nginx.org/download/nginx-1.6.0.tar.gz

#解压
tar -zxvf nginx-1.6.0.tar.gz

#配置
cd nginx-1.6.0
./configure --prefix=/usr/local/www/nginx --with-pcre=/usr/local/pcre/pcre-8.33 --with-http_stub_status_module --with-http_gzip_static_module

#编译
make
make install
{% endcodeblock %}
</div>

安装完成后的提示信息：

<div>
{% img center /images/2014-6/tishi.jpg 提示信息 %}
</div>

研究了一下nginx帮助后发现，有-s参数可对nginx服务进行管理：
<div>
{% codeblock lang:bash %}
root@AY1405311210497418adZ:/usr/local/www/nginx# sbin/nginx -h
nginx version: nginx/1.6.0
Usage: nginx [-?hvVtq] [-s signal] [-c filename] [-p prefix] [-g directives]

Options:
  -?,-h         : this help
  -v            : show version and exit
  -V            : show version and configure options then exit
  -t            : test configuration and exit
  -q            : suppress non-error messages during configuration testing
  -s signal     : send signal to a master process: stop, quit, reopen, reload
  -p prefix     : set prefix path (default: /usr/local/www/nginx/)
  -c filename   : set configuration file (default: conf/nginx.conf)
  -g directives : set global directives out of configuration file

{% endcodeblock %}
</div>

因此执行以下命令对nginx重启：
<div>
{% codeblock lang:bash %}
root@AY1405311210497418adZ:/usr/local/www/nginx# sbin/nginx -s reload
{% endcodeblock %}
</div>










