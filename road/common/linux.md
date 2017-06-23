### 上传文件到服务器
SSH 方式 scp ./FILE ssh WHO@HOST:DIR
账号方式 scp ./FILE WHO@HOST:DIR

//安装nodejs
sudo apt-get install nodejs
sudo apt-get install npm
//升级
sudo npm install -g n 
sudo n stable

### ubuntu 防火墙
```shell
##查看防火墙状态
ufw status 
utw allow 22/tcp

sudo apt-get install ufw （一般Ubuntu已默认安装 ufw）

sudo ufw logging on 启动日志
sudo ufw logging off 关闭日志


##查看操作系统监听的情况

sudo bash -c “netstat -an | grep LISTEN | grep -v ^unix”
 
netstat -ntulp
sudo lsof -i -n -P
 
##默认常规设置
 
sudo ufw defualt deny|allow 
 
## deny 在拒绝大多数访问的基础上自定义开启一些通路（一般情况下推荐选择deny）
## allow在允许大多数访问的基础上自定义关闭一些通路
 
### 开启/关闭防火墙，重启ufw： 
sudo ufw disable, sudo ufw enable

sudo ufw enable|disable

### 日志状态

sudo ufw logging on|off

##许可或者屏蔽某些入埠的包 (可以在“status” 中查看到服务列表)。可以用“协议：端口”的方式指定一个存在于/etc/services中的服务名称，也可以通过包的meta-data。‘allow’ 参数将把条目加入 /etc/ufw/maps ，而 ‘deny’ 则相反。基本语法如下：

sudo ufw allow|deny [service]

##显示防火墙和端口的侦听状态，参见 /var/lib/ufw/maps。括号中的数字将不会被显示出来。

sudo ufw status
 
 
##一般用户，只需如下设置：

sudo apt-get install ufw #安装UFW防火墙

sudo ufw enable #开启防火墙，并在系统启动时自动开启

sudo ufw default deny #关闭所有外部对本机的访问，但本机访问外部正常。

##以上三条命令已经足够安全了，如果你需要开放某些服务，再按照如下提供的例子，使用 sudo ufw allow开启。

##允许 53 端口

sudo ufw allow 53

##禁用 53 端口

sudo ufw delete allow 53
sudo ufw delete limit 22
 
##允可 80 端口的tcp协议

sudo ufw allow 80/tcp
 
##许可多个端口（80,443）协议
 
##sudo ufw allow 80,443/tcp

##删除 80 端口的tcp协议许可

##sudo ufw delete allow 80/tcp
    
##许可一定范围端口 6881 – 6999 (torrent)
##sudo ufw allow 6881:6999/tcp
 
 
##查看应用程序列表 （用于许可和删除相应的应用程序端口）
 
sudo ufw app list

##增加许可规则 许可ssh 端口
 
sudo ufw allow ssh
 
##增加许可规则许可 smtp 端口

sudo ufw allow smtp

##删除许可规则 smtp 的端口

sudo ufw delete allow smtp

##增加许可规则 许可特定 IP

sudo ufw allow from 192.168.254.254

##删除上面的许可规则

sudo ufw delete allow from 192.168.254.254
 
##增加限制访问的许可规则 （同时允许ssh端口，在30秒内超过6次连接则拒绝此ip）
 
sudo ufw limit ssh
```
 
自定义应用程序的端口许可规则
 
将自定义的规则放到目录：/etc/ufw/application.d 里面

```java
    [Apache]  
    title=Web Server  
    description=Apache v2 is the next generation of the omnipresent Apache web server.  
    ports=80/tcp  
    
    [Apache Secure]  
    title=Web Server (HTTPS)  
    description=Apache v2 is the next generation of the omnipresent Apache web server.  
    ports=443/tcp  
    
    [Apache Full]  
    title=Web Server (HTTP,HTTPS)  
    description=Apache v2 is the next generation of the omnipresent Apache web server.  
    ports=80,443/tcp 
```

如果你想把ssh的流通端口改为8822，增加一个文件 “ssh-custom”, 在 /etc/ufw/applications.d/ssh-custom
```java
[SSH Custom]  
title= SSH Custom port  
description=OpenSSH Server Custom port  
ports=8822/tcp 
```

### linux文件覆盖只能用cp
```shell
cp -fr DIR/ ../NEW_DIR/  #f表示强制替换 R表示复制子目录
```