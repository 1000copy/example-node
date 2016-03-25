# example-node

一个简单的电商网站demo，使用了nodejs, express, mongodb, mongoose等功能模块。

实现功能：

1. 注册
2. 登录
3. 添加商品
4. 加入购物车和结算

使用说明：

1. 启动mongodb；

2. 启动项目: 在命令行窗口 cd 到项目目录，输入: node app.js

然后就可以通过 http://localhost:3000 访问了。

# 技术细节

## 上下文

需要测试 https://github.com/1000copy/example-node，一个简单的商城 (shop) 代码 。它有些错误，需要看mongodb内部的数据值才好troubleshooting 。

再次：因此想要连接支付宝的支付，我需要一个shop 。

## Install mongodb3.2.4

curl -O https://fastdl.mongodb.org/osx/mongodb-osx-x86_64-3.2.4.tgz
tar -zxvf mongodb-osx-x86_64-3.2.4.tgz

mkdir -p mongodb
cp -R -n mongodb-osx-x86_64-3.2.4/ mongodb

export PATH=<mongodb-install-directory>/bin:$PATH

##创建数据目录，修改权限以便mongod可以访问数据库目录：

mkdir -p /data/db 
sudo chown `id -u` /data/db

## run

    $mongodb

## 管理web GUI for mongodb

没有管理gui，要调试效果什么的都用代码就比较悲催。
我选择 mongo-express，一个node npm应用安装

    npm install -g mongo-express
    mongo-express 
    chrome to localhost:8081 // 默认端口 8081

登录时，使用admin，pass作为用户名和密码。可以通过
    
    mongo-express -u user -p password -d database

来修改密码。最后参数 database 不指定就是全部，指定的了话，就只管理指定数据库。

# session store

默认为memory store，会导致 Node.js server restart drops the sessions 的问题。调试代码会飞起来的麻烦，因此必须fix。

可以使用persist的存储。比如cookies。

    $ npm install cookie-session // 最好在package.json 内配置

代码放到app.js 内

    var cookieSession = require('cookie-session')

    app.use(cookieSession({
      name: 'session',////
      keys: ['key1', 'key2']
    }))



