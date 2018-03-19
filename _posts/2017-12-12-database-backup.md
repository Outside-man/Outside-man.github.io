---
layout:     post
title:      "在linux系统自动备份数据库"
date:       2017-12-12 12:00:00
author:     "yuhai"
tags:
    - 服务器
    - Linux
---

### 背景交代
在给学校做一些项目的时候，会遇到学校工作人员误操作，导致数据库数据损坏。这时候我想阿里云的服务器肯定有mysql服务器的备份功能，然而要钱。  
我就想自己实现自动备份  
其实就是把备份命令写下来然后定时执行就行了。
##### 步骤
1. 安装crontabs
2. 写好sh文件
3. 创建定时任务

---

## 尝试开始
### 1. crontabs  

学校用了新的服务器 很多没有配置

```
apt-get install cron
```
惊奇发现 阿里云已经有了crontabs
> cron is already the newest version (3.0pl1-128ubuntu2).
  
### 2. sh文件  

```
backupdir=/home/backup/backup
#定义变量backupdir  备份数据之后放到哪里

time='date +%Y%m%d%H'
#定义时间变量

mysqldump 数据库名 > $backupdir/备份名_$(date +%Y_%m_%d_%H%M%S).sql
#通过mysqldump指令指定数据库备份，通过管道备份为backupdir下的 以备份名_时间命名的.sql文件

find $backupdir -name "sztw_*.sql" -type f -mtime +30 -exec rm {}\; > /dev/null 2>&1
#find $backupdir -name "sztw_*.sql" -type f -mtime +30 -exec rm {}\;
#找到打包文件夹下面的备份文件 类型普通文件，时间超过30日的删除
#> /dev/null 2>&1 
#这个是让如果有输出指令不要打印
```
写完sh 别忘了把文件权限为可执行chmod u+x xxxxx.sh  
然后尝试 ./xxxxx.sh 执行一发 
```
vim /etc/mysql/my.cnf
#里的末尾添加
[mysqldump]
user='username'
password='password'
#这样就可以在备份的时候自动登入数据库账号
```
### 3.创建定时任务

```
crontab -e

#进入到任务编辑文件 添加任务
0 0 * * * /xxxxx.sh  #所在文件
#表示每天的0:00执行sh
```
crontab 中前面五个值分别表示分钟、小时、几号、月份、周几  
(*)表示所有可能  
(,)逗号隔开几个表示列表范围 如1,3,5  
(-)表示整数范围 如2-6  
(/)表示时间频率 0-23/2 每两小时一次 */10每十分钟一次  

完成后重启下crontabs服务  

```
service cron restart
```


到此大功告成