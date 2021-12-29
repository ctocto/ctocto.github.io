---
title: Consul 服务注销
date: 2021-12-29 11:25:00
tags: [consul,service,微服务,注册中心,注销,删除]
categories: 饭碗(技术)
---

### 方式一：CI 命令

**`命令更好用，api 有时调用没有反应`**

docker容器部署consul服务，进入容器里边执行。
```sh
consul services deregister -id=web
```
![image](https://user-images.githubusercontent.com/16641120/147624403-fccfae09-249e-4f4a-8cb3-2d65d668551e.png)

> https://www.consul.io/commands/services/deregister

### 方式二：Api
```
curl \
    --request PUT \
    http://127.0.0.1:8500/v1/agent/service/deregister/my-service-id

```
> https://www.consul.io/api-docs/agent/service#deregister-service
