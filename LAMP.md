# 实现LAMP架构 

php配置 
php：脚本语言解释器 
配置文件：/etc/php.ini,  /etc/php.d/*.ini  
配置文件在php解释器启动时被读取 
对配置文件的修改生效方法  
       Modules：重启httpd服务  
       FastCGI：重启php-fpm服务 
    /etc/php.ini配置文件格式：  
            [foo]：Section Header  
            directive = value   
             注释符：较新的版本中，已经完全使用;进行注释  
            #：纯粹的注释信息  
           ;：用于注释可启用的directive 



php配置 
php.ini的核心配置选项文档：  
 http://php.net/manual/zh/ini.core.php 
php.ini配置选项列表：  
http://php.net/manual/zh/ini.list.php 
 
php语言格式
 <?php 
 ...php code... 
?> 

Php使用PDO(PHP Data Object)扩展连接数据库 
使用pdo扩展连接数据库的测试代码1 
<?php 
$dsn='mysql:host=mysqlhost;dbname=test'; 
$username=‘root'; 
$passwd=‘magedu'; 
$dbh=new PDO($dsn,$username,$passwd); 
var_dump($dbh); ?> 
 使用pdo扩展连接数据库的测试代码2 
<?php 
try { 
$user=‘root'; 
$pass=‘magedu'; 
$dbh = new PDO('mysql:host=mysqlhost;dbname=mysql’, $user, $pass); 
foreach($dbh->query('SELECT user,host from user') as $row) { 
print_r($row); 
} 
$dbh = null; 
} catch (PDOException $e) { 
print "Error!: " . $e->getMessage() . "<br/>"; 
die(); 
} 
?> 
Php使用mysqli扩展连接数据库 
Php使用mysqli扩展连接数据库的测试代码 
<?php 
$mysqli=new mysqli("mysqlserver",“username",“password"); 
if(mysqli_connect_errno()){  
      echo "Failure";  
      $mysqli=null;  
      exit; 
} 
echo “OK"; 
$mysqli->close(); 
?> 


实验
1安装数据库  启动  (27)
yuminstall mariadb-server
2  建立账户   用来让别的主机连接
grant all on *.* to test@'192.168.34.%' idenfified by '123456';
3 在另一个主机上安装  (7)
yum install php-mysql
4  写测试 /var/www/hmtl 下
vim pdo.php
<?php 
$mysqli=new mysqli("192.168.34.5",“test",“123456"); 
if(mysqli_connect_errno()){  
      echo "Failure";  
      $mysqli=null;  
      exit; 
} 
echo "OK"; 
$mysqli->close(); 
?> 
Php使用PDO连接
vim pdo2.php
<?php 
try { 
$user='test'; 
$pass='123456'; 
$dbh = new PDO('mysql:host=192.168.34.5;dbname=mysql’, $user, $pass); 
foreach($dbh->query('SELECT user,host from user') as $row) { 
print_r($row); 
} 
$dbh = null; 
} catch (PDOException $e) { 
print "Error!: " . $e->getMessage() . "<br/>"; 
die(); 
} 
?> 
--------------------------------------------------------------------------------
常见LAMP应用 
PhpMyAdmin是一个以PHP为基础，以Web-Base方式架构在网站主机上的 MySQL的数据库管理工具，让管理者可用Web接口管理MySQL数据库 
布署phpMyadmin 
1安装相应的包 启动服务
yum -y install  httpd mariadb-server php php-mysql 
systemctl start httpd 
systemctl start mariadb 
mysql_secure_installation 
2下载：https://www.phpmyadmin.net/downloads/ 
3 解包 
tar xvf phpMyAdmin-4.0.10.20-all-languages.tar.xz -C /var/www/html 
4cd phpMyAdmin-4.0.10.20-all-languages
5 改配置名字
cp config.sample.inc.php  config.inc.php 
6 可以改一下口令 
vim  config.inc.php 
7给mysql配置口令 
跑脚本 mysql_secure_installation
如果找不到路径就软连接路径
ln -s  /data/mysql/mysql.sock /tmp/mysql.sock
8安装包
yum install php-mysql
yum -y install php-mbstring 
9启动 systemctl reload httpd 

WordPress是一种使用PHP语言开发的博客平台，用户可以在支持PHP和MySQL 数据库的服务器上架设属于自己的网站。也可把 WordPress当作一个内容管理系 统（CMS）来使用 
布署wordpress 
下载地址：    
教室：ftp://172.16.0.1/pub/Sources/sources/httpd/    
官网：https://cn.wordpress.org/ 
解压缩WordPress博客程序到网页站点目录下    /var/ww/hmtl
unzip wordpress-4.3.1-zh_CN.zip 
cd worepress/   
改一下名字
cp wp-config-sample.php  wp-config.php
vim wp-config.php
修改账号和密码 主机ip
新建wpdb库和wpuser用户    （之前就创建好的 
mysql> create database wpdb;    
mysql> grant all privileges on wpdb.* to wpuser@'%' identified by "wppass"; 
打开http://webserver/wordpress进行页面安装 
注意wordpress目录权限 Setfacl –R –m u:apache:rwx wordpress 
Crossday Discuz! Board（简称 Discuz!）是一套通用的社区论坛软件系统。自 2001年6月面世以来，是全球成熟度最高、覆盖率最大的论坛软件系统之一。 2010年8月23日，与腾讯达成收购协议 
准备数据库  创建用户和口令 跑脚本 
mysql_secure_installation

1 下载安装包
2 解压缩 unzip d
3 cd upload/
4  赋予权限
setfacl -R -m u:apache:rwx upload/
5  安装 进入网页
6 装完以后取消权限

ECShop是一款B2C独立网店系统，适合企业及个人快速构建个性化网上商店。 系统是基于PHP语言及MYSQL数据库构架开发的跨平台开源程序。2006年6月， ECShop推出第一个版本1.0 

配置fastcgi 
php配置 
   配置文件：/etc/php.ini，/etc/php.d/*.ini 
           Module下，重启Httpd服务 
            FastCGI模式下，重启php-fpm服务 
配置文件格式                               
     配置文件格式：[foo]:Section Header     
                                    Directive=value 
     注释符：#  纯粹的注释信息      
                    ;   用于注释可启动的指令    
            说明：在较新的版本中，已经完全使用”;”进行注释 
php.ini核心配置的详细说明： http://php.net/manual/zh/ini.core.php 
Php.ini配置选项列表： http://php.net/manual/zh/ini.list.php 

fcgi服务配置文件：/etc/php-fpm.conf,  /etc/php-fpm.d/*.conf 
连接池： 
       pm = static|dynamic   
              static：固定数量的子进程；pm.max_children   
              dynamic：子进程数量以动态模式管理   
                           pm.max_children    
                            pm.start_servers    
                           pm.min_spare_servers    
                           pm.max_spare_servers    
                           pm.max_requests = 500   
确保运行php-fpm进程的用户对session目录有读写权限  
          mkdir  /var/lib/php/session  
         chown apache.apache /var/lib/php/session  

 (1) 配置httpd，添加/etc/httpd/conf.d/fcgi.conf配置文件，内容类似   DirectoryIndex index.php 
ProxyRequests Off 
ProxyPassMatch ^/(.*\.php)$  fcgi://127.0.0.1:9000/var/www/html/$1  
注意：在HTTPD服务器上必须启用proxy_fcgi_module模块，充当PHP客户端 
httpd –M |grep fcgi 
cat  /etc/httpd/conf.modules.d/00-proxy.conf 
 2) 虚拟主机配置    
vim /etc/httpd/conf.d/vhosts.conf 
DirectoryIndex index.php 
<VirtualHost *:80>  
     ServerName www.b.net  
     DocumentRoot /apps/vhosts/b.net  
     ProxyRequests Off  
     ProxyPassMatch ^/(.*\.php)$     fcgi://127.0.0.1:9000/apps/vhosts/b.net/$1  
     <Directory "/apps/vhosts/b.net">   
              Options None   
               AllowOverride None   
                Require all granted  
      </Directory>  
</VirtualHost> 

过程 
1在7上安装yum  install  httpd
配置httpd，添加/etc/httpd/conf.d/fcgi.conf配置文件，内容类似 
DirectoryIndex index.php 
ProxyRequests Off 
ProxyPassMatch ^/(.*\.php)$  fcgi://192.168.34.17:9000/var/www/html/$1 
注意：在HTTPD服务器上必须启用proxy_fcgi_module模块，充当PHP客户端 
httpd –M |grep fcgi 
cat  /etc/httpd/conf.modules.d/00-proxy.conf 
安装wordpress包  解包
2 在17上 安装yum install php-fpm  php-mysql
vim /etc/php-fpm.conf.d/www.cof
改端口
listen =9000
#listen.allowed_clients =172.0.0.1  或者是改ip
如果没有html 目录 mkdir /data/html    
启动服务systemctl restart php-fpm.service
查看端口是否开启
安装wordpress包
解包
进入目录
cp wp-config-sample.php  wp-config.php
vim wp-config.php
修改账号和密码 主机ip
新建wpdb库和wpuser用户    （之前就创建好的 ）
3 在27上安装yum install mariadb （ 数据库一般是独立的）
创建数据库  用户  给17用

--------------------------------------------------------------------------------
LAMP 
LAMP 
httpd：接收用户的web请求；静态资源则直接响应；动态资源为php脚本， 对此类资源的请求将交由php来运行 
php：运行php程序 
MariaDB：数据管理系统 
http与php结合的方式  
    CGI   
    FastCGI   
    modules (将php编译成为httpd的模块,默认方式)      
         MPM:  
              prefork: libphp5.so  
              event, worker: libphp5-zts.so 
快速部署LAMP 
CentOS 7: 
          Modules：httpd, php, php-mysql, mariadb-server 
          FastCGI：httpd, php-fpm, php-mysql, mariadb-server 
CentOS 6： 
Modules:httpd, php, php-mysql, mysql-server 
FastCGI:默认不支持 
安装LAMP 
CentOS 6:   
yum install httpd, php, mysql-server, php-mysql  
service httpd  start  s
ervice  mysqld  start 
CentOS 7:   yum install httpd, php, php-mysql, mariadb-server  
systemctl  start  httpd.service  
systemctl  start  mariadb.service 
注意：要使用prefork模型 
CentOS7编译Php-xcache加速访问 
官网：http://xcache.lighttpd.net/wiki/ReleaseArchive 
  安装方法  rpm包：来自epel源  
编译安装 
编译安装 
  yum -y install php-devel 
 下载并解压缩xcache-3.2.0.tar.bz2 
  phpize  生成编译环境 
  cd xcache-3.2.0 
 ./configure --enable-xcache  --with-php-config=/usr/bin/php-config 
  make && make install  
  cp xcache.ini  /etc/php.d/ 
  systemctl restart httpd.service 
php 
httpd+php结合的方式： 
module: php 
fastcgi : php-fpm   
php-fpm： 
CentOS 6：  PHP-5.3.2之前：默认不支持fpm机制；需要自行打补丁并编译安装  
httpd-2.2：默认不支持fcgi协议，需要自行编译此模块  
解决方案：编译安装httpd-2.4, php-5.3.3+ 
CentOS 7：  
httpd-2.4：rpm包默认编译支持fcgi模块  
php-fpm包：专用于将php运行于fpm模式   
--------------------------------------------------------------------------------
实验
实现LAMP 架构
1(7) 安装包 (基于模块方式）
yum install  httpd php php-mysql    
2(27) 在另一个主机上安装好数据库并且在数据库上创建一个用户和创建一个数据库
grant all on *.* to test@'192.168.34.%' identified by  '123456';
crreate tester 
3(7) 传入WordPress  解压到 /var.www.html 
或者是解压完 把目录下的数据拷贝到/var/ww/html
cd /wordpress/
cp -av . /var/www/html/
进入目录 改一下目录名称
mv wp-config-sample.php wp-config.php
修改这个文件 把27上的数据库的信息写入
vim  wp-config.php
重启服务进入网页访问

检测一下性能
用第三个主机
ab -c 10 -n 200 http://192.168.34.5/

4 (7)
安装加速器
yum  install php-xcache
重启http服务
 再一次检测一下性能 看看有没有提升

源码编译php-xcache 
tar xvf xcache-3.2.0.tar.za
cd 进去  cd xcache-3.2.0 
安装编译包组
yum groupinstall "development tools"  -y
 yum -y install php-devel 
cat INSTALL   查看编译 
 phpize   生成编译环境     
 ./configure --enable-xcache  --with-php-config=/usr/bin/php-config 
  make && make install  
  cp xcache.ini  /etc/php.d/ 
  systemctl restart httpd.service 
--------------------------------------------------------------------------------
 源码编译LAMP

CentOS 7安装LAMP(PHP-FPM模式) 
安装PHP-FPM   
        首先要卸载PHP  
        yum install php-fpm  
查看php-fpm所对应的配置文件   
 rpm -ql  php-fpm    
/usr/lib/systemd/system/php-fpm.service    
/etc/logrotate.d/php-fpm    
/etc/php-fpm.conf    
/etc/php-fpm.d   
 /etc/php-fpm.d/www.conf   
 /etc/sysconfig/php-fpm    
/run/php-fpm 
PHP-FPM常见配置   
daemonize = no  //是否将程序运行在后台   
listen = 127.0.0.1:9000 //FPM 监听地址  
listen.backlog = -1 //等待队列的长度 -1表示无限制   
listen.allowed_clients = 127.0.0.1  //仅允许哪些主机访问   
pm = dynamic //PM是动态运行还是静态运行            
       //static 固定数量的子进程，pm.max_childen           
        //dynamic子进程数据以动态模式管理   
pm.start_servers   
pm.min_spare_servers   
pm.max_spare_servers   
pm.max_requests = 500 
php_value[session.save_handler] = files  
php_value[session.save_path] = /var/lib/php/session   
//设置session存放位置 
 
启动PHP-FPM    s
ystemctl start php-fpm 

安装httpd包   
yum install httpd 
查看Httpd mod_fcgi模块是否加载 
 httpd -M | grep fcgi  
proxy_fcgi_module (shared) 
添加FCGI的配置文件     
DirectoryIndex index.php     
ProxyRequests off  //是否开启正向代理     
ProxyPassMatch ^/(.*\.php)$   fcgi://127.0.0.1:9000/var/www/html/$1  
//开启FCGI反向代理,//前面的/相对于后面的/var/www/html而言，后面的$1 是指前面的/(.*\.php)  
重启Httpd：systemctl start httpd 
在centos7上编译安装LAMP： 
mairadb：通用二进制格式，mariadb-5.5.56 
httpd：编译安装，httpd-2.4.25 
php5：编译安装，php-5.6.30 
phpMyAdmin：安装phpMyAdmin-4.4.15.10-all-languages 
Xcache：编译安装xcache-3.2.0 
 php5.4依赖于mariadb-devel包 
顺序：mariadb-->httpd-->php 
二进制安装mariadb  
ftp://172.16.0.1/pub/Source/7.x86_64/mariadb/mariadb-5.5-46-linuxx86_64.tar.gz 
tar xvf mariadb-5.5-46-linux-x86_64.tar.gz -C /usr/local 
cd /usr/local 
ls -sv mariadb-5.5.46-linux-x86_64 mysql  
cd mysql 
chown -R root.mysql ./*  
mkdir /mydata/data -p 
chown -R mysql.mysql /mydata/data 
mkdir /etc/mysql 
cp support-files/my-large.cnf /etc/mysql/my.cnf  
vim /etc/mysql/my.cnf  
[mysqld]加三行 
    datadir =/mydata/data 
    innodb_file_per_table = ON 
    skip_name_resolve = ON  
vim /etc/profile.d/mysql.sh  
      export PATH=/usr/local/mysql/bin/:$PATH 
cd /usr/local/mysql;scripts/mysql_install_db  --user=mysql -datadir=/mydata/data 
cp support-files/mysql.server  /etc/rc.d/init.d/mysqld 
chkconfig --add mysqld 
service mysqld start  
编译安装httpd-2.4 
yum  install  pcre-devel  apr-devel  apr-util-devel  openssl-devel 
./configure --prefix=/app/httpd24 \  --enable-so \  --enable-ssl \  --enable-cgi \  --enable-rewrite \  --with-zlib \  --with-pcre  \  --enable-modules=most  \  --enable-mpms-shared=all \  --with-mpm=prefork  \  --with-included-apr 
make -j 4 && make install 
编译安装php-5.6 
相关包：  libxml2-devel  bzip2-devel libmcrypt-devel (epel) 
./configure --prefix=/app/php --with-mysql=/usr/local/mysql --withopenssl --with-mysqli=/usr/local/mysql/bin/mysql_config --enablembstring --with-png-dir --with-jpeg-dir --with-freetype-dir --with-zlib -with-libxml-dir=/usr --enable-xml --enable-sockets --withapxs2=/app/httpd24/bin/apxs --with-mcrypt --with-config-filepath=/etc --with-config-file-scan-dir=/etc/php.d --with-bz2 
make -j 4 && make install 

实验过程


--------------------------------------------------------------------------------
CentOS7 php模块方式编译安装LAMP 
编译安装php-5.6 
相关包：  libxml2-devel  bzip2-devel libmcrypt-devel (epel) 
./configure --prefix=/app/php --with-mysql=/usr/local/mysql --withopenssl --with-mysqli=/usr/local/mysql/bin/mysql_config --enablembstring --with-png-dir --with-jpeg-dir --with-freetype-dir --with-zlib -with-libxml-dir=/usr --enable-xml --enable-sockets --withapxs2=/app/httpd24/bin/apxs --with-mcrypt --with-config-filepath=/etc --with-config-file-scan-dir=/etc/php.d --with-bz2 
make -j 4 && make install 


编译安装php-7.1.7
 ./configure --prefix=/app/php --enable-mysqlnd --with-mysqli=mysqlnd --with-openssl --with-pdo-=mysqlnd --enable-mbstring --withfreetype-dir --with-jpeg-dir --with-png-dir --with-zlib --with-libxmldir=/usr --enable-xml  --enable-sockets --withapxs2=/app/httpd24/bin/apxs --with-config-file-path=/etc --withconfig-file-scan-dir=/etc/php.d --enable-maintainer-zts --disablefileinfo 
注意：php-7.0以上版本使用--enable-mysqlnd --with-mysqli=mysqlnd ， 原--with-mysql不再支持 
为php提供配置文件     
cp php.ini-production /etc/php.ini 
编辑apache配置文件httpd.conf，以使apache支持php     
vim /etc/httpd24/conf/httpd.conf 
1加二行  
AddType application/x-httpd-php .php 
 AddType application/x-httpd-php-source .phps 
 
2 定位至DirectoryIndex index.html   
修改为DirectoryIndex index.php index.html 
 
apachectl restart
 
--------------------------------------------------------------------------------
CentOS7 fpm方式编译安装LAMP 
1 tar xvf php-7.1.7.tar.bz2  
 2 cd php-7.1.7/ 
3 ./configure --prefix=/app/php \ --enable-mysqlnd \ --with-mysqli=mysqlnd \ --with-openssl \ --with-pdo-mysql=mysqlnd \ --enable-mbstring  \ --with-freetype-dir \ --with-jpeg-dir \ --with-png-dir \ --with-zlib \ --with-libxml-dir=/usr \ --enable-xml \ --enable-sockets \ --enable-fpm  \ --with-config-file-path=/etc \ --with-config-file-scan-dir=/etc/php.d \ --enable-maintainer-zts \ --disable-fileinfo  
4  cp php.ini-production  /etc/php.ini 
5  cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm 
6  chmod +x /etc/init.d/php-fpm  
7  chkconfig --add php-fpm  
8  chkconfig php-fpm on 
9  cd /app/php/etc 
10 cp php-fpm.conf.default php-fpm.conf 
11 cp php-fpm.d/www.conf.default  php-fpm.d/www.conf 
12 service php-fpm start 
 配置httpd支持php 
vim /app/httpd24/conf/httpd.conf  
取消下面两行的注释 
LoadModule proxy_module modules/mod_proxy.so 
LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so 
修改下面行 
<IfModule dir_module>     
DirectoryIndex index.php index.html 
</IfModule> 
加下面四行 
AddType application/x-httpd-php .php 
AddType application/x-httpd-php-source .phps
 ProxyRequests Off 
ProxyPassMatch  ^/(.*\.php)$ fcgi://127.0.0.1:9000/app/httpd24/htdocs/$1 
 
--------------------------------------------------------------------------------

CentOS6编译安装PHP-FPM模式的LAMP 
在centos6上编译安装LAMP： 
mairadb：通用二进制格式，mariadb-5.5.56 
httpd：编译安装，httpd-2.4.27 
php5：编译安装，php-5.6.30 
Wordpress: 安装wordpress-4.8-zh_CN.tar.gz 
Xcache：编译安装xcache-3.2.0 
 
php5.4依赖于mariadb-devel包 
实现顺序：mariadb-->httpd-->php 
 编译httpd和二进制安装mariadb 
安装相关包     bzip2-devel  libxml2-devel libmcrypt-devel(epel源)   
编译安装php 
cd php-5.6.30/ 
./configure --prefix=/app/php5 --with-mysql=/usr/local/mysql --withopenssl --with-mysqli=/usr/local/mysql/bin/mysql_config --enablembstring --with-freetype-dir  --with-jpeg-dir --with-png-dir --with-zlib --with-libxml-dir=/usr --enable-xml --enable-sockets --enable-fpm  -with-mcrypt --with-config-file-path=/etc/php5 --with-config-filescan-dir=/etc/php5.d --with-bz2    
make -j 4 && make install  
 实现php的配置文件和服务脚本 
mkdir /etc/php5  /etc/php5.d/ 
cd php-5.6.30/ 
cp php.ini-production /etc/php5/php.ini 
 cp sapi/fpm/init.d.php-fpm  /etc/rc.d/init.d/php-fpm 
chmod +x /etc/rc.d/init.d/php-fpm  
chkconfig --add php-fpm 
chkconfig --list php-fpm 
编辑php配置文件 
cd /app/php5/etc 
cp php-fpm.conf.default   php-fpm.conf 
vim  /app/php5/etc/php-fpm.conf 
     pm.max_children = 50 
     pm.start_servers = 5 
     pm.min_spare_servers = 2 
     pm.max_spare_servers = 5 和pm.start_servers一致 
     pid = /app/php5/var/run/php-fpm.pid 
 service php-fpm restart  
修改httpd24的配置文件 
vim /app/apache24/conf/httpd.conf 
说明：启用httpd的相关模块 
在Apache httpd 2.4以后已经专门有一个模块针对FastCGI的实现，此模块为 mod_proxy_fcgi.so，它其实是作为mod_proxy.so模块的扩充，因此，这两个 模块都要加载 去掉下面两行注释 
LoadModule proxy_module modules/mod_proxy.so 
LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so 
添加如下二行    
AddType application/x-httpd-php  .php    
AddType application/x-httpd-php-source  .phps 
定位至DirectoryIndex index.html     
修改为：     DirectoryIndex  index.php  index.html 
加下面两行 
ProxyRequests Off    关闭正向代理 
ProxyPassMatch ^/(.*\.php)$
 fcgi://127.0.0.1:9000/app/httpd24/htdocs/$1 
 
service httpd24 restart 

测试   
vim /app/httpd24/htdocs/index.php   
如下：     
<?php       
   $link = mysql_connect('127.0.0.1','root','magedu');       
   if ($link)         
      echo "Success...";      
   else         
       echo "Failure...";       
   mysql_close();       
   phpinfo();     
?> 
 
--------------------------------------------------------------------------------
实验过程   （干净系统）
 准备 俩个主机7和17 分工（7）lap    （17） mysql 
(7) 相应的包 php-7.1.18.tar.bz2  apr-1.6.5.tar.bz2    apr-util-1.6.1.tar.bz2     httpd-2.4.37.tar.bz2 
wordpress-4.9.4-zh_CN.tar.gz
1  编译 httpd
 解相应的包 一个一个的解tar xf  apr-1.6.5.tar.bz2    apr-util-1.6.1.tar.bz2     httpd-2.4.37.tar.bz2
2 cp -r  apr-1.6.5  httpd-2.4.37/srclib.apr
   cp -r  apr-unil-1.6.1  httpd-2.4.37/srclib.apr-unil
  cd httpd-2.4.37/
3  装包组和依赖包
yum groupinstall "development tools"  -y
yum  install  pcre-devel   openssl-devel  expat-devel  -y
4   ./configure --prefix=/app/httpd24 \  --enable-so \  --enable-ssl \  --enable-cgi \  --enable-rewrite \  --with-zlib \  --with-pcre  \  --enable-modules=most  \  --enable-mpms-shared=all \  --with-mpm=prefork  \  --with-included-apr 
5 make -j 4 && make install 
6准备变量 
 cd /app/httpd24
echo 'PATH=/app/httpd24/bin:$PATH' > /etc/profile.d/lamp.sh
. /etc/profile.d/lamp.sh
7  cd  httpd-2.4.37/app/http24/bin
 .  /etc/profile.d/lamp.sh
启动  apachectl start
8建一个Apache用户
useradd -r -s /sbin/nologin apache
修改httpd配置文件 加入用户
vim /app/httpd24/conf/httpd.conf
 user  apache
 group  apache
重启httpd服务 


(17)编译数据库 （二进制安装）
相应的安装包mariadb-10.2.19-linux-x86_64.tar.gz
1 解压缩 到指定位置
tar xcf mariadb-10.2.19-linux-x86_64.tar.gz  -C /usr/local/
2  建立用户 cd  /usr/local/
useradd -s /sbin/nologin -r mysql  -d /data/mysql
改所属组
chown -R root.root mariadb-10.2.19-linux-x86_64/
创建软连接
ln -s   mariadb-10.2.19-linux-x86_64/  mysql
3  cd  mysql/
mkdir /data/mysql
chown mysql.mysql /data/mysql
脚本
scripts/mysql_install_ab --user=mysql --datadir=/data/mysql
4 配置文件   都 在/usr/local/mysql下
mkdir /etc/mysql/
cp  support-files/my-huge.cnf  /etc/mysql/my.cnf
改配置文件
vim  /etc/mysql/my.cnf   加入
 [mysqld] 
datadir=/data/mysql
5 启动脚本
cp support-files/mysql.server  /etc/init.d/mysqld
chkconfig --list
chkconfig --add mysqld
启动 service mysqld start
加变量
在/usr/local/mysql下
echo 'PATH=/usr/local/mysql/bin:$PATH' > /etc/profile.d/mysql.sh
. /etc/profile.d/mysql.sh
6 建立数据库 创建账号
create database wpdb;
grant all  on wpdb.*  to wpuser@'192.168.34.%' identified by 'cebtos'

(7) 编译php     fpm 方式
1安装依赖包 
yum install  libxml2-devel  bzip2-devel libmcrypt-devel       (epel) 
解包 tar xvf php-7.1.18.tar.bz2 
cd  php-7.1.18/
./configure --help  查看编译选项
2  ./configure --prefix=/app/php \ --enable-mysqlnd \ --with-mysqli=mysqlnd \ --with-openssl \ --with-pdo-mysql=mysqlnd \ --enable-mbstring  \ --with-freetype-dir \ --with-jpeg-dir \ --with-png-dir \ --with-zlib \ --with-libxml-dir=/usr \ --enable-xml \ --enable-sockets \ --enable-fpm  \ --with-config-file-path=/etc \ --with-config-file-scan-dir=/etc/php.d \ --enable-maintainer-zts \ --disable-fileinfo  

3  配置httpd支持php
vim /app/httpd24/conf/httpd.conf  
取消下面两行的注释 
LoadModule proxy_module modules/mod_proxy.so 
LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so 
修改下面行 
<IfModule dir_module>     
DirectoryIndex index.php index.html 
</IfModule> 
加下面四行 
AddType application/x-httpd-php .php 
AddType application/x-httpd-php-source .phps
 ProxyRequests Off 
ProxyPassMatch  ^/(.*\.php)$ fcgi://127.0.0.1:9000/app/httpd24/htdocs/$1 
4 在php-7.1.18/下
复制配置文件
 cp php.ini-production  /etc/php.ini 
复制脚本
 cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm 
加执行权限
 chmod +x /etc/init.d/php-fpm  
加入
chkconfig --add php-fpm  
开启
 chkconfig php-fpm on 
5 cd /app/php/etc 
改名
cp php-fpm.conf.default php-fpm.conf 
改名
cp php-fpm.d/www.conf.default  php-fpm.d/www.conf 
启动服务
service php-fpm start 

安装wordpress
tar  xvf wordpress-4.9.4-zh_CN.tar.gz
cd wordpress/
mv * /app/httpd24/htdocs/
cd/app/httpd24/htdocs/
mv wp-config-sample.php wp-config.php
vim wp-config.php   把数据库的账号密码数据库写入

提高性能
1改配置文件  增加sock 
 vim php-fpm.d/www.conf 
listen =/tmp/php-fpm.sock
重启服务
service php-fpm restart 
2改http的配置文件
vim /app/httpd24/conf/httpd.conf  
ProxyPassMatch  "^/(.*\.php(/.*)?)$ "unix:/tmp/php-fpm.sock |fcgi://localhost/app//httpd24/htdocs/" 
3 apachectl stop 
   apachectl start
如果错误  查看日志
tail -f  /app/httpd24/logs/error_log

实验：实现UDS的FASTCGI
1  创建用户
useradd apache 
修改用户
vim  /app/httpd24/conf/httpd.conf 
user apache
group apache
addType application/x-httpd-php .php
AddType application/x-httpd-php-source .phps
ProxyRequests Off
#ProxyPassMatch  ^/(.*\.php)$ fcgi://127.0.0.1:9000/app/httpd24/htdocs/$1
proxypassmatch "^/(.*\.php(/.*)?)$" "unix:/var/run/fpm.sock|fcgi://127.0.0.1/app/httpd24/htdocs/"
修改配置文件  
vim  /app/php/etc/php-fpm.d/www.conf
user = apache
group = apache

; The address on which to accept FastCGI requests.
; Valid syntaxes are:
;   'ip.add.re.ss:port'    - to listen on a TCP socket to a specific IPv4 address on
;                            a specific port;
;   '[ip:6:addr:ess]:port' - to listen on a TCP socket to a specific IPv6 address on
;                            a specific port;
;   'port'                 - to listen on a TCP socket to all addresses
;                            (IPv6 and IPv4-mapped) on a specific port;
;   '/path/to/unix/socket' - to listen on a unix socket.
; Note: This value is mandatory.
;listen = 127.0.0.1:9000
listen = /var/run/fpm.sock

listen.mode = 0666
