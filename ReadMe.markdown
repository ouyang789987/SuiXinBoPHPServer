# 随心播后台说明

## 1. 环境部署

### 1.1 搭建环境

PHP >= 5.4(但代码基本是按照5.1写法，所以稍作修改，PHP5.1也能运行), MySQL >= 5.5.3，Apache/Nginx服务器。

### 1.2 下载源码和文档

从https://github.com/zhaoyang21cn/SuiXinBoPHPServer下载得到zip文件，解压到服务器文档目录下
（比如apache DOCUMENT_ROOT），并且更改目录名为sxb。

### 1.3 修改配置

在lib/db/DBConfig.php填写数据库用户名和密码; 开通腾讯云COS服务，得到对应的APPID、SecretKey和SecretID。
然后填写deps/cos-php-sdk/Conf.php中对应的部分(不开通COS也能跑，只是客户端无法上传图片到COS)。

### 1.4 数据库建表建库

执行sxb_db.sql文件中的sql。

## 2. 目录结构
![参考directory.png](https://github.com/zhaoyang21cn/SuiXinBoPHPServer/blob/master/directory.png)

### 2.1 service 

服务层，也就是接口层，主要包括：直播服务、AV房间服务、COS服务。每个服务（亦即模块）下是各个子接口。详细可参看协议文档。

#### 2.1.1 直播服务

- 开始直播：数据库Replace一条记录，注意一个用户同一时间最多只能有一场直播；
- 直播结束：从数据库中删除记录；
- 直播列表：从数据库分页获取直播列表；
- 直播心跳包：客户端30秒发一次心跳包更新数据。

#### 2.1.2 Cos服务

获取Cos签名。

#### 2.1.3 AvRoom服务

获取Av房间号。


### 2.2 model 

数据层。

### 2.3 client-data 

客户端数据对象层，主要用于接收和返回给客户端的数据对象。

### 2.4 lib 

包括数据库和日志等库。

### 2.5 deps 

依赖库，主要是其他项目或者SDK，比如腾讯云COS SDK。

### 2.6 cron 
后台定时任务。清理90秒没有发心跳包的直播记录。可以crontab定时执行。
