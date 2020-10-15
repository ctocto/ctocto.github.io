---
title: Jenkins Docker 部署实践 
date: 2020-09-28 16:29:09
tags: [Jenkins,Docker,docker-compose]
categories: 饭碗(技术)
---


### Jenkins Docker 部署实践

##### docker-compose.yml
```docker
version: '3'                                    # 指定 docker-compose.yml 文件的写法格式
services:                                       # 多个容器集合
  docker_jenkins: 
    user: root                                  # 为了避免一些权限问题 在这我使用了root
    restart: always                             # 重启方式
    image: jenkins/jenkins:lts                  # 指定服务所使用的镜像 在这里我选择了 LTS (长期支持)
    container_name: jenkins                     # 容器名称
    ports:                                      # 对外暴露的端口定义
      - '7071:8080'
      - '50000:50000'
    volumes:                                    # 卷挂载路径
      - ./jenkins/jenkins_home/:/var/jenkins_home   # 这是我们一开始创建的目录挂载到容器内的jenkins_home目录
      - /var/run/docker.sock:/var/run/docker.sock
      - /usr/bin/docker:/usr/bin/docker                 # 这是为了我们可以在容器内使用docker命令
      - /usr/local/bin/docker-compose:/usr/local/bin/docker-compose     # 同样的这是为了使用docker-compose命令
```

##### run Create and start containers
```bash
docker-compose up -d
```
强制重新构建容器
```bash
docker-compose up -d --force-recreate
```

##### start
```bash
docker-compose start
```

##### stop
```bash
docker-compose stop
```

##### restart            
```bash
docker-compose restart            
```


##### dcoker 常用命令
```
# 查看镜像列表
docker images

# 删除镜像
docker rmi IMAGE ID
docker rmi -f IMAGE ID # 强制删除

# 删除未使用的镜像
docker image prune

# 列出所有在运行的容器信息
docker ps

# 查看所有运行或者不运行容器
docker ps -a

# 停止容器
docker stop CONTAINER ID

# 重启容器
docker restart CONTAINER ID

# 删除容器
docker rm CONTAINER ID

# 删除所有停止的容器
docker container prune

# 进入容器
docker exec -it jenkins /bin/bash
```

##### jenkins plugins

* Docker
* Docker Pipeline
* Localization: Chinese (Simplified)
* NodeJS Plugin
* Simple Theme Plugin


##### QA
* WorkflowScript: 3: Invalid agent type "docker" specified. Must be one of [any, label, none] @ line 3, column 9.

[You have to install 2 plugins: Docker plugin and Docker Pipeline. Hope that helps.](https://stackoverflow.com/questions/62253474/jenkins-invalid-agent-type-docker-specified-must-be-one-of-any-label-none)

* /var/jenkins_home/workspace/htzxh5-test@tmp/durable-8063fe32/script.sh: 1: /var/jenkins_home/workspace/htzxh5-test@tmp/durable-8063fe32/script.sh: docker: Permission denied

* /bin/sh: 12: sudo: not found

```
apt-get update && \
      apt-get -y install sudo
```

* [Clear Jenkins build history](https://superuser.com/questions/1418885/clear-jenkins-build-history-clear-build-yesterday)

Jenkins home page -> Manage Jenkins -> Script Console.
```
def jobName = "project-name"  
def job = Jenkins.instance.getItem(jobName)  
job.getBuilds().each { it.delete() }  
job.nextBuildNumber = 1   
job.save()
```

* docker in docker

[The simple way to run Docker-in-Docker for CI](https://tutorials.releaseworksacademy.com/learn/the-simple-way-to-run-docker-in-docker-for-ci)

* windows cannot touch '/var/jenkins_home/copy_reference_file.log': Permission denied

[The easy fix it to use the -u parameter. Keep in mind this will run as a root user (uid=0)](https://stackoverflow.com/questions/44065827/jenkins-wrong-volume-permissions)
```
docker run -u 0 -d -p 8080:8080 -p 50000:50000 -v /data/jenkins:/var/jenkins_home jenkins/jenkins:lts
```

* install huawei cloud OBS

```bash
cd /usr/local
wget https://obs-community.obs.cn-north-1.myhuaweicloud.com/obsutil/current/obsutil_linux_amd64.tar.gz
mkdir obsutil_linux_amd64
tar -xzvf obsutil_linux_amd64.tar.gz -C obsutil_linux_amd64 --strip-components 1
cd obsutil_linux_amd64
chmod 755 obsutil
ln -s /usr/local/obsutil_linux_amd64/obsutil /usr/bin/obsutil
```
