---
layout: post
title: "在linux上安装ftp服务器"
date: 2014-06-03 14:40:20 +0800
comments: true
categories: backend
description: '在linux上安装ftp服务器'
keywords: 'ftp, vsftpd, filezilla'
---

###ftp

ftp是文件传输协议，用于在支持此协议的主机间传输文件。ftp使用tcp生成一个虚拟连接用于控制信息，然后再生成另一个tcp连接用于数据传输。

<!-- more -->

###安装

敲入以下命令安装ftp：

<div>
{% codeblock lang:bash %}
sudo apt-get install vsftpd
{% endcodeblock %}
</div>

安装成功后，系统会生成/srv/ftp目录。这个目录是匿名用户的根目录。

###配置

安装完成后，vsftpd的配置文件默认在/etc/vsftpd.conf，敲入：

<div>
{% codeblock lang:bash %}
sudo vi /etc/vsftpd.conf
{% endcodeblock %}
</div>

配置文件当中有很详细的说明，这是非常贴心的设计，忍不住赞一个。其中有一些是默认配置，非默认配置被注释起来了。以下是我设置的一些配置项：

<div>
{% codeblock lang:bash %}
# 让ftp成为独立的守护线程
listen=YES

# 允许本地用户登录
local_enable=YES

# 允许写权限
write_enable=YES

# umask设置为022
local_umask=022

# 向ftp客户端发送目录信息，比如当你进入/srv/ftp时发送这个目录名称
dirmessage_enable=YES

# 向ftp客户端发送信息时使用ftp客户端的本地时区显示时间
use_localtime=YES

# 启动日志功能
xferlog_enable=YES

# 日志文件地址
xferlog_file=/var/log/vsftpd.log

# 使用20端口监听
connect_from_port_20=YES

# 允许登录用户
chroot_list_enable=YES
# 登录用户列表地址
chroot_list_file=/etc/vsftpd.chroot_list

secure_chroot_dir=/var/run/vsftpd/empty
pam_service_name=vsftpd
rsa_cert_file=/etc/ssl/private/vsftpd.pem
{% endcodeblock %}
</div>

如果ftp服务器允许匿名用户访问并具有写权限的话，写权限也就是上传权限，配置下列项：

<div>
{% codeblock lang:bash %}
anonymous_enable=YES
anon_upload_enable=YES
anon_mkdir_write_enable=YES
{% endcodeblock %}
</div>

tips:每次修改了配置文件后记得执行service vsftpd restart使修改后的配置文件生效。

但是出于安全方面的考虑，不建议启动匿名访问，尤其是匿名用户的上传权限。你总不希望随便某个人都可以将他的文件上传到你的服务器上吧。于是我是关闭了匿名访问。

###设置登录用户

匿名用户登录到ftp服务器是不需要输入用户名和密码，但登录用户需要。登录用户是我们信任的用户，所以可以开放写权限。

我新建了一个ftpuser用户，根目录在/srv/user/ftp，属于ftpgroup用户组。

<div>
{% codeblock lang:bash %}
# 创建根目录
sudo mkdir -p /srv/user/ftp

# 新建用户组
sudo groupadd ftpgroup

# 新建用户ftpuser并设置其根目录为/srv/user/ftp
# 注：-g：用户所在的组 -d：表示创建用户自己的目录 -M：不建立默认的自己目录，也就是说在/home下没有自己的目录
sudo useradd -g ftpgroup -d /srv/user/ftp -M ftpuser

# 设置用户密码
passwd ftpuser

#编辑chroot_list文件，添加登录用户，每个用户占一行
sudo vi /etc/vsftpd.chroot_list
ftpuser

#启动ftp服务器
service vsftpd start

{% endcodeblock %}
</div>

现在用ftp客户端连接服务器，输入用户名ftpuser及密码，用户就可以登录到服务器的/srv/user/ftp目录中。用户可以下载此目录下的文件，但没有权限在此目录下新建文件，文件夹以及上传文件。回到/srv/user目录中，查看ftp目录的属性，如下所示:

<div>
{% codeblock lang:bash %}
drwxr-xr-x 3 root root 4096 Jun  3 13:54 ftp/
{% endcodeblock %}
</div>

你现在应该可以看出ftp目录是属于root用户，而且其他用户的权限只有r-x，缺少写权限。解决方法是在ftp目录下再新建一个upload目录，并将upload目录的权限改为drwxrwxrwx：

<div>
{% codeblock lang:bash %}
sudo mkdir -p /srv/user/ftp/upload
chmod 777 /srv/user/ftp/upload
{% endcodeblock %}
</div>

今后登录到服务器，切换到upload目录，就可以上传文件了。

ftp客户端推荐使用开源的FileZilla。

###启动／停止／重启ftp服务器

<div>
{% codeblock lang:bash %}
service vsftpd start
service vsftpd stop
service vsftpd restart
{% endcodeblock %}
</div>










