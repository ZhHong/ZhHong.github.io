## pm2
学习这个工具的主要目的就是为了nodejs能稳定运行。目前就只是简单的了解其使用和配置。[PM2](https://github.com/Unitech/PM2/)
### 安装
* node环境下只需要
```shell
>>>npm install pm2 -g
```
* (Windows)常用的命令
```shell
>>>pm2 ls #查看现在所有正在跑的线程
>>>pm2 start app.config.json #从配置中加载需要启动的程序，并启动
>>>pm2 stop id/name/`all` #停止对应的线程
>>>pm2 delete id/name/`all` #移除相应的启动配置
>>>pm2 m #监视器环境
```
* json配置启动程序
```java script
exports ={
    apps:[
        {
            name        :'Name',            //应用名称
            script      :'/WHERE/WHAT.js',  //应用启动脚本
            args        :'XXX',             //启动参数
            interpreter :'node',            //解释器
        }
    ]
}
```
* 更多配置信息请参考[PM2 Environment management](http://pm2.keymetrics.io/docs/usage/environment/)