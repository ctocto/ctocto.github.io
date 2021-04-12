---
title: CentOS7 安装Docker、Docker Compose
date: 2021-04-12 17:46:09
tags: [服务器,CentOS,Docker,docker-compose]
categories: 饭碗(技术)
---

## 一、安装 Docker

### 1、安装所需的包。其中yum-utils保证yum-config-manager可用，并且使用device-mapper-persistent-data 和 lvm2作为存储驱动，为Docker提供存储能力。
```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```
### 2、设置一个稳定的仓库
```
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

### 3、安装最新版本的Docker CE和containerd，或者转到下一步安装特定版本
```
sudo yum install docker-ce docker-ce-cli containerd.io
```

### 4、启动Docker
```
// 开机自启
systemctl enable docker

systemctl start docker
```

### 5、查看版本
```
docker version
```

## 二、安装 Docker Compose

### 1、Curl下载Docker-Compose的二进制文件到/usr/local/bin目录
```
curl -L "https://github.com/docker/compose/releases/download/1.29.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

### 2、使二进制文件可执行
```
chmod +x /usr/local/bin/docker-compose
```

### 3、查看版本
```
docker-compose --version
```
