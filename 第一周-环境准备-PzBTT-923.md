### **第一周环境准备任务**

**任务目标：**搭建linux+nginx+php-fqm+mysql

**最终目标：**能够运行php代码并且可以使用php连接mysql，成功执行mysql的语句。

**报告要求：**将整个环境的搭建过程进行详细记录，收集网络上的加固文章，学习加固技术，从而思考不加固可能存在的安全问题，对于加固的过程以及对于安全的思考都需要做详细的记录。

**拓展任务：**除了这个web环境还有其他的环境可以搭建，能力强者可以做更多的练习，比如：基于apache的环境、基于windows server 的iis 环境等。



#### 1 VMware Workstation安装CentOS系统

1.1 CentOS官网下载最新版本的IOS文件，目前最新版本为7.6.1810

![1564147232074](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564147232074.png)





1.2 在虚拟化软件VMware workstation中安装CentOS操作系统

![1564147853585](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564147853585.png)

![1564148332552](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564148332552.png)

![1564148384217](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564148384217.png)



1.3 创建虚拟机快照做备份

![1564148454085](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564148454085.png)



1.4 初始化配置

1. 4.1 IP地址设置

​	vi /etc/sysconfig/network-scripts/ifcfg-ens33

![1564148597965](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564148597965.png)

![1564413643074](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564413643074.png)



​	重启网卡，获取IP地址

![1564148950872](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564148950872.png)



1. 4.2 更换为aliyun源

  下载http://mirrors.aliyun.com/repo/Centos-7.repo 文件，替换系统/etc/yum.repos.d/CentOS-Base.repo文件。

yum clean all                          #清除yum缓存

yum makecache                    #重新生成yum缓存

yum update                           #升级所有包同时也升级软件和系统内核

![1564151035224](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564151035224.png)

![1564414499606](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564414499606.png)





1. 4.3 安装net-tools组件

​	因初始系统，默认不能使用ifconfig/netstat/route等命令，个人比较习惯用这些命令，所以安装net-tools组件

yum -y install net-tools

![1564414559881](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564414559881.png)



1. 4.4 安装wget

yum -y install wget

![1564414594526](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564414594526.png)



1. 4.5 创建虚拟机快照

   ![1564414848769](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564414848769.png)

   

#### 2 nginx安装

##### 2.1 下载nginx安装包

wget -c http://nginx.org/download/nginx-1.17.2.tar.gz

![1564508447414](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564508447414.png)



##### 2.2 安装编译环境以及依赖包

① gcc安装，nginx编译依赖gcc环境

yum -y gcc gcc-c++

![1564509157010](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564509157010.png)



② PCRE pcre-devel库安装，(Perl Compatible Regular Expressions)是一个Perl库，包括perl兼容的正则表达式库。ginx的http模块使用pcre来解析正则表达式。

yum -y install pcre pcre-devel

![1564509306285](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564509306285.png)



③ zlib安装，该库提供了很多种压缩和解压缩的方式，nginx使用zlib对http包的内容进行gzip。

yum -y install  zlib zlib-devel

![1564509351537](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564509351537.png)



④ openssl安装，一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及SSL协议，并提供丰富的应用程序供测试或其它目的使用。nginx不仅支持http协议，还支持https（即在ssl协议上传输http）。

yum -y install openssl openssl-devel

![1564509421534](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564509421534.png)



##### 2.2 解压源码压缩包

tar -zvxf nginx-1.17.2.tar.gz

![1564508530177](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564508530177.png)



##### 2.3 配置编译参数

cd nginx-1.17.2

./configure --prefix=/usr/local/nginx --with-http_ssl_module

![1564509634248](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564509634248.png)

![1564509595378](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564509595378.png)



##### 2.4 编译并安装

make && make install

![1564509768639](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564509768639.png)



##### 2.5 启动nginx

./nginx

![1564509982896](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564509982896.png)

![1564510311976](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564510311976.png)

有master、worker两个进程说明启动成功



##### 2.6 设置nginx开机启动

​	用yum命令安装会自动创建nginx.service文件，使用源码编译安装的，需要手动创建nginx.service服务文件。

开机没有登陆情况下就能运行的程序，存在系统服务（system）里： cd /lib/systemd/system

![1564540050412](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564540050412.png)

① 创建nginx.service文件

vi /lib/systemd/system/nginx.service

![1564540426774](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564540426774.png)



② 设置开机启动

systemctl enable nginx.service

![1564540726544](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564540726544.png)



##### 2.7 开放防火墙端口

![1564510583601](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564510583601.png)

开放80端口

![1564214334121](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564214334121.png)

使配置生效

![1564214349738](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564214349738.png)

查看当前开放的端口

![1564214395702](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564214395702.png)

![1564510615655](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564510615655.png)



#### 3 mysql安装

##### 3.1 下载mysql安装包

wget -c https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.27-el7-x86_64.tar.gz

![1564511455624](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564511455624.png)

![1564511477365](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564511477365.png)



##### 3.2 解压文件

 tar -zvxf mysql-5.7.27-el7-x86_64.tar.gz

![1564511746147](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564511746147.png)

将解压文件拷贝到/usr/local/mysql 目录

cp mysql-5.7.27-el7-x86_64 /usr/local/mysql

![1564511969908](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564511969908.png)



##### 3.3 添加系统mysql组和mysql用户

groupadd mysql

useradd -r -g mysql myslq

![1564511665467](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564511665467.png)



##### 3.4 安装mysql数据库

修改mysql目录拥有者为mysql用户

chown -R mysql:mysql mysql/

![1564512093411](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564512093411.png)



安装

/usr/local/mysql/bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data

![1564512274080](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564512274080.png)



3.5 配置mysql

配置my.cnf

vi /etc/my.cnf

![1564514013558](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564514013558.png)



添加开启启动

cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld

![1564513168757](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564513168757.png)



修改 vi /etc/init.d/mysqld

![1564513121302](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564513121302.png)

启动mysql

![1564514110006](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564514110006.png)

![1564514123184](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564514123184.png)



加入开机启动

![1564514279540](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564514279540.png)



创建软连接

![1564514620519](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564514620519.png)



修改初始化myslq密码

安装mysql的时候会生成一个随机的密码

![1564549253107](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564549253107.png)

修改root密码

ALTER USER 'root'@'localhost' IDENTIFIED BY 'Password@123';

![1564549316004](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564549316004.png)



#### 4 php

​	php-fqm，只用与PHP，是一个PHPFastCGI管理器，CGI(公共网关接口),是外部应用程序（CGI程序）与Web服务器之间的接口标准，是在CGI程序和Web服务器之间传递信息的过程。

##### 4.1 下载php安装包

wget -c https://www.php.net/distributions/php-7.3.7.tar.gz

![1564588937467](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564588937467.png)

![1564589044836](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564589044836.png)



##### 4.2 解压

​	tar -xvzf php-7.3.7.tar.gz

![1564216090757](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564216090757.png)



##### 4.3 安装依赖包

yum -y install libxml2 libxml2-devel

![1564589279946](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564589279946.png)

yum install libxml2-devel bzip2 bzip2-devel curl-devel libjpeg-devel libpng libpng-devel freetype-devel libxslt-devel libzip-devel -y

![1564589798386](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564589798386.png)



##### 4.4 参数配置

./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --with-fpm-user=mysql --with-fpm-group=mysql --with-curl --with-freetype-dir --with-gd --with-gettext --with-iconv-dir --with-kerberos --with-libdir=lib64 --with-libxml-dir --with-mysqli=mysqlnd --with-openssl --with-pcre-regex --with-pdo-mysql=mysqlnd --with-mysql=mysqlnd --with-pdo-sqlite --with-pear --with-png-dir --with-jpeg-dir --with-xmlrpc --with-xsl --with-zlib --with-bz2 --with-mhash --enable-fpm --enable-bcmath --enable-libxml --enable-inline-optimization --enable-mbregex --enable-mbstring --enable-opcache --enable-pcntl --enable-shmop --enable-soap --enable-sockets --enable-sysvsem --enable-sysvshm --enable-xml --enable-fpm

![1564589852066](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564589852066.png)

![1564590002010](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564590002010.png)



##### 4.5 安装

![1564334584845](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564334584845.png)

![1564590140261](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564590140261.png)

![1564591085147](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564591085147.png)



##### 4.6 配置

cp /root/php-7.3.7/php.ini-production /usr/local/php/etc/php.ini

cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf

cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf

![1564591485461](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564591485461.png)



测试php-fpm

![1564592150146](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564592150146.png)



拷贝启动文件

cp /root/php-7.3.7/sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm

chmod php-fpm start

![1564592476712](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564592476712.png)



查询启动状态

![1564592550340](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564592550340.png)



配置nginx可解析.php文件



![1564593209347](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564593209347.png)



重启nginx

![1564593567042](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564593567042.png)

测试

vi /usr/local/nginx/html/test.php

![1564593901266](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564593901266.png)

得到php配置说明nginx解析php成功

![1564593554292](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564593554292.png)



#### 5 php连接mysql的语句

测试代码

连接数据库

![1564594517565](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564594517565.png)![1564761950702](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564761950702.png)

![1564761970563](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564761970563.png)

![1564762169759](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564762169759.png)

![1564762199744](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564762199744.png)

![1564762215391](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564762215391.png)

#### 5 安全加固

​		在进行渗透测试的时候，攻击者一定会进行各种类型的扫描，扫描服务器对外开放的端口、服务以及相应服务的版本号，根据这些信息，攻击者能更有针对性的进行渗透，减少不必要的对外开放的端口、服务，屏蔽回显标识能避免暴露更多的信息给攻击者，增加攻击者的攻击成本，可以用以下的手段进行加固：

##### 5.1 操作系统加固

开启系统防火墙，只开放必要的端口，这里以我的实验环境为例子，对外只开放80、22端口；**

service firewalld start #开启防火墙

systemctl start firewalld.service #开机自动启动

service firewalld status #查看当前防火墙运行状态

firewall-cmd --list-all #查看防火墙状态

firewall-cmd --list-ports #查看当前开放的端口

firewall-cmd --list-service #查看当前开放的服务

firewall-cmd --reload #更新防火墙规则

firewall-cmd --remove-service=dhcpv6-client #阻止服务

firewall-cmd --remove-port=80/tcp #阻止端口

​			![1564763752434](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564763752434.png)

![1564763938844](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564763938844.png)

![1564763978004](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564763978004.png)

![1564764480751](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564764480751.png)



##### 5.1 nginx安全加固

**1 隐藏nginx版本号**

修改nginx配置文件

![1564819188113](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564819188113.png)

使用nmap进行验证

未修改前

![1564819271125](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564819271125.png)

修改后

![1564819237320](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564819237320.png)





2 过滤user-agent

​		user-agent 也即浏览器标识，每个正常的web请求都包含用户的浏览器信息，除非经过伪装，恶意扫描工具一般都会在user-agent里留下某些特征字眼，比如scan，nmap等。我们可以用正则匹配这些字眼，从而达到过滤的目的，请根据需要调整。

配置nginx.conf

![1564824749116](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564824749116.png)



配置前效果

![1564824804085](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564824804085.png)

配置后效果

![1564824840879](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564824840879.png)



3 封杀特定的http方法和行为

配置nginx.conf

![1564825009291](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564825009291.png)

![1564825647971](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564825647971.png)





##### 5.2 php安全加固

1 隐藏PHP版本号

​		PHP 配置默认允许服务器在 HTTP 响应头 `X-Powered-By` 中显示安装在服务器上的 PHP 版本，如下所示：

![1564821867370](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564821867370.png)

可以对PHP版本号进行隐藏，避免暴露已知版本存在的漏洞

修改php配置文件，把expose_php 设置成Off

vi /usr/local/php/etc/php.ini

![1564822271319](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564822271319.png)

重启php-fpm/nginx服务

service php-fpm restart

service nginx restart

![1564822554251](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564822554251.png)





##### 5.3 mysql安全加固

1 改变默认mysqlg管理员账号

​		系统 MySQL 的管理员名称是 root,而一般情况下,数据库管理员都没进行修改,这一定程度上对系统用户穷举的恶意行为提供了便利,此时修改为复杂的用户名,请不要在设定为 admin 或者 administraror 的形式,因为它们也在易猜的用户字典中。

![1564830656668](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564830656668.png)

![1564830814600](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564830814600.png)



2 用户目录权限设置（安装mysql时已经设置好）

限制其他用户对数据库文件的访问权限

![1564831222557](C:\Users\PzBTT\AppData\Roaming\Typora\typora-user-images\1564831222557.png)











