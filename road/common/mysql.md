### mysql 配置远程可访问服务器
* 修改服务器鉴定地址，默认监听是(127.0.0.1),一般情况下配置是在etc/mysql/mysql.conf.d/修改bind-address= YOUR_HOST,监听的地址可以是局域网，也可以是外网IP
* 安全关闭MYSQL  ./mysqladmin -uroot -p shutdown
* 重启MYSQL ./usr/sbin/mysqld
* 添加远程访问用户及权限
```sql
    GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'%' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;
    GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'HOST' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;
    flush privileges;
```
* mysql字符集指定
```
    [client]
    default-character-set = utf8
    [mysqld]
    character-set-server = utf8
```