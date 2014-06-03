---
layout: post
title: "在nginx服务器上访问phpmyadmin"
date: 2014-06-03 23:04:11 +0800
comments: true
categories: backend
description: '在nginx服务器上访问phpmyadmin'
keywords: 'nginx, phpmyadmin, mysql, apache'
---

###phpmyadmin

phpmyadmin是由php脚本语言编写，作用是对mysql数据库进行可视化的管理。虽然我们可以通过系统命令行操作mysql，但命令行毕竟无法显示丰富的信息。在导入，导出大量数据的时候，phpmyadmin的优势就更明显。phpmyadmin通过网页的形式方便的实现了远程控制。

<!-- more -->

以前我在用apache服务器的时候，只需要运行下面两个命令，phpmyadmin就已经和apache集成好了：

<div>
{% codeblock lang:bash %}
sudo apt-get install lamp-server^ 
sudo apt-get install libapache2-mod-auth-mysql phpmyadmin 
{% endcodeblock %}
</div>

但是现在在nginx下，事情似乎不是这么简单了，因为在nginx下跑php脚本必须有fast-cgi的支持。

###安装＆配置

首先安装php：

<div>
{% codeblock lang:bash %}
sudo apt-get install php5-cli php5-cgi php5-fpm php5-mcrypt php5-mysql
{% endcodeblock %}
</div>

其中php5-fpm是fast-cgi进程管理器，安装好之后必须重启一下：

<div>
{% codeblock lang:bash %}
/etc/init.d/php5-fpm restart
{% endcodeblock %}
</div>

接着配置nginx，在/etc/nginx/conf.d目录下我已经创建了一个站点的配置文件site.conf，打开site.conf文件，添加index项，并添加对php文件的解析：

<div>
{% codeblock lang:bash %}
index index.php index.html index.htm;

location ~ \.php$ {
                fastcgi_split_path_info ^(.+\.php)(/.+)$;
        #       # NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
        #
        #       # With php5-cgi alone:
                fastcgi_pass 127.0.0.1:9000;
        #       # With php5-fpm:
        #       fastcgi_pass unix:/var/run/php5-fpm.sock;
                fastcgi_index index.php;
                include fastcgi_params;
        }
{% endcodeblock %}
</div>

输入service nginx restart重启nginx。此时在浏览器中输入site/phpmyadmin应该能出现phpmyadmin首页，但在我这里失败了。后来我输入site/phpmyadmin/index.php竟然成功了。到目前为止我还不明白问题出在哪，今后只好每次都输入index.php后缀了。

参考博文：[http://hi.baidu.com/sudoer/item/479347788e7364376e29f6f8](http://hi.baidu.com/sudoer/item/479347788e7364376e29f6f8)


















