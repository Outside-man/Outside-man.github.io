---
layout:     post
title:      "minecraft开服实录"
date:       2017-12-22 12:00:00
author:     "yuhai"
tags:
    - minecraft
---

### 写在前面
Minecraft(简称Mc) 是一款从中学一直玩到大学都觉得非常好玩的游戏  
但是以前和小伙伴玩的时候是通过蛤蟆吃或者是本地建服然后端口开放出去。这些方法都是中学的时候一点点看着论坛摸索出来的。之后进了大学就都是在寝室里局域网play。  
这次有机会搞到了一台还不错的ubuntu的服务器，打算在上面建个服，然后放假了大家可以在家里玩。但是再liunx的服务器上，没有图形化界面 开服就没那么容易，就参考着资料试试看。

---

### 需要材料
1. jdk
2. minecraft_server.jar
3. forge-install.jar
4. forge-universal.jar  

### 搞起(以1.7.10为例)
java环境就不多说了。直接创个目录就把上面三个jar丢进去。

```
java -Xmx2048M -Xms2048M -XX:ParallelGCThreads=16 -jar minecraft_server.1.7.10.jar
```
> -Xmx最大内存 -Xms最小内存 -XX:ParallelGCThreads同时调用CPU数量，建议不设  

第一次运行的时候 会要求签署EULA文件。

```
eula=true
```
再次运行，这个时候一个**纯净服务器**就开起来了。
#### Forge
如果没有forge-install.jar、forge-universal.jar那就wget

```
wget http://files.minecraftforge.net/maven/net/minecraftforge/forge/1.7.10-10.13.2.1291/forge-1.7.10-10.13.2.1291-installer.jar
wget http://files.minecraftforge.net/maven/net/minecraftforge/forge/1.7.10-10.13.2.1291/forge-1.7.10-10.13.2.1291-universal.jar

```
有了之后就开始安装forge

```
java -jar forge-1.7.10-10.13.2.1291-installer.jar nogui --installServer
```
> 同目录下必须有minecraft_server.1.7.10.jar，注意版本号

启动forge服务器

```
java -jar forge-1.7.10-10.13.2.1291-universal.jar
```
可以装mod的服务器就配置好了（撒花
> 如果是盗版服的话 修改server.properties 里的 online-mode=false

#### 还有一点点
这个时候在目录下面有两个jar(forge-install.jar安装完毕后就可以删除了),强迫症不能忍啊，整到一个jar里面
```
mkdir tmp
cd tmp
tmp ../minecraft_server.1.7.10.jar
tmp ../forge-1.7.10-10.13.2.1291-universal.jar
```
> Archive: ../forge-1.7.10-10.13.2.1291-universal.jar   
replace META-INF/MANIFEST.MF? [y]es, [n]o, [A]ll, [N]one, [r]ename: A

```
zip -r ../Forge-minecraft_server.1.7.10.jar *
```
这样的话可以就把server和forge里的东西整一块,把tmp文件夹以及之前的jar删除即可。

```
完成之后的文件树

tree -L 1

├── banned-ips.json
├── banned-players.json
├── config
├── eula.txt
├── Forge-minecraft_server.1.7.10.jar
├── libraries
├── logs
├── mods
├── ops.json
├── server.properties
├── usercache.json
├── whitelist.json
└── world
```
打上mod，开始Mc之旅吧~
```
nohup java -Xmx2048M -Xms2048M -XX:ParallelGCThreads=16 -jar minecraft_server.1.7.10.jar &
```