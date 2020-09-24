---
title: CentOS8 部署web服务：Nginx、MySQL、Node.js
date: 2020-09-24 10:46:09
tags: [服务器,CentOS,Nginx,MySQL,Node.js,nvm]
categories: 饭碗(技术)
---

一台Ali ECS cento8.2服务器, 开始部署服务

### 一、远程登录
ssh、...

### 二、安装环境

#### 安装 Git

```bash
yum install git
```

#### 安装nvm
```bash
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash

source ~/.bashrc
```

#### 安装Node.js
```bash
nvm install 14.12.0

node -v
```

#### 安装Nginx

要设置yum存储库，请创建/etc/yum.repos.d/nginx.repo 包含以下内容的文件 
```
vi /etc/yum.repos.d/nginx.repo
```
一下内容拷贝进nginx.repo 文件，并保存。
```
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
```

要安装nginx，请运行以下命令：
```
yum install nginx

nginx -v
```
常用命令
```bash
# 查看nginx.conf配置文件目录
nginx -t
```

#### 安装MySQL

```bash
# 下载mysql的repo源
wget https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-community-server-8.0.21-1.el8.x86_64.rpm #选择RPM包 https://dev.mysql.com/downloads/mysql/
# 安装源
rpm -ivh mysql-community-server-8.0.21-1.el8.x86_64.rpm --nodeps --force
# 安装mysql (可以不需要前两步，直接执行安装，但可能版本不是想要的。)
yum install mysql-server
# 如果安装过程中发生包冲突（... conflicts with file from package ...）,删除一个包，再尝试install
# 删除命令
yum -y remove 包名
# 检查安装结果
mysqladmin --version
# 初始化mysql
mysqld --initialize //创建数据文件目录和mysql系统数据库 产生随机root密码
# 启动mysql服务
systemctl start mysqld

# 查看随机root 密码
cat /var/log/mysqld.log | grep password 
# 或者
cat /var/log/mysql/mysqld.log | grep password 

# 安全设置
mysql_secure_installation

# 登录
mysql -uroot -p 

# 停止服务
systemctl stop mysqld 
# 重启服务
systemctl restart mysqld 
# 查看服务
systemctl status mysqld 
```
> [安全设置](https://ctocto.github.io/2020/09/24/MySql%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E5%AE%89%E5%85%A8%E8%AE%BE%E7%BD%AE/)
#### 常用命令
```bash
# 查看开机启动程序列表
systemctl list-unit-files
# 查看指定应用是否开启开机启动，如：mysql, nginx
systemctl list-unit-files | grep mysqld
systemctl list-unit-files | grep nginx
# 设置开机启动，如：mysql，nginx
systemctl enable mysqld.service
systemctl enable nginx
# 检查是否已安装程序，如：nginx
rpm -qa | grep -i nginx
```
