## ������߹�VPS

Ҫ�vpn���ȵ���һ̨����ķ����������߹����ǣ����� https://bandwagonhost.com/

��ȥ��ѡһ������Ҫ���ײͣ����Order KVM

![](https://upload-images.jianshu.io/upload_images/3387045-080ee489ab313d6c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

���빺�ﳵ
![](https://upload-images.jianshu.io/upload_images/3387045-c0bd942dca18c27c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

���checkout�����µ�
![](https://upload-images.jianshu.io/upload_images/3387045-55ede7e8348c6ef1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

��д������Ϣ���������������Ҫ��ס����������֧����ʽѡalipay��֧������ţ�ƣ�

![](https://upload-images.jianshu.io/upload_images/3387045-61a6cb3807d3b82e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

֧����ɺ������Ͻ�Client Area ��¼

![](https://upload-images.jianshu.io/upload_images/3387045-62dc4f176d12fae8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

��¼��ѡ��myservice��control panel�Ϳ��Խ��к�̨������
![](https://upload-images.jianshu.io/upload_images/3387045-e7c928309c69e1a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](https://upload-images.jianshu.io/upload_images/3387045-311c5de48192f4b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## �����̨�������

![](https://upload-images.jianshu.io/upload_images/3387045-82f6326f690efb1a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
�������кܶ�ϵͳ���Ը���ѡ��Ĭ����centos������Ե��Install new OSѡ��һ���Լ�Ҫ�Ľ��а�װ����ѡ�����Ubuntu 16.04 x86_64,��װǰҪ��Main Controls���Actions�����stop�ص�����������װ������һ�����룬���������Լ�������
�������ʼ���Ҳ�У������¼��Ҫ����װ��ҪһЩʱ�䣬����ˢ�¾Ϳ���

![](https://upload-images.jianshu.io/upload_images/3387045-894eebcb4076b2ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


��װ��ϵͳ������Ҫ����Ҫ���������в������������Ƽ����ַ�ʽ

* һ����interactive Shell,���ӻ�������
* ������SSH�Լ�����

interactive Shell���lunch�����web���棬�����˺�root���������㴴��ϵͳʱ��������
![](https://upload-images.jianshu.io/upload_images/3387045-00552e59ef57d0eb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
���������������ˣ������Լ�������û������ <br>

����һ�ַ�ʽ������SSH�Լ����ӣ��ұȽ��Ƽ����֣����Լ��ĵ�����Linux�ģ��ڰ�װgitʱ�Դ���װ��ssh��
����ֱ���������� ssh root@ip��ַ -p �˿ڵ�ַ���ɣ�������ip��ַ�Ͷ˿���Main Control�����У�����
```shell
ssh root@192.168.1.1 -p 8080
```
�س���������ס��Կ��ѡyes��Ȼ���������뼴�ɽ��з����������У���������ľ͸��Լ����Ե�������һ���ˣ�����ճ����ȫû���⣡


## ��װShadowSocks

��������Ubuntu 16.04Ϊ�������Ȱ�װpip
```shell
apt install python3-pip
```
Ȼ��װgithub�����µ�shadowsocks
```shell
pip3 install https://github.com/shadowsocks/shadowsocks/archive/master.zip
```
�����ļ�,�����½�һ��Ŀ¼,Ȼ���½�json�ļ�
```shell
mkdir /etc/shadowsocks
vim /etc/shadowsocks/config.json
```
��������������ʵ��޸ĸ��Ƶ�config.json�ļ���
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
server���������ip��server_port��������password�������ã������ľ�����Ҫ�ģ�method�Ǽ��ܷ�ʽ��������ShadowSocks�ͻ������ñȽ���Ϥ���ͻᷢ����������ø���һģһ�����ǵģ��������õĶ������ǿͻ�����Ҫ���õ� 

���úú�����
```shell
ssserver -c /etc/shadowsocks/config.json
```

��ʱ�����ڿͻ��˰���������������оͿ����ˣ��������ͻ�����֧���������̳̰�
https://github.com/shadowsocks/shadowsocks-qt5
https://github.com/shadowsocks/shadowsocks-windows

��һЩС���Ż��������¿����������½�Shadowsocks�����ļ�
```shell
vim /etc/systemd/system/shadowsocks-server.service
```
���������ݸ��Ƶ������ļ�
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
���ÿ�������
```shell
systemctl enable shadowsocks-server
```


## ��װmysql
```shell
apt-get install mysql-server
apt-get install mysql-client
apt-get install libmysqlclient-dev
```

��װ�ɹ������ͨ���������������Ƿ�װ�ɹ���
```shell
netstat -tap | grep mysql
```
![](https://upload-images.jianshu.io/upload_images/3387045-37eff2af7ad9647c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


ʵ��Զ�̿���mysql

�����޸������ļ�

```shell
vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

����ļ��Ƚϳ�����/��������bind ,�س������������vim����������
```shell
/bind
```
�ҵ�bind-address = 127.0.0.1���У�Ȼ��ע�͵��������˳�

�������������ݿ⣬123456��������ݿ�����
```shell
mysql -uroot -p123456
```

��mysql������ִ����Ȩ����(��Ȩ��Զ���κε��Ե�¼���ݿ�)�� 123456�������õ���Ȩ�������룬�������д��Զ�̷��ʾ����������
```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
```

ˢ��������Ϣ
```sql
flush privileges;
```

Crlt+Z,�˳�mysql,����mysql����ס���ﲻ��mysql��䣬��shell����
```shell
service mysql restart
```

�������ǾͿ����ñ��ؿ��ӻ�����Զ�̲������ݿ��ˣ����õ�navicat
![](https://upload-images.jianshu.io/upload_images/3387045-0863c6f440bd3771.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## ��װtomcat

��װǰҪ��װjdk
```shell
apt-get install openjdk-8-jdk
```

����tomcat,[����](http://tomcat.apache.org/download-80.cgi)
�ҵ�tar.gz������أ�Ҳ���԰����ص�ַ��������ֱ������
```shell
wget http://mirrors.shu.edu.cn/apache/tomcat/tomcat-8/v8.5.31/bin/apache-tomcat-8.5.31.tar.gz
```

���غú��ѹ
```shell
tar xvf apache-tomcat-8.5.31.tar.gz
```

��ѹ���ļ����ƶ���usr/tomcatĿ¼��
```shell
mv apache-tomcat-8.5.31 /usr/tomcat
```
�޸�Ŀ¼Ȩ��
```shell
cd /usr
chmod 755 -R tomcat/
```
����tomcat,����binĿ¼
```shell
cd bin
sh startup.sh
```
![](https://upload-images.jianshu.io/upload_images/3387045-9731e36cc422a2f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

���������������,���ip:8080,����19.56.86.195:8080,�������»���ͱ�ʾ��������ɹ�
![](https://upload-images.jianshu.io/upload_images/3387045-e6b2bbe2e2647e13.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

�����Ҫ�Լ�дjar��Ȼ����ʽӿڣ���д�õ�jar���ŵ�tomcat/webappsĿ¼�£�����tomcat����
�ϴ��ļ���������,443�Ƕ˿ڣ�192.168.1.147��ip��ַ��super�Ƿ�����Ŀ¼����˼���ϴ�api.war����������superĿ¼��
```shell
scp -P 443 api.war root@192.168.1.147:/super
```


