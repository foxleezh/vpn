## 购买搬瓦工VPS

要搭建vpn首先得有一台国外的服务器，搬瓦工就是，官网 https://bandwagonhost.com/

进去后选一个你想要的套餐，点击Order KVM

![](https://upload-images.jianshu.io/upload_images/3387045-080ee489ab313d6c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

加入购物车
![](https://upload-images.jianshu.io/upload_images/3387045-c0bd942dca18c27c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击checkout进行下单
![](https://upload-images.jianshu.io/upload_images/3387045-55ede7e8348c6ef1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

填写基本信息，除了邮箱和密码要记住，其他随便填，支付方式选alipay，支付宝真牛逼！

![](https://upload-images.jianshu.io/upload_images/3387045-61a6cb3807d3b82e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

支付完成后点击右上角Client Area 登录

![](https://upload-images.jianshu.io/upload_images/3387045-62dc4f176d12fae8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

登录后选择myservice和control panel就可以进行后台管理了
![](https://upload-images.jianshu.io/upload_images/3387045-e7c928309c69e1a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/3387045-311c5de48192f4b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 进入后台管理界面

![](https://upload-images.jianshu.io/upload_images/3387045-82f6326f690efb1a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
服务器有很多系统可以给你选择，默认是centos，你可以点击Install new OS选择一个自己要的进行安装，我选择的是Ubuntu 16.04 x86_64,安装前要在Main Controls里的Actions栏点击stop关掉服务器，安装后会给你一个密码，这个密码可以记下来，
不过在邮件里也有，后面登录需要，安装需要一些时间，不断刷新就可以

![](https://upload-images.jianshu.io/upload_images/3387045-894eebcb4076b2ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


安装好系统后，最重要的是要进入命令行操作，我这里推荐两种方式

* 一是用interactive Shell,可视化命令行
* 二是用SSH自己连接

interactive Shell点击lunch后进入web界面，输入账号root，密码是你创建系统时给的密码
![](https://upload-images.jianshu.io/upload_images/3387045-00552e59ef57d0eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
进入后就是命令行了，跟在自己电脑上没有区别 <br>

另外一种方式就是用SSH自己连接，我比较推荐这种，我自己的电脑是Linux的，在安装git时自带安装了ssh，
所以直接输入命令 ssh root@ip地址 -p 端口地址即可，服务器ip地址和端口在Main Control里面有，比如
```shell
ssh root@192.168.1.1 -p 8080
```
回车后会让你记住密钥，选yes，然后输入密码即可进行服务器命令行，这里是真的就跟自己电脑的命令行一样了，复制粘贴完全没问题！


## 安装ShadowSocks

我这里以Ubuntu 16.04为例，首先安装pip
```shell
apt install python3-pip
```
然后安装github上最新的shadowsocks
```shell
pip3 install https://github.com/shadowsocks/shadowsocks/archive/master.zip
```
配置文件,首先新建一个目录,然后新建json文件
```shell
mkdir /etc/shadowsocks
vim /etc/shadowsocks/config.json
```
将下面的内容做适当修改复制到config.json文件中
```txt
{
    "server":"192.168.1.1",
    "server_port":443,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":600,
    "method":"rc4-md5",
    "fast_open": false
}
```
server是你服务器ip，server_port可以随便填，password自行设置，其他的尽量不要改，method是加密方式，如果你对ShadowSocks客户端配置比较熟悉，就会发现这里的配置跟它一模一样，是的，这里配置的东西就是客户端需要配置的 

配置好后运行
```shell
ssserver -c /etc/shadowsocks/config.json
```

这时我们在客户端按照上面的配置运行就可以了，如果不会客户端那支官网看看教程吧
https://github.com/shadowsocks/shadowsocks-qt5
https://github.com/shadowsocks/shadowsocks-windows

做一些小的优化，配置下开机启动，新建Shadowsocks管理文件
```shell
vim /etc/systemd/system/shadowsocks-server.service
```
将如下内容复制到管理文件
```txt
[Unit]
Description=Shadowsocks Server
After=network.target

[Service]
ExecStart=/usr/local/bin/ssserver -c /etc/shadowsocks/config.json
Restart=on-abort

[Install]
WantedBy=multi-user.target
```
配置开机启动
```shell
systemctl enable shadowsocks-server
```


## 安装mysql
```shell
apt-get install mysql-server
apt-get install mysql-client
apt-get install libmysqlclient-dev
```

安装成功后可以通过下面的命令测试是否安装成功：
```shell
netstat -tap | grep mysql
```
![](https://upload-images.jianshu.io/upload_images/3387045-37eff2af7ad9647c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


实现远程控制mysql

首先修改配置文件

```shell
vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

这个文件比较长，用/符号输入bind ,回车搜索，这个是vim的搜索命令
```shell
/bind
```
找到bind-address = 127.0.0.1这行，然后注释掉，保存退出

接下来进入数据库，123456是你的数据库密码
```shell
mysql -uroot -p123456
```

在mysql环境下执行授权命令(授权给远程任何电脑登录数据库)： 123456是你设置的授权访问密码，可以随便写，远程访问就用这个密码
```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
```

刷新配置信息
```sql
flush privileges;
```

Crlt+Z,退出mysql,重启mysql：记住这里不是mysql语句，是shell命令
```shell
service mysql restart
```

这样我们就可以用本地可视化工具远程操作数据库了，我用的navicat
![](https://upload-images.jianshu.io/upload_images/3387045-0863c6f440bd3771.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 安装tomcat

安装前要先装jdk
```shell
apt-get install openjdk-8-jdk
```

下载tomcat,[官网](http://tomcat.apache.org/download-80.cgi)
找到tar.gz点击下载，也可以把下载地址复制下来直接下载
```shell
wget http://mirrors.shu.edu.cn/apache/tomcat/tomcat-8/v8.5.31/bin/apache-tomcat-8.5.31.tar.gz
```

下载好后解压
```shell
tar xvf apache-tomcat-8.5.31.tar.gz
```

解压后将文件夹移动到usr/tomcat目录下
```shell
mv apache-tomcat-8.5.31 /usr/tomcat
```
修改目录权限
```shell
cd /usr
chmod 755 -R tomcat/
```
运行tomcat,进入bin目录
```shell
cd bin
sh startup.sh
```
![](https://upload-images.jianshu.io/upload_images/3387045-9731e36cc422a2f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现在在浏览器访问,你的ip:8080,比如19.56.86.195:8080,看到如下画面就表示服务器搭建成功
![](https://upload-images.jianshu.io/upload_images/3387045-e6b2bbe2e2647e13.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果你要自己写jar包然后访问接口，把写好的jar包放到tomcat/webapps目录下，重启tomcat即可
上传文件到服务器,443是端口，192.168.1.147是ip地址，super是服务器目录，意思是上传api.war到服务器的super目录下
```shell
scp -P 443 api.war root@192.168.1.147:/super
```


