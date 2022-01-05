---
title: Linux crontab 定时任务-Shell脚本实现
date: 2022-01-05 10:59:00
tags: [Linux,crontab,Shell,定时任务]
categories: 饭碗(技术)
---

Linux crontab是用来定期执行程序的命令。

当安装完成操作系统之后，默认便会启动此任务调度命令。

crond 命令每分锺会定期检查是否有要执行的工作，如果有要执行的工作便会自动执行该工作。

**注意：** 新创建的 cron 任务，不会马上执行，至少要过 2 分钟后才可以，当然你可以重启 cron 来马上执行。

### [语法](https://www.runoob.com/linux/linux-comm-crontab.html)
```
crontab [ -u user ] file
```
或
```
crontab [ -u user ] { -l | -r | -e }
```

时间格式：
```
*    *    *    *    *
-    -    -    -    -
|    |    |    |    |
|    |    |    |    +----- 星期中星期几 (0 - 6) (星期天 为0)
|    |    |    +---------- 月份 (1 - 12) 
|    |    +--------------- 一个月中的第几天 (1 - 31)
|    +-------------------- 小时 (0 - 23)
+------------------------- 分钟 (0 - 59)
```

### 示例
每月末通知充值流量卡，利用企业微信机器人发送消息通知提醒。

```sh
# crontab_list.sh

#!/bin/sh

# 每月20号通知检查流量卡余额
0 10 20 * * sh +x /home/project/script/cellular_data.sh collect >> /home/project/script/cellular_data_collect.log 2>&1

# 每月25号通知检查流量卡余额
0 10 25 * * sh +x /home/project/script/cellular_data.sh collect >> /home/project/script/cellular_data_collect.log 2>&1

```
cellular_data_collect.log 日志文件
```sh
# cellular_data.sh

#!/bin/sh

curl 'https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=' \
-H 'Content-Type: application/json' \
-d '{
  "msgtype": "text",
  "text": {
    "content": "🙋‍♂️Hi，各位少侠、女侠上午好。马上月底了请检查各自绑定的流量卡是否还充足，请酌情处理加油⛽️（续费）。🚗💨",
    "mentioned_list":["@all"]
  }
}'
```

#### sh文件需要设置可执行权限

```bash
chmod 777
```

#### 生效 crontab_list中所有的任务

```bash
crontab ./crontab_list.sh
```

#### 查看任务

```bash
crontab -l
```

#### 新增任务
如：新增一个每月最后一天提醒的任务

```diff
# crontab_list.sh

#!/bin/sh

# 每月20号通知检查流量卡余额
0 10 20 * * sh +x /home/project/script/cellular_data.sh collect >> /home/project/script/cellular_data_collect.log 2>&1

# 每月25号通知检查流量卡余额
0 10 25 * * sh +x /home/project/script/cellular_data.sh collect >> /home/project/script/cellular_data_collect.log 2>&1


+ # 每月最后一天通知检查下月充值情况
+ 0 10 28-31 * * [[ "$(date --date=tomorrow +\%d)" == "01" ]] && sh +x /home/project/script/next_month.sh collect >> /home/project/script/cellular_data_collect.log 2>&1
```

```sh
# next_month.sh

#!/bin/sh

curl 'https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=' \
-H 'Content-Type: application/json' \
-d '{
  "msgtype": "text",
  "text": {
    "content": "🙋‍♂️Hi，各位少侠、女侠上午好。月底了请检查各自绑定的流量卡是否已为下月续费，如未续费请尽快处理。🚗💨",
    "mentioned_list":["@all"]
  }
}'
```




