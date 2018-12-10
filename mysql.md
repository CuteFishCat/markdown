#  MYSQL数据库 
/etc/init.d/mysqld start  开启服务  

1. 数据库中事务的四大特性（ACID）
事务概念：
事务由单独单元的一个或多个SQL语句组成，在这个单元中，每个SQL语句是相互依赖的。而整个单独单元作为一个不可分割的整体，如果单元中某条SQL语句一旦执行失败或产生错误，整个单元将会回滚。所有受到影响的数据将返回到事物开始以前的状态；如果单元中的所有SQL语句均执行成功，则事物被顺利执行。  

⑴ 原子性（Atomicity）
　　原子性是指事务包含的所有操作要么全部成功，要么全部失败回滚。  

⑵ 一致性（Consistency）
　　一致性是指事务必须使数据库从一个一致性状态变换到另一个一致性状态，也就是说一个事务执行之前和执行之后都必须处于一致性状态。
　　拿转账来说，假设用户A和用户B两者的钱加起来一共是5000，那么不管A和B之间如何转账，转几次账，事务结束后两个用户的钱相加起来应该还得是5000，这就是事务的一致性。⑶ 隔离性（Isolation）
　　隔离性是当多个用户并发访问数据库时，比如操作同一张表时，数据库为每一个用户开启的事务，不能被其他事务的操作所干扰，多个并发事务之间要相互隔离。
　　即要达到这么一种效果：对于任意两个并发的事务T1和T2，在事务T1看来，T2要么在T1开始之前就已经结束，要么在T1结束之后才开始，这样每个事务都感觉不到有其他事务在并发地执行。⑷ 持久性（Durability）
　　持久性是指一个事务一旦被提交了，那么对数据库中的数据的改变就是永久性的，即便是在数据库系统遇到故障的情况下也不会丢失提交事务的操作。
2. MySQL中myisam与innodb的区别，至少5点
(1)、问5点不同；
    1>.InnoDB支持事物，而MyISAM不支持事物
    2>.InnoDB支持行级锁，而MyISAM支持表级锁
    3>.InnoDB支持MVCC, 而MyISAM不支持
    4>.InnoDB支持外键，而MyISAM不支持
    5>.InnoDB不支持全文索引，而MyISAM支持。
    InnoDB提供提交、回滚、崩溃恢复能力的事务安全（ASID）能力，实现并发控制。
    MyISAM提供较高的插入和查询记录的效率，主要用于插入和查询。
    memory用于临时存放数据，数据量不大并且不需要较高数据安全性。
    archive：如果只有插入和查询可以用，支持高并发的插入操作，但本身不是事务安全。  

(2)、2者select count(*)哪个更快，为什么
   MyISAM更快，因为MyISAM内部维护了一个计数器，可以直接调取。  

(3)mysql中四个存储引擎：innodb、myisam、memory、archive

3. innodb的事务与日志的实现方式
错误日志：记录出错信息，也记录一些警告信息或者正确的信息。
查询日志：记录所有对数据库请求的信息，不论这些请求是否得到了正确的执行。
慢查询日志：设置一个阈值，将运行时间超过该值的所有SQL语句都记录到慢查询的日志文件中。
二进制日志：记录对数据库执行更改的所有操作。
实现方式：事务日志是通过redo和innodb的存储引擎日志缓冲（Innodb log buffer）来实现的。

4. MYSQL的三级模式
（1）模式（逻辑模式）：是数据库中全体数据的逻辑结构和特征的描述，是所有用户的公共数据视图。  

（2）外模式（用户模式）：是数据库用户的数据视图，是局部数据的逻辑结构和特征的描述。  

（3）内模式（存储模式）：一个数据库只有一个内模式，是数据物理结构和存储方式的描述，是数据在数据库内部的表示方式。  


5. 表操作命令：create、alter、drop。    数据操作指令：select、insert、delete、update
select 表名 from ... where ...
insert into table values()
update 表名 set ...
delete from 表名 where ...

6. 外连接分为内连接、左连接、右连接
内连接是根据某个条件连接两个表共有的数据；
左连接是根据某个条件以及左边的表连接数据，右边的表没数据的话则填null；
右连接是根据某个条件以及右边的表连接数据，左边的表没数据的话则填null；

7. mysql中视图和表的区别以及联系是什么？
区别：
(1)视图是已经编译好的SQL语句，是基于SQL语句的结果集的可视化的表，而表不是。  

(2)视图没有实际的物理记录，而表有。  

(3)视图是窗口，表是内容。  

(4)视图是逻辑概念的存在，不占用物理空间；而表占用物理空间。  

(5)表可以及时对它进行修改；而视图只能用创建语句来修改。  

(6)视图是查看数据表的一种方法，可以查询数据表中某些字段构成的数据，只是一些SQL语句的集合。  

(7)从安全来说，视图可以防止用户直接接触表，因而用户不知道表结构。  

(8)表属于全局模式中的表，是实表；视图属于局部模式的表，是虚表。  

(9)视图的建立和删除只影响视图本身，不影响对应的表。  

联系：
      视图是在表之上建立的虚表，它的结构（所定义的列）和内容（所有记录）都来自表，视图依据表存在而存在。一个视图可以对应多个表。视图是表的抽象和在逻辑意义上建立的新关系。
      删除视图中的数据,数据库中表的数据会一起被删除。

8. 安全性操作
授权：grant 权限（列） on 表名 to 用户
所有权限：all priviliges
收回权限：revoke 权限（列） on 表名 from 用户

9. 完整性约束
主键约束：primary
 key
外键约束：foreign
 key
唯一约束：unique
检查约束：check
非空约束：not null

10. 存储过程(procedure)和函数(function)区别
      本质上它们都是存储程序。函数只能通过return语句返回单个值或表对象；而存储过程不允许执行return语句，但是可以通过output参数返回多个值。函数限制比较多，不能用临时变量，只能用表变量，还有一些函数都不可用等等；而存储过程的限制相对就比较少。函数可以嵌入在SQL语句中使用，可以在select语句中作为查询语句的一个部分调用；而存储过程一般是作为一个独立的部分来执行。

11. 触发器和约束的区别
      触发器是由服务器自动激活的，类似于约束，但是比约束更加灵活，可以实施比约束更加复杂的检查和操作，具有更强大的数据控制能力。

12. 事务隔离级别
（1）Read uncommitted 未提交读（RU）
        最弱的隔离级别，事务中的修改即使没有提交，对其他事务也都是可见的。（即脏读）  

（2）Read committed 提交读  不可重复读（RC）
        大多数数据库系统的默认隔离级别。
        解决了脏读的问题，一个事务从开始直到提交之前，所做的任何修改对其他事务都是不可见的。
        一个事务两次执行同样的查询，可能会得到不一样的结果。  

（3）Repeatable read 可重复读（RR）
        mysql默认隔离级别。
        解决了脏读的问题。该级别保证了在同一个事务中多次读取同样记录的结果是一致的。
       该级无法解决幻读的问题，幻读是当某个事务在读取某个范围内的记录时，另一个事务又在该范围内插入了新的记录，当之前的事务再次读取该范围的记录时，会产生幻读。
         innodb和xtradb存储引擎通过多版本并发控制（MVCC，Multiversion
 Concurrency Control）解决了幻读的问题。  

（4）Serializable 可串行化
        该级是最高的级别，通过强制事务串行执行，避免了幻读的问题，该级会在读取的每一行数据上都加锁，所以可能导致大量的超时和锁争用的问题，  

13. 索引脚本.note
MYSQL中索引文件以B树结构存储，索引可分为单列索引和多列索引。
对于多列索引中，一个SQL语句是否用到了索引取决于其数据是否符合最左前缀原则。
MySQL只有对以下操作符才使用索引：<，<=，=，>，>=，BETWEEN，IN，以及某些时候的LIKE。可以在LIKE操作中使用索引的情形是指另一个操作数不是以通配符（%或者_）开头的情形。例如，“SELECT id FROM people WHERE firstname LIKE ‘Li%’;”这个查询将使用索引，但“SELECT id FROM people WHERE firstname LIKE ‘%ike’;”这个查询不会使用索引。

14. 关系数据库的特点（记住6个点即可）
1）数据集中控制。在文件管理方法中，文件是分散的，每个用户或每种处理都有各自的文件，这些文件之间一般是没有联系的，因此，不能按照统一的方法来控制、维护和管理。而数据库则很好地克服了这一缺点，可以集中控制、维护和管理有关数据。  

2）数据独立性高。数据库中的数据独立于应用程序，包括数据的物理独立性和逻辑独立性，给数据库的使用、调整、优化和进一步扩充提供了方便，提高了数据库应用系统的稳定性。  

3）数据共享性好。数据库中的数据可以供多个用户使用，每个用户只与库中的一部分数据发生联系；用户数据可以重叠，用户可以同时存取数据而互不影响，大大提高了数据库的使用效率。  

4）数据冗余度小。数据库中的数据不是面向应用，而是面向系统。数据统一定义、组织和存储，集中管理，避免了不必要的数据冗余，也提高了数据的一致性。  

5）数据结构化，整个数据库按一定的结构形式构成，数据在记录内部和记录类型之间相互关联，用户可通过不同的路径存取数据。  

6）统一的数据保护功能，在多用户共享数据资源的情况下，对用户使用数据有严格的检查，对数据库规定密码或存取权限，拒绝非法用户进入数据库，以确保数据的安全性、一致性和并发控制。  
--------------------------------------------------------------------------------
##  安装MYSQL 
centos7  
http://downloads.mariadb.org/mariadb/repositories/
[mariadb]  

name = MariaDB  

baseurl = http://yum.mariadb.org/10.2/centos7-amd64  

gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB  

gpgcheck=1  

yum install MariaDB-server  


### RPM包安装MySQL （centos6）   service mysqld start
1.   yum install mysql-server
服务的启动脚本   /etc/rc.d/init.d/mysqld
MySQL的主程序路径   /usr/libexec/mysqld
数据库的路径   /var/lib/mysql
 MySQL的配置文件       /etc/my.cnf      服务器端和客户端
2.  service mysqld start   启动服务
启动后生成     /var/lib/mysql/

mysql.sock     套接字文件
3.  登录系统：mysql  –uroot  –p  
服务器的命令在后面加 分号 ；
更改提示符  永久生效写在  /etc/my.conf 


###  （centos7）       yum install mariadb-server 
1. 配置文件      /etc/my.cnf.d/server.cnf
 服务器的程序             /usr/libexec/mysqld
 数据库存放的路径         /var/lib/mysql
2.  systemctl start mariadb
3. 登陆 mysql 
 更改提示符  永久生效写在   vim /etc/my.cnf.d/mysql-clients.cnf 

服务器的命令在后面加 分号 ；
执行这个命令 启动安全  mysql_secure_installation 
添加密码和删除匿名账号
删除目录下的文件重启服务可以恢复默认 （测试环境可以) 生产环境不可以

 做维护的时候可以不让别人连接
vim /etc/my.cnf   
   [mysqld]      
skip-networking=1   
只能自己连接而且3306的端口也没有了

##  数据库迁移
新分区
1.  fdisk /dev/sda
n 1G  t  8e  w
2.  同步 partprobe
3.  pvcreate /dev/sda6      创建卷组名
 vgcreate vg0 /dev/sda6       加入卷组
 lvcreate -n mysql -l 100%FREE vg0    分卷组大小
4.  mkfs.xfs /dev/vg0/mysql    创建文件系统
    blkid
  mkdir /date/mysql
mount /dev/vg0/mysql  /date/mysql  挂载
5.  chown mysql.mysql /date/mysql    改文件夹属性 
6.   vim /etc/my.cnf    修改文件加入
datadir=/date/mysql
7. systemctl restart mariadb  重新启动服务
--------------------------------------------------------------------------------

## 通用二进制格式安装过程 
下载包   http://mariadb.org/
编译完了的二进制
1. 解包    解到/usr/local
tar xf mariadb-10.2.19-linux-x86_64.tar.gz -C /usr/local
2. 真正的目录在 /usr/local/mysql  创建一个软连接
ln -s /usr/local/mariadb-10.2.19-linux-x86_64/ /usr/local/mysql
3. 更改属性
chown -R root:root /usr/local/mysql/ 
4. 创建用户
useradd -r -s /sbin/nologin -d /data/mysql -c 'mariadb user' mysql
5.  
mkdir /date/mysql
install -d /date/mysql -o mysql -g mysql

6. 运行 mysql_install_db 
 在/usr/local/mysql/ 下运行
scripts/mysql_install_db  --user=mysql --datadir=/date/mysql

7. mysql 的配置文件
cp support-files/my-huge.cnf /etc/mysql/my.cnf
vim /etc/mysql/my.cnf
改路径socket    最好不要改
和加入datadir=/date/mysql


8. 服务脚本  mysql.server
cp support-files/mysql.server /etc/init.d/   移动过去
mv mysql.server mysqld   改个名字
chkconfig --add mysqld   出现在列表
service mysqld start   启动服务

9. 改PATH变量
echo 'PATH=/usr/local/mysql/bin:$PATH' >/etc/profile.d/mysql.sh
. /etc/profile.d/mysql.sh 

10. 安全脚本
mysql_secure_installation
ok了 可以启动MySQL
--------------------------------------------------------------------------------
源码编译安装maeiadb
1.  yum install bison bison-devel  zlib-devel libcurl-devel libarchive-devel  boostdevel  gcc  gcc-c++  cmake ncurses-devel gnutls-devel libxml2-devel openssldevel libevent-devel libaio-devel  
 
 2.  做准备用户和数据目录
 useradd -r -s /sbin/nologin -d  /data/mysql/  mysql 
mkdir   /data/mysql 
chown mysql.mysql  /data/mysql 
3.    源码解压缩
tar xvf   mariadb-10.2.18.tar.gz  
4.    cmake 编译安装 
cd mariadb-10.2.18/ 
执行
cmake . \ 
-DCMAKE_INSTALL_PREFIX=/app/mysql \ 
-DMYSQL_DATADIR=/data/mysql/ \ 
-DSYSCONFDIR=/etc \ -DMYSQL_USER=mysql \ 
-DWITH_INNOBASE_STORAGE_ENGINE=1 \ 
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \ 
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \ 
-DWITH_PARTITION_STORAGE_ENGINE=1  \ 
-DWITHOUT_MROONGA_STORAGE_ENGINE=1 \ 
-DWITH_DEBUG=0 \ -DWITH_READLINE=1 \ 
-DWITH_SSL=system \ -DWITH_ZLIB=system \ 
-DWITH_LIBWRAP=0 \ -DENABLED_LOCAL_INFILE=1  \ -DMYSQL_UNIX_ADDR=/data/mysql/mysql.sock \ 
-DDEFAULT_CHARSET=utf8 \ 
-DDEFAULT_COLLATION=utf8_general_ci 

cmake -DCMAKE_INSTALL_PREFIX=/app/mysql -DMYSQL_DATADIR=/data/mysql/ -DSYSCONFDIR=/etc -DMYSQL_USER=mysql -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_ARCHIVE_STORAGE_ENGINE=1 -DWITH_BLACKHOLE_STORAGE_ENGINE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DWITHOUT_MROONGA_STORAGE_ENGINE=1 -DWITH_DEBUG=0 -DWITH_READLINE=1 -DWITH_SSL=system -DWITH_ZLIB=system -DWITH_LIBWRAP=0 -DENABLED_LOCAL_INFILE=1 -DMYSQL_UNIX_ADDR=/data/mysql/mysql.sock -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci 

5.  编译
make -j 4 && make install   
6. 准备环境变量 
/app/mysql  下
echo 'PATH=/app/mysql/bin:$PATH' > /etc/profile.d/mysql.sh 
.     /etc/profile.d/mysql.sh 
7.  生成数据库文件 
cd   /app/mysql/ 
scripts/mysql_install_db --datadir=/date/mysql/ --user=mysql 
8.  准备配置文件 
cp  /app/mysql/support-files/my-huge.cnf   /etc/my.cnf 
9.   准备启动脚本 
 cp /app/mysql/support-files/mysql.server  /etc/init.d/mysqld 
启动服务 
chkconfig --add mysqld 
service mysqld start 
--------------------------------------------------------------------------------
实战：rpm包安装
1. 、yum安装
centos6上yum install mysql-server
rpm -ql mysql-server
/etc/rc.d/init.d/mysqld  服务名（服务脚本的名称）
/usr/libexec/mysqld  数据库的主程序（二进制的程序路径与平时看到的二进制程序路径不一样）
/var/lib/mysql  存放用户数据的数据库所在的目录，将来存放大量的用户数据
/var/log/mysqld.log     mysql日志3  36638e  e8y8y88`yeyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyq
chkconfig mysqld on  设为开机启动
service mysqld start  启动mysqld服务
ss -ntul 查看3306端口是否打开
ls /var/lib/mysql/  第一次启动的时候会自动生成数据库文件，如图
mysql有一个专门的客户端工具，就是mysql
which mysql  显示/usr/bin/mysql
rpm -qf /usr/bin/mysql  查看mysql命令来自于哪个包，显示：
mysql-5.1.73-8.el6_8.x86_64  这是客户端工具包
在centos6命令行输入mysql
下列这些都是客户端命令，不需要加分号！
?         (\?) Synonym for `help’.
clear     (\c) Clear the current input statement.
connect   (\r) Reconnect to the server. Optional arguments are db and host.
delimiter (\d) Set statement delimiter.
edit      (\e) Edit command with $EDITOR.
ego       (\G) Send command to mysql server, display result vertically.
exit      (\q) 退出mysql数据库.
go        (\g) Send command to mysql server.
help      (\h) Display this help.
nopager   (\n) Disable pager, print to stdout.
notee     (\t) Don’t write into outfile.
pager     (\P) Set PAGER [to_pager]. Print the query results via PAGER.
print     (\p) Print current command.
prompt    (\R) 更改mysql提示信息，即更改提示符！prompt \u@[\D] \r:\m:\s ->
man mysql
vim /etc/my.cnf.d/mysql-clients.cnf
或者vim /etc/profile.d/mysql.sh
export MYSQL_PS1=”(\u@\h) [\d]> “定义为全局变量也可以！source或者.下生效
quit      (\q) 退出mysql数据库
rehash    (\#) Rebuild completion hash.
source    (\.) 执行SQL脚本，后面跟文件作为参数
status    (\s)  得到系统的状态信息
system    (\!)  在mysql里能执行shell命令
tee       (\T) Set outfile [to_outfile]. Append everything into given outfile.
use       (\u)  切换数据库，use后面跟数据库的名字
charset   (\C) Switch to another charset. Might be needed for processing binlog
mysql> show databases;查看当前系统中所有的数据库列表，显示
有三个数据库，每个数据库里又可以存放若干张表。
数据库连接时用的用户名，这个名字千万别和linux的用户名混淆，root@localhost可不是linux用户，是mysql数据库用户的自身账号，在mysql数据库中也有一个账号默认也叫root，这个root就是mysql数据库的管理员。@后面的代表主机名，指的是在mysql数据库中，root用户在localhost主机上登陆
mysql> status
在centos7上yum install mariadb-server -y
mariadb-server 服务器端包   mariadb  客户端包
rpm -ql mariadb-server
/usr/lib/systemd/system/mariadb.service 服务名
/usr/libexec/mysqld  二进制主程序路径
systemctl start mariadb  启动服务
ss -ntul 查看端口是否打开
在命令行输入mysql
su – liu 切换到普通用户
在命令行输入mysql
drop database test; 删除test库
drop tables  删除表
create database testdb;  创建testdb数据库
为什么liu用户能对mysql数据库进行删减，这也太不安全了，实际上，登陆mysql数据库时如果不输入用户密码时，默认是用mysql的root用户进行登陆。
为了加强安全，通常将root@localhost加口令！
而且如果
user表中存放的是当前在mysql数据库中的用户信息，显示内容太多
显示如下
所以mysql数据库刚刚装好的时候默认是不安全的，因此要做一些方法增强安全性！
要实现这个功能，目前不需要掌握太复杂的语法，只需要跑一个脚本即可！即
mysql_secure_installation，这个脚本是系统自带的脚本
which mysql_secure_installation 显示/usr/bin/mysql_secure_installation
rpm -qf /usr/bin/mysql_secure_installation显示
mariadb-server-5.5.56-2.el7.x86_64
在命令行输入mysql_secure_installation
 
设置完以后，再在命令行直接输入mysql就不能直接登陆了，如图
注意：mysql用户账号由两部分组成：’USERNAME’@’HOST’，上图中是四个账号！host代表你从哪台电脑上登陆。如果host写成192.168.30.6，意味着我可以坐在192.168.30.6主机上远程的用mysql数据库以root账号的身份来登陆！默认是不允许远程登陆的！
在centos6上通过远程连接centos7上的mysql数据库，可以联吗？
说明不可以连，原因是在centos7上mysql数据库根本没有授权centos6可以连！
 
实战：以mariadb为例，在官网（www.mariadb.org）上下载较新的rpm包进行安装！
登陆www.mariadb.org网站—Download—Download—
—
—
vim /etc/yum.repos.d/base.repo 修改yum仓库
光盘里带的是mariadb5.5,而自己配的yum仓库里面是mariadb10.2，如果这两个仓库同时都是开启状态，默认会下载哪一个版本？答：最新版本！
yum install mariadb-server
 
实战：在centos7.4上二进制安装mariadb-10.2.15-linux-x86_64.tar.gz过程
二进制安装指的是人家都给你编译好了，打包到一个tar文件里面，你把它下载下来解包，解包解到哪去要看人家编译的时候指定在哪，你就往哪解压缩，解完以后还要相应的配些东西。
第0步：下载二进制源码
www.mariadb.org—Download— Download—
—
下载后
rz 将二进制包传到centos7.4上
第一步：检查环境，iptables、selinux、mariadb-server、mysql-server，如果有老版本卸载！
第二步：下载包
第三步：
useradd -r -d /app/mysqldb -s /sbin/nologin mysql 创建mysql用户，并指定家目录是/data/mysqldb，用来存放用户数据的。
 
由于二进制文件是已经被别人编译过的，那么解包的时候就要将文件解到编译时指定的目录下，所以要搞清楚人家编译的时候它的路径在哪，怎么查？
tar -xvf mariadb-10.2.15-linux-x86_64.tar.gz -C /usr/local/  解包
cd /usr/local/
ln -s mariadb-10.2.15-linux-x86_64 mysql 创建软链接
chown -R root:root mysql  将mysql目录及目录下的所有文件的所有者、所属组改为root
echo PATH=/usr/local/mysql/bin:$PATH>/etc/profile.d/mysql.sh修改环境变量
. /etc/profile.d/mysql.sh  立即生效
第四步：创建逻辑卷，作为存放用户数据
在虚拟机上加一块200G硬盘，将整块硬盘作为逻辑卷
echo “- – -” > /sys/class/scsi_host/host0/scan
pvcreate /dev/sdb  创建物理卷
vgcreate vg0 /dev/sdb 创建卷组
lvcreate -n lv_mysql -l 100%FREE vg0 创建逻辑卷，名字是lv_mysql
mkfs.xfs /dev/vg0/lv_mysql  创建文件系统
vim /etc/fstab   将逻辑卷挂载到/app下
mount -a  挂载
mkdir /app/mysqldb
chown mysql:mysql /app/mysqldb  将mysqldb目录的所有者和所属组改成mysql
chmod 770 /app/mysqldb/  修改mysqldb目录的权限
需要注意的是：现在有两个文件夹，/usr/local/mysql这个文件夹是数据库的二进制程序放的位置，而/app/mysqldb放的是用户的数据的目录。
第五步：生成默认数据库，否则无法启动服务，生成的方法就是调用一个脚本
调用脚本的时候一定要进入到/usr/local/mysql目录下  cd /usr/local/mysql
切记：调用该脚本的时候必须只能在/usr/local/mysql目录下，不能进入到scripts目录下使用该脚本，因为该脚本还调用了/usr/local/mysql目录下的其他文件！
第五步：修改配置文件，告诉二进制程序数据库文件是放在/app/mysqldb下
配置文件就在解包的目录下，即/usr/local/mysql/support-files目录下，有好几个模板，按需求进行选择
cp /etc/my.cnf{,.bak}  将原/etc/my.cnf文件备份
cp /usr/local/mysql/support-files/my-huge.cnf  /etc/my.cnf覆盖原配置文件
注：
vim /etc/my.cnf
第六步：
复制解包中自带的服务脚本
cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld -a
chkconfig –add mysqld 将mysqld服务加入到服务列表中
chkconfig –list
systemctl start mysqld 启动mysqld服务
ss -ntul
说明服务正常启动！
mysql_secure_installation  设置安全策略
/usr/local/mysql/bin/mysqld –print-defaults 查看mysqld主程序的默认设置
 
## 老师总结：
 
源码编译安装mariadb
1. 安装包
yum install bison bison-devel zlib-devel libcurl-devel libarchive-devel boost-devel gcc gcc-c++ cmake libevent-devel gnutls-devel libaio-devel openssl-devel ncurses-devel libxml2-devel
2. 做准备用户和数据目录
mkdir /data
useradd –r –s /bin/false –m –d /data/mysqldb/ mysql
tar xvf mariadb-10.2.12.tar.gz
3. cmake 编译安装
cd mariadb-10.2.12/
编译选项:
https://dev.mysql.com/doc/refman/5.7/en/source-configuration-options.html
 
源码编译安装mariadb
cmake . \
-DCMAKE_INSTALL_PREFIX=/app/mysql \   指定应用程序路径
-DMYSQL_DATADIR=/data/mysqldb/ \      指定数据库路径
-DSYSCONFDIR=/etc \                   配置文件
-DMYSQL_USER=mysql \                  用户名
-DWITH_INNOBASE_STORAGE_ENGINE=1 \  存储引擎
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \
-DWITH_PARTITION_STORAGE_ENGINE=1 \
-DWITHOUT_MROONGA_STORAGE_ENGINE=1 \
-DWITH_DEBUG=0 \
-DWITH_READLINE=1 \
-DWITH_SSL=system \
-DWITH_ZLIB=system \
-DWITH_LIBWRAP=0 \
-DENABLED_LOCAL_INFILE=1 \
-DMYSQL_UNIX_ADDR=/app/mysql/mysql.sock \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci
make && make install
 
源码编译安装mariadb
4. 准备环境变量
echo ‘PATH=/app/mysql/bin:$PATH’ >  /etc/profile.d/mysql.sh
. /etc/profile.d/mysql.sh
5. 生成数据库文件
cd /app/mysql/
scripts/mysql_install_db –datadir=/data/mysqldb/ –user=mysql
6. 准备配置文件
cp /app/mysql/support-files/my-huge.cnf /etc/my.cnf
7. 准备启动脚本
cp /app/mysql/support-files/mysql.server /etc/init.d/mysqld
8. 启动服务
chkconfig –add mysqld ;service mysqld start
 
例如：使用rpm包安装好mariadb服务后，如果我想将数据库存放的路径进行修改，如何实现？
vim /etc/my.cnf.d/server.cnf
[musqld]
datadir=/data/mysql
mkdir /data/mysql -pv
chown mysql:mysql /data/mysql
mysql_install_db –datadir=/data/mysql –user=mysql
systemctl start mariadb即可！
 
##  实战：源码编译安装
rz  将源码上传到centos7.4系统中  

第一步：安装所需包
yum install bison bison-devel zlib-devel libcurl-devel libarchive-devel boost-devel gcc gcc-c++ cmake libevent-devel gnutls-devel libaio-devel openssl-devel ncurses-devel libxml2-devel  

第二步：
useradd -r -s /sbin/nologin mysql  创建用户
tar -xvf mariadb-10.2.15.tar.gz   解包
mkdir -pv /app/mysql
chown mysql:mysql /app/mysql
mkdir -pv /data/mysqldb  

第三步：编译
cd mariadb-10.2.15
cmake . \
-DCMAKE_INSTALL_PREFIX=/app/mysql \
-DMYSQL_DATADIR=/data/mysqldb/ \
-DSYSCONFDIR=/etc \
-DMYSQL_USER=mysql \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \
-DWITH_PARTITION_STORAGE_ENGINE=1 \
-DWITHOUT_MROONGA_STORAGE_ENGINE=1 \
-DWITH_DEBUG=0 \
-DWITH_READLINE=1 \
-DWITH_SSL=system \
-DWITH_ZLIB=system \
-DWITH_LIBWRAP=0 \
-DENABLED_LOCAL_INFILE=1 \
-DMYSQL_UNIX_ADDR=/app/mysql/mysql.sock \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci
make -j 8 && make install    


第四步：后续与二进制安装步骤一样。  

echo PATH=/app/mysql/bin:$PATH > /etc/profile.d/mysql.sh
. /etc/profile.d/mysql.sh
cd /app/mysql/
生成数据库：
yum安装默认放在/usr/bin下
cp /app/mysql/support-files/my-huge.cnf /etc/my.cnf
vim /etc/my.cnf
cp support-files/mysql.server /etc/init.d/mysqld  准备服务脚本
setfacl -m u:mysql:rwx /app/mysql/  设置mysql这个用户对/app/mysql目录有读写执行权限
service mysqld start 启动服务
 实战：实现多实例，参考yum安装的包或者二进制安装的包都可以做多实例，我以yum安装的包为例
yum install mariadb-server -y
rpm -ql mariadb-server
/usr/bin/mysqld_multi 这个程序本身就是用来实现多实例的，但条件是必须是同一个版本，不能做不同版本之间的多实例，灵活性较差！
每个实例有自己的数据库路径、有各自的端口号
第一步：
#以端口号为文件夹名，每个实例都有一套各自的配置文件
mkdir /mysqldb/{3306,3307,3308}/{etc,socket,pid,log,data} -pv
由于是yum安装的，会自动生成mysql这个用户
chown -R mysql:mysql /mysqldb  将/mysqldb这个目录及目录下的所有文件的所有者和所属组都改成mysql，mysql用户就对/mysqldb这个目录具有完全控制的权限。
第二步：生成各自的配置文件，配置文件放到各自的配置文件目录下
cp /etc/my.cnf /mysqldb/3306/etc/ -a
cp /etc/my.cnf /mysqldb/3307/etc/ -a
cp /etc/my.cnf /mysqldb/3308/etc/ -a
vim /mysqldb/3306/etc/my.cnf
vim /mysqldb/3307/etc/my.cnf
vim /mysqldb/3308/etc/my.cnf
第三步：为各自的实例编制启动脚本 脚本内容如下（脚本放在D:\linux面授班课件\课堂笔记\课堂笔记\做mysql多实例时用到的脚本）
cp mysqld /mysqldb/3306
cp mysqld /mysqldb/3307
cp mysqld /mysqldb/3308
vim /mysqldb/3307/mysqld
vim /mysqldb/3308/mysqld
chmod +x /mysqldb/3306/mysqld   加执行权限
chmod +x /mysqldb/3307/mysqld
chmod +x /mysqldb/3308/mysqld
chmod 700 /mysqldb/3306/mysqld  修改权限
chmod 700 /mysqldb/3307/mysqld
chmod 700 /mysqldb/3308/mysqld
第四步：启动脚本
/mysqldb/3306/mysqld start   启动3306的mysqld服务
/mysqldb/3307/mysqld start   启动3307的mysqld服务
/mysqldb/3308/mysqld start   启动3308的mysqld服务
tree /mysqldb/3308
mysql -S /mysqldb/3308/socket/mysql.sock  连接3308的mysql数据库
第五步：现在各个实例都是没有口令的，随便登陆，这样很不安全，可以加口令，方法：
修改的口令生效
这块加完口令后，记得将启动脚本中也要添加口令，如图
vim /mysqldb/3308/mysqld
此时连接数据库要mysql -S /mysqldb/3308/socket/mysql.sock -uroot -pcentos

## mysql 
实验：基于源编译多实例

1 规划相关目录
mkdir /mysql/{3306,3307,3308}/{data,etc,socket,log,pid} -pv
chown -R mysql.mysql /mysql
2 准备数据库数据文件
/app/mysql/scripts/mysql_install_db  --user=mysql --datadir=/mysql/3306/data
/app/mysql/scripts/mysql_install_db  --user=mysql --datadir=/mysql/3307/data/
/app/mysql/scripts/mysql_install_db  --user=mysql --datadir=/mysql/3308/data

3 准备配置文件
vim /mysql/3308/etc/my.cnf 
[mysqld]
port=3308
datadir=/mysql/3308/data
socket=/mysql/3308/socket/mysql.sock
[mysqld_safe]
log-error=/mysql/3308/log/mariadb.log
pid-file=/mysql/3308/pid/mariadb.pid    

4 准备启动脚本
chmod +x /mysql/3308/mysqld

5 启动
/mysql/3308/mysqld start
mysql -uroot -h127.0.0.1 -P 3308 
mysql -uroot -S /mysql/3308/socket/mysql.sock

6 安全加固
/app/mysql/bin/mysql_secure_installation  -S /mysql/3308/socket/mysql.sock 

mysql -uroot -pcentos -h127.0.0.1 -P 3308 
mysql -uroot -pcentos -S /mysql/3308/socket/mysql.sock


cp /mysql/3308/mysqld /etc/init.d/mysql3308
chkconfig --list
chkconfig --add mysqld3308
--------------------------------------------------------------------------------
 多实例
systemctl stop mariadb
1  cd /date
mkdir /mysql/{3306,3307,3308} -pv
mkdir {3306,3307,3308}/{etc,socket,log,pid} -pv
chown -R mysql.mysql *
2  cd  /usr/local/mysql
./scripts/mysql_install_db  --user=mysql --datadir=/mysql/3306
./scripts/mysql_install_db  --user=mysql --datadir=/mysql/3307
./scripts/mysql_install_db  --user=mysql --datadir=/mysql/3308
3  cp /etc/my.cnf  /mysql/3306/etc/   我们选择第二种 
vim /mysql/3306/etc/my.cnf
改端口  改路径   主要是服务器端  客户端也该
socket     =/mysql/3306/socket/mysql.sock
这个是源码编译的

  这个是yum安装的
[mysql]
port=3306
datadir=/mysql/3306/
socket=/mysql/3306/socket/mysql.sock
[mysql_safe]
log-error=/mysql/3306/log/mariadb.log
pid-file=/mysql/3306/pid/mariadb.pid
#!includedir /etc/my.cnf.d

4  cp my.cnf  /mysql/3307/etc/   改端口  路径  3307
    cp my.cnf  /mysql/3308/etc/     改端口  路径   3308
5  cd  3306  准备服务的启动脚本   rz  mysqld
vim mysql  
port=3306
mysql_basedir="mysql"
 cp mysqld  ../3307  改端口号
cp mysqld ../3308    改端口号
6    启动  
如果把脚本 拷到、etc/init.d/ 改个名字  可以用命令启动
chmod +x mysql  
  ./mysqld start
关闭以前的 service mysqld stop
 ../3307/mysqld start  启动3307 端口

启动服务 停服务有口令
vim mysqld  去掉 -p $[mysql_pwd]
启动 
--------------------------------------------------------------------------------
老师笔记
实验：基于源编译多实例

1 规划相关目录
mkdir /mysql/{3306,3307,3308}/{data,etc,socket,log,pid} -pv
chown -R mysql.mysql /mysql
2 准备数据库数据文件
/app/mysql/scripts/mysql_install_db  --user=mysql --datadir=/mysql/3306/data
/app/mysql/scripts/mysql_install_db  --user=mysql --datadir=/mysql/3307/data/
/app/mysql/scripts/mysql_install_db  --user=mysql --datadir=/mysql/3308/data

3 准备配置文件
vim /mysql/3308/etc/my.cnf 
[mysqld]
port=3308
datadir=/mysql/3308/data
socket=/mysql/3308/socket/mysql.sock
[mysqld_safe]
log-error=/mysql/3308/log/mariadb.log
pid-file=/mysql/3308/pid/mariadb.pid    

4 准备启动脚本
chmod +x /mysql/3308/mysqld

5 启动
/mysql/3308/mysqld start
mysql -uroot -h127.0.0.1 -P 3308 
mysql -uroot -S /mysql/3308/socket/mysql.sock

6 安全加固
/app/mysql/bin/mysql_secure_installation  -S /mysql/3308/socket/mysql.sock 

mysql -uroot -pcentos -h127.0.0.1 -P 3308 
mysql -uroot -pcentos -S /mysql/3308/socket/mysql.sock


cp /mysql/3308/mysqld /etc/init.d/mysql3308
chkconfig --list
chkconfig --add mysqld3308


--------------------------------------------------------------------------------
1.MySQL多实例介绍
1.1.什么是MySQL多实例
MySQL多实例就是在一台机器上开启多个不同的服务端口（如：3306,3307），运行多个MySQL服务进程，通过不同的socket监听不同的服务端口来提供各自的服务:；
1.2.MySQL多实例的特点有以下几点
1：有效利用服务器资源，当单个服务器资源有剩余时，可以充分利用剩余的资源提供更多的服务。
2：节约服务器资源
3：资源互相抢占问题，当某个服务实例服务并发很高时或者开启慢查询时，会消耗更多的内存、CPU、磁盘IO资源，导致服务器上的其他实例提供服务的质量下降；
1.3.部署mysql多实例的两种方式
第一种是使用多个配置文件启动不同的进程来实现多实例，这种方式的优势逻辑简单，配置简单，缺点是管理起来不太方便；
第二种是通过官方自带的mysqld_multi使用单独的配置文件来实现多实例，这种方式定制每个实例的配置不太方面，优点是管理起来很方便，集中管理；
1.4.同一开发环境下安装两个数据库，必须处理以下问题
配置文件安装路径不能相同
数据库目录不能相同
启动脚本不能同名
端口不能相同
socket文件的生成路径不能相同
2.Mysql多实例安装部署
2.1.部署环境
Red Hat Enterprise Linux Server release 6.4
2.2.安装mysql软件版本
2.2.1.免编译二进制包
mysql-5.6.21-linux-glibc2.5-x86_64.tar.gz
2.3.解压和迁移
tar -xvf mysql-5.6.21-linux-glibc2.5-x86_64.tar.gz
mv mysql-5.6.21-linux-glibc2.5-x86_64 /usr/local/mysql
2.4.关闭iptables
临时关闭：service iptables stop 
永久关闭：chkconfig iptables off
2.5.关闭selinux
vi /etc/sysconfig/selinux  
将SELINUX修改为DISABLED，即SELINUX=DISABLED 
2.6.创建mysql用户
groupadd -g 27 mysql
useradd -u 27 -g mysql mysql
id mysql
uid=501(mysql) gid=501(mysql) groups=501(mysql)
2.7.创建相关目录
mkdir -p /data/mysql/ {mysql_3306,mysql_3307}
mkdir /data/mysql/mysql_3306/ {data,log,tmp}
mkdir /data/mysql/mysql_3307/ {data,log,tmp}
2.8.更改目录权限
chown -R mysql:mysql /data/mysql/ 
chown -R mysql:mysql /usr/local/mysql/
2.9. 添加环境变量
echo 'export PATH=$PATH:/usr/local/mysql/bin' >>  /etc/profile 
source /etc/profile  
2.10.复制my.cnf文件到etc目录
cp /usr/local/mysql/support-files/my-default.cnf /etc/my.cnf
2.11.修改my.cnf（在一个文件中修改即可）
[client]  
port=3306  
socket=/tmp/mysql.sock  
 
[mysqld_multi]  
mysqld = /usr/local/mysql /bin/mysqld_safe  
mysqladmin = /usr/local/mysql /bin/mysqladmin  
log = /data/mysql/mysqld_multi.log  
 
[mysqld]  
user=mysql  
basedir = /usr/local/mysql  
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES  
 
[mysqld3306]  
mysqld=mysqld  
mysqladmin=mysqladmin  
datadir=/data/mysql/mysql_3306/data  
port=3306  
server_id=3306  
socket=/tmp/mysql_3306.sock  
log-output=file  
slow_query_log = 1  
long_query_time = 1  
slow_query_log_file = /data/mysql/mysql_3306/log/slow.log  
log-error = /data/mysql/mysql_3306/log/error.log  
binlog_format = mixed  
log-bin = /data/mysql/mysql_3306/log/mysql3306_bin  
   
[mysqld3307]  
mysqld=mysqld  
mysqladmin=mysqladmin  
datadir=/data/mysql/mysql_3307/data  
port=3307  
server_id=3307  
socket=/tmp/mysql_3307.sock  
log-output=file  
slow_query_log = 1  
long_query_time = 1  
slow_query_log_file = /data/mysql/mysql_3307/log/slow.log  
log-error = /data/mysql/mysql_3307/log/error.log  
binlog_format = mixed  
log-bin = /data/mysql/mysql_3307/log/mysql3307_bin
2.12. 初始化数据库
2.12.1. 初始化3306数据库 
/usr/local/mysql/scripts/mysql_install_db --basedir=/usr/local/mysql/ --datadir=/data/mysql/mysql_3306/data --defaults-file=/etc/my.cnf  
2.12.2. 初始化3307数据库 
/usr/local/mysql/scripts/mysql_install_db --basedir=/usr/local/mysql/ --datadir=/data/mysql/mysql_3307/data --defaults-file=/etc/my.cnf  
2.12.3. 检查数据库是否初始化成功
出现两个”OK”
 



 
 
2.12.4. 查看数据库是否初始化成功（2）
查看3306数据库
[root@mysql ~]# cd /data/mysql/mysql_3306/data
[root@mysql data]# ls
auto.cnf  ibdata1  ib_logfile0  ib_logfile1  mysql  mysql.pid  performance_schema  test
 
查看3307数据库
[root@mysql ~]# cd /data/mysql/mysql_3307/data
[root@mysql data]# ls
auto.cnf  ibdata1  ib_logfile0  ib_logfile1  mysql  mysql.pid  performance_schema  test
2.13.设置启动文件
cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysql
2.14.mysqld_multi进行多实例管理
启动全部实例：/usr/local/mysql/bin/mysqld_multi start
查看全部实例状态：/usr/local/mysql/bin/mysqld_multi report 
启动单个实例：/usr/local/mysql/bin/mysqld_multi start 3306 
停止单个实例：/usr/local/mysql/bin/mysqld_multi stop 3306 
查看单个实例状态：/usr/local/mysql/bin/mysqld_multi report 3306 
2.14.1.启动全部实例
[root@mysql ~]# /usr/local/mysql/bin/mysqld_multi start
[root@mysql ~]# /usr/local/mysql/bin/mysqld_multi report
Reporting MySQL servers
MySQL server from group: mysqld3306 is running
MySQL server from group: mysqld3307 is running
2.15.查看启动进程  

2.16.修改密码
mysql的root用户初始密码是空，所以需要登录mysql进行修改密码，下面以3306为例： 
mysql -S /tmp/mysql_3306.sock   
set password for root@'localhost'=password('123456'); 
flush privileges; 
下次登录：
[root@mysql ~]# mysql -S /tmp/mysql_3306.sock -p
Enter password:
2.17.新建用户及授权
一般新建数据库都需要新增一个用户，用于程序连接，这类用户只需要insert、update、delete、select权限。
新增一个用户，并授权如下： 
grant select,delete,update,insert on *.* to admin@'192.168.0.%' identified by '123456'; 
flush privileges
2.18.外部软件登录数据库
 

2.19.测试成功
 

3.源码安装常见报错信息
1：安装mysql报错
checking for tgetent in -lncurses... no
checking for tgetent in -lcurses... no
checking for tgetent in -ltermcap... no
checking for tgetent in -ltinfo... no
checking for termcap functions library... configure: error: No curses/termcap library found
原因：
缺少ncurses安装包
解决方法：
yum list|grep ncurses
yum -y install ncurses-devel
yum install ncurses-devel
2：.../depcomp: line 571: exec: g++: not found
make[1]: *** [my_new.o] 错误 127
make[1]: Leaving directory `/home/justme/software/mysql-5.1.30/mysys'
make: *** [all-recursive] 错误 1
解决方法：
yum install gcc-c++
3：.../include/my_global.h:909: error: redeclaration of C++ built-in type `bool'
make[2]: *** [my_new.o] Error 1
make[2]: Leaving directory `/home/tools/mysql-5.0.22/mysys'
make[1]: *** [all-recursive] Error 1
make[1]: Leaving directory `/home/tools/mysql-5.0.22'
make: *** [all] Error 2
是因为gcc-c++是在configure之后安装的，此时只需重新configure后再编译make即可。
4：初始化数据库报错
报错现象：
root@mysql mysql-6.0.11-alpha]# scripts/mysql_install_db --basedir=/usr/local/mysql/ --user=mysql
Installing MySQL system tables...
ERROR: 1136  Column count doesn't match value count at row 1
150414  7:15:56 [ERROR] Aborting
150414  7:15:56 [Warning] Forcing shutdown of 1 plugins
150414  7:15:56 [Note] /usr/local/mysql//libexec/mysqld: Shutdown complete
Installation of system tables failed!  Examine the logs in
/var/lib/mysql for more information.
You can try to start the mysqld daemon with:
shell> /usr/local/mysql//libexec/mysqld --skip-grant &
and use the command line tool /usr/local/mysql//bin/mysql
to connect to the mysql database and look at the grant tables:
shell> /usr/local/mysql//bin/mysql -u root mysql
mysql> show tables
Try 'mysqld --help' if you have problems with paths.  Using --log
gives you a log in /var/lib/mysql that may be helpful.
The latest information about MySQL is available on the web at
http://www.mysql.com/.  Please consult the MySQL manual section
'Problems running mysql_install_db', and the manual section that
describes problems on your OS.  Another information source are the
MySQL email archives available at http://lists.mysql.com/.
Please check all of the above before mailing us!  And remember, if
you do mail us, you MUST use the /usr/local/mysql//scripts/mysqlbug script!
原因：
原有安装的mysql信息没有删除干净
解决方法：
删除/var/lib/mysql目录
--------------------------------------------------------------------------------
数据库的创建
查看所有的引擎：SHOW ENGINES 
查看表：SHOW TABLES [FROM db_name] 
查看表结构：DESC [db_name.]tb_name    SHOW COLUMNS FROM [db_name.]tb_name 
删除表：DROP TABLE [IF EXISTS] tb_name 
查看表创建命令：SHOW CREATE TABLE tbl_name 
查看表状态：SHOW TABLE STATUS LIKE 'tbl_name’ 
查看库中所有表状态：SHOW TABLE STATUS FROM db_name 


查看数据库   show  databases
查看表show tables

创建   create database  name
删除       drop  database name
 查看类型         show  character set;
 查看怎么创建的数据库信息 show create database name
创建指定的信息  create database name CHARACTER SET=utf8mb4


删除   drop database  name
创建表
查看帮助   help create table;
1  create table student (id int unsigned auto_increment primary key,name varchar(10) not null,sex enum ('f','m') default 'm',age tinyint unsigned ,phone char(11),address varchar(50));
查看表  desc student         show columns from student;
  查看当初怎么定义的表   show create table student;     


在表中增加内容  insert     详情  help insert 
  查看表的数     select * from student
  查询 表中的记录数select count(*) from student
1在表中添加记录
insert into student (name,age,phone,address) values ('mage',30,10086,'beijing')
或者insert student set name='wang',age=18,phone=10020,address='shanghai';
或者 是一个表中的数据导入和他相同表结构相同的表中
    insert custom select * from student



2   克隆一个表   create table employee select * from student;


 3   create table custom like student;
数据没有同步到

4   更改表的编码alter table student CHARACTER SET =utf8mb4;
该字段的字符级   
统一编码  

服务器端vim /etc/my.cnf.d
[mysqld]
character-set-server=utf8mb4
重启服务 systemctl restart mariadb
客户端vim /etc/my.cnf.d/mysql-clients.cnf
[mysql] 
default-character-set=utf8mb4

 删除表中的记录 单个
delete from student where  id >= 5;
truncate table stydent; 全部删 不记录日志

修改信息
update student set name='bbb' where id=5;
显示特定的行
select id,name,age from student;
显示特定列
select * from student where age >25;
select * from student where age >= 25 and age <=28;
select * from student where age between 25 and 28;
select * from student where age in (20,30);
select * from student where phone is null;
select * from student where phone is not null;
包含特定东西
select * from  student where name like 'a%';
select * from  student where name like '^a';
去重
select distinct sex from student;
分组 统计

select count(*) from students;
select count(*) from students group by gender;
select gender ,count(*) from students group by gender;
select gender ,count(*)  as 人数 from students group by gender;
 select gender,avg(age) from students group by gender;

最大值
select max(age) from students group by classid,gender;
 select classid,gender, max(age) from students group by classid,gender;

select gender,avg(age)  from students group by gender;
先分组后过滤
select gender,avg(age)  from students group by gender  having gender ='M';
先过滤后分组

对年龄进行排序 从小到大   asc 默认可以不写
 select * from students order by age asc ;
对年龄进行排序 从大到小
select * from students order by age desc;
前三
select * from students order by age asc  limit 3 ;
跳过前三取后四个
select * from students order by age asc  limit 3,4 ;
先对班级排序然后年龄的倒序
select * from students order by classid ,age deac;
以ClassID为分组依据，显示大于30的分组及平均年龄 
select classid,avg(age) from students group by classid having avg(age) >30;
查询年龄大于等于20岁，小于等于25岁的同学的信息 
select * from students where age between 20 and 25;
select * from students where age >=20 and age <=25;
多表查询
select * from teachers;
.select * from students;



取名用as 
挑出相同的合起来 纵向合并
.select stuid ,name,age, gender from students;
union select * from teachers;
增加信息
insert teachers values(5, yutong,26,'M');
横向合并（交叉连接）cross join 
select * from students cross join teachers;

相同的部分取出来（内连接）
select * from students,teachers where students.teacherid=teachers.tid;

select *  from students inner join teachers on students.teacherid=teachers.tid;

select stuid,students.name,tid, teachers.name from students inner join teachers on students.teacherid=teachers.tid;

 select stuid,students.name as student_name,tid, teachers.name teacher_name from students inner join teachers on students.teacherid=teachers.tid;    设置别名

给表起别名   一旦有别名就必须用别名  以前的名字就用不了了


1、内联接（典型的联接运算，使用像 =  或 <> 之类的比较运算符）。包括相等联接和自然联接。     
内联接使用比较运算符根据每个表共有的列的值匹配两个表中的行。例如，检索 students和courses表中学生标识号相同的所有行。   
    
2、外联接。外联接可以是左向外联接、右向外联接或完整外部联接。     
在 FROM子句中指定外联接时，可以由下列几组关键字中的一组指定：     
1）LEFT  JOIN或LEFT OUTER JOIN     
左向外联接的结果集包括  LEFT OUTER子句中指定的左表的所有行，而不仅仅是联接列所匹配的行。如果左表的某行在右表中没有匹配行，则在相关联的结果集行中右表的所有选择列表列均为空值。       
2）RIGHT  JOIN 或 RIGHT  OUTER  JOIN     
右向外联接是左向外联接的反向联接。将返回右表的所有行。如果右表的某行在左表中没有匹配行，则将为左表返回空值。       
3）FULL  JOIN 或 FULL OUTER JOIN
完整外部联接返回左表和右表中的所有行。当某行在另一个表中没有匹配行时，则另一个表的选择列表列包含空值。如果表之间有匹配行，则整个结果集行包含基表的数据值。   

3、交叉联接   
交叉联接返回左表中的所有行，左表中的每一行与右表中的所有行组合。交叉联接也称作笛卡尔积。    
FROM 子句中的表或视图可通过内联接或完整外部联接按任意顺序指定；但是，用左或右向外联接指定表或视图时，表或视图的顺序很重要。有关使用左或右向外联接排列表的更多信息，请参见使用外联接。
     
左外连接
select * from students as s left outer join teachers t on s.teacherid=t.tid;

右外连接
select * from students as s right  outer join teachers t on s.teacherid=t.tid;



子查询  嵌套查询

--------------------------------------------------------------------------------
视图
视图本身没有任何东西，他是从表里调出来的。是虚的东西
创建视图
create view v_students as select stuid,name,age from students;

查看
desc v_students;
select * from v_students;

查看视图的定义
show create view v_students\G
--------------------------------------------------------------------------------
函数

自定义函数
见PPT
存储过程
查看用户select user,host from user;

创建用户 create user test@'172.18.%.%' identified by '123456';

删除用户 drop user 用户名@'IP地址“；
查看用户口令 select user,host,password from user;
修改密码  set password for test@'172.18.%.%'=password('123456');
    update user set password=password('123456') where user='root';
生效  flush privileges;
root 口令忘记方法  
vim /etc/my.cnf
加入 --skip-grant-tables 

重启服务 systemctl restart mariadb
MySQL用户和权限管理 
创建用户 create user test2@'172.18.%.%' identified by '123456'
授权  grant all on hellodb.* to test2@'172.18.%.%';
个特定hellodb ,students 库中 查看 name age 的权限 而且有密码
grant  select(name,age) on hellodb.students to test2@'172.18.%.%' identified by '123456‘；
查看授权 show grants for test2@'172.18.%.%' \G
取消授权 revoke  select on *.*  from test2@'172.18.%.%';
--------------------------------------------------------------------------------
索引     提高搜索的速度
 创建索引  CREATE INDEX index_name ON tbl_name (index_col_name[(length)],...); 
          create index  idx_name on students(name (10));
建立复合索引 create  index idx_name_age on  students(name,age);
复合索引有先后顺序 ，第一个先索引，后面的无法搜索索引
删除索引：DROP INDEX index_name ON tbl_name;  
查看索引：  SHOW INDEXES FROM [db_name.]tbl_name; 
优化表空间：  OPTIMIZE TABLE tb_name; 
查看索引的使用  SET GLOBAL userstat=1;  SHOW INDEX_STATISTICS; 
通过EXPLAIN来分析索引的有效性 
explain  select * from students where  name='  xuxian '
explain select * from students where name='x% and age=30';
查看状态   show indexes from students\G
--------------------------------------------------------------------------------
锁 
加锁 lock tables students read;  读锁  只能读不能改
解锁        退出 或者 unlock tables;
lock tables students write;  写锁   自己能读能改   别人看不来改过的
     unlock tables;   解锁  
整个数据库加读锁    flush tables with read lock;  改不了数据
可以kill进程  来解锁    kill4 
做数据库备份的时候用    加锁  
flush tables; 关闭正在打开的 表（清除查询缓存）
--------------------------------------------------------------------------------
事务
启动  begin 
结束 commit
撤销   rollback;  针对增删改   一些也没有用  定义  truncate 
自动提交：set autocommit={1|0} 默认为1，为0时设为非自动提交  建议：显式请求和提交事务，而不要使用“自动提交”功能  vim /etc/my .cnf        加入   autocommit=0

以事务的方式速度提升特别快   提高效率
事务隔离级别 show variables like 'tx_isolation';

指定事务隔离级别： 
服务器变量tx_isolation指定，默认为REPEATABLE-READ，可在GLOBAL和 SESSION级进行设置     SET tx_isolation=''  
                READ-UNCOMMITTED   可以读到没有提交的   脏读
                READ-COMMITTED      可读到提交  没有提交不可读
                REPEATABLE-READ       开启事务 提交别人看不见
                SERIALIZABLE      开启事务  没有提交 ，别人改不了 相当于加写锁
服务器选项中指定
  vim /etc/my.cnf   
  [mysqld]  
   transaction-isolation=SERIALIZABLE 


死锁    两个或多个事务在同一资源相互占用，并请求锁定对方占用的资源的状态  
  事务日志     /var/lib/mysql       ib_logfile 事务文件     ibsata 数据库文件
事务日志：transaction log 
事务型存储引擎自行管理和使用，建议和数据文件分开存放
   redo log
   undo log 
Innodb事务日志相关配置：
 show variables like '%innodb_log%'; 
 innodb_log_file_size   5242880   每个日志文件大小 
 innodb_log_files_in_group  2      日志组成员个数 
innodb_log_group_home_dir  ./   事务文件路径 
 innodb_flush_log_at_trx_commit  默认为1 
   tail -f     查看日志过程
 
通用日志：记录对数据库的通用操作，包括错误的SQL语句   
  文件：file，默认值       
  表：table 
通用日志相关设置         
     general_log=ON|OFF    
     general_log_file=HOSTNAME.log       
      log_output=TABLE|FILE|NONE 

慢查询日志 
slow_query_log=ON|OFF 开启或关闭慢查询
 long_query_time=N    慢查询的阀值，单位秒
 slow_query_log_file=HOSTNAME-slow.log  慢查询日志文件

 log_queries_not_using_indexes=ON  不使用索引或使用全索引扫描，不论 是否达到慢查询阀值的语句是否记录日志，默认OFF，即不记录 
注意：建议二进制日志和数据文件分开存放 
二进制日志 
格式配置   show variables like ‘binlog_format'; 
sql_log_bin=ON|OFF：是否记录二进制日志，默认ON 
log_bin=/PATH/BIN_LOG_FILE：指定文件位置；默认OFF，表示不启用二 进制日志功能，上述两项都开启才可 
专门建一个目录放二进制文件 
 mkdir /date/logbin 
chown -R mysql.mysql /date/logbin
改存放的路 vim /etc/my.cnf
log-bin=/date/logbin/mysql
binlog_format=STATEMENT|ROW|MIXED：二进制日志记录的格式，默认 STATEMENT
改成  row  至少是mixed 

临时禁用 二进制    set sql_log_bin=OFF;
show binary logs;    查看二进制  文件大小 
show master logs； 
show master status;     更详细 
flush logs;  刷新日志 
查看二进制文件中的指定内容 
 show binlog events in 'mysql-bin.000021';

跳过 2个看3个
show binlog events in 'mysql-bin.000021' from 256  limit 2 3;
  mysqlbinlog：二进制日志的客户端命令工具 
示例：mysqlbinlog  --start-position=6787 --stop-position=7527
 /var/lib/mysql/mariadb-bin.000003 -v 
 mysqlbinlog  --start-datetime="2018-01-30 20:30:10"   
--stopdatetime="2018-01-30 20:35:22" mariadb-bin.000003 -vvv 
清除指定的二进制 purge binary logs to 'mysql-bin.000004';  清除前面的
reset master    删除所有二进制日志文件，并重新生成日志文 件
切换日志 FLUSH LOGS;   

--------------------------------------------------------------------------------
备份和恢复
完全备份 冷备份           
二台机 （ 主  副  ）   
 副  rpm -q mariadb-server   有没有 
安装   yum install mariadb-server   
             systemctl restart autofs    
主     scp /etc/my.cnf 副;/etc/
副      创建二进制文件目录
更改属组 
 主    scp -rp /var/lib/mysql/*  副/var/lib/mysql/
 副    chown -r mysql.mysql. /var/lib/mysql/
    systemctl start mariadb  启动数据库
--------------------------------------------------------------------------------
差异备份 每一个差异备份都包含前一个差异备份
先用完全备份还原 然后用最近的差异备份还原 最后一部分二进制还原
增量备份  一个一个的还原  最后二进制还原    
--------------------------------------------------------------------------------
lvm 备份
(1) 请求锁定所有表   mysql> FLUSH TABLES WITH READ LOCK; 
(2) 记录二进制日志文件及事件位置 
  mysql> FLUSH LOGS;  
  mysql> SHOW MASTER STATUS;  
  mysql -e 'SHOW MASTER STATUS' > /PATH/TO/SOMEFILE 
(3) 创建快照   lvcreate -L # -s -p r -n NAME /DEV/VG_NAME/LV_NAME 
(4) 释放锁   mysql> UNLOCK TABLES; 
(5) 挂载快照卷，执行数据备份 
(6) 备份完成后，删除快照卷 (7) 制定好策略，通过原卷备份二进制日志 
细节
1  实现逻辑卷
fdisk /dev/sda
n +5G   t  8e
同步   partprobe
变成物理卷  pvcreate  /dev/sda6
卷组   vgcreate vg0 /dev/sda6
lvcreate -n mysql -L 1G vg0
lvcreate -n binlog -L 1G vg0
mkdir /data/{mysql,binlog} -pv
改所属组
chown -R mysql.mysql /data/mysql
chowm -R mysql.mysql /data/binlog
格式化
mkfs.xfs /dev/vg0/mysql
mkfs.xfs /dev/vg0/binlog
挂载
mount /dev/vg0/mysql  /data/mysql/
mount /dev/vg0/binlog   /data/binlog/
改路径
vim /etc/my.cnf 
log-bin=/data/binlog/mysql-bin
datadir=/data/mysql
socket=/data/mysql/mysql.sock
数据迁移
systemctl stop mariadb   停止服务
 cp -av /var/lib/mysql/*  /data/mysql/
systemctl  start mariadb   重启服务
 mysql  -S /data/mysql/mysql.sock  人为的指定socket 文件路径（客户端登）
改配置文件
vim /etc/my.cnf.d/mysql-clients.cnf
socket=/data/mysql/mysql.sock
2   锁表
 FLUSH TABLES WITH READ LOCK; 
3   FLUSH LOGS;    刷新 
    show master logs; 记录 写到文件
 外面执行开一个新的窗口 不要退出来 退出来相当于解锁
   mysql -e 'show master logs' > post.log
4 做快照
 lvcreate -n mysql_snapshot -L 200M -s -p r /dev/vg0/mysql
  lvs  查看 
5  解锁  mysql> UNLOCK TABLES; 
6  挂载快照( 和之前的UUID相同 必须加 nouuid,norecovery)
mount  -o nouuid,norecovery /dev/vg0/mysql_snapshot /mnt
备份  打包 
tar -cvf /root/mysql .tar /mnt/
卸载快照   umount /mnt
lvremove /dev/vg0/mysql_snapshot    删除快照
7  还原
删库  rm -rf /data/mysql/*
tar xvf mysql.tar      解包 
mv mnt/* /data/mysql/   
还原二进制    禁止用户访问
set sql_llog_bin=off； 禁用二进制    
cd /data/binlog/
mysqlbinlog --start-position=454 mysql-bin.0000001  >incr.sql    最初记录的二进制导入文件
mysqlbinlog  mysql-bin.000002 >> incr.sql          都  追加到文件
mysqlbinlog  mysql-bin.000003 >> incr.sql
mysqlbinlog  mysql-bin.000004 >> incr.sql
mysqlbinlog  mysql-bin.000005 >> incr.sql
进入mysql    导入  
source /data/binlog/incr.sql   
--------------------------------------------------------------------------------
cat  .mysql_history    记录mysql操作命令
--------------------------------------------------------------------------------
备份和恢复 
1  mysqldump  hellodb  >   hellodb_bak.sql 对数据库备份   
    恢复的时候要创建一个和原来相同的数据库，名字 属性
    mysql -e 'create database hello'   创建一个新的数据库
    mysql hello < hellodb_bak.sql     导入备份数据库

    mysqldump hellodb students > students_bak.sql  备份表

2   mysqldump -B hellodb > hellodb_bak.sql   备份多个数据库  有数据库结构
     恢复的时候不需要建数据库  直接导入
     mysql < hellodb_bak.sql  
     mysql -e 'drop database hellodb'     删库
     备份打包压缩   xz  hellodb_bak.sql      解压 xz  -d  hellodb_bak.sql.xz
     mysqldump -B hellodb  | xz > hellodb_bak.sql.xz
     -R, --routines：备份所有存储过程和自定义函数 
3   mysqldump -A >  all_bak.sql         全部数据库都备份
   挑出二进制部分恢复    告诉日志从哪是新的  
  mysqldump  --master-data=1 -A > all_bak.sql           用到主从复制 
   vim all_bak.sql 

显示备份的时间点   
查位置点    mysql -e 'show master logs'
 mysqldump  --master-data=2 -A > all_bak.sql     不用到主从复制
  下面哪行被注释了      单纯的备份就=2就可以了
mysqldump -A -F > all_bak.sql   -F 用来分离旧的二进制文件 和新的分开

分库备份
 for db in `mysql -e 'show databases' |grep -Ev '^(Database|information_schema|performance_schema)$'`;do mysqldump -B $db|gzip  > $db`date +%F`.sql.gz;done

 mysql -e 'show databases' |grep -Ev '^(Database|information_schema|performance_schema)$'|sed -r 's/(.*)/mysqldump -B \1 |gzip > \/data\/\1.sql.gz/'  |bash

生产环境实战备份策略 
InnoDB建议备份策略 
 mysqldump –uroot –A –F –E –R  --single-transaction --master-data=1 -flush-privileges  --triggers --default-character-set=utf8 --hex-blob >$BACKUP/fullbak_$BACKUP_TIME.sql
简化版  mysqldump –uroot –A –F  --single-transaction --master-data=2  --default-character-set=utf8 --hex-blob >$BACKUP/fullbak_$BACKUP_TIME.sql
 MyISAM建议备份策略  
mysqldump –uroot –A –F –E –R –x --master-data=1 --flush-privileges  -triggers  --default-character-set=utf8  --hex-blob >$BACKUP/fullbak_$BACKUP_TIME.sql 

实验：删除数据库，还原至最新状态
1 数据库和二进制日志分离存放      
 2 确认字符集和存储引擎 
（mysql -e 'show create database hellodb'     mysql -e "show variables like 'character%'";)
mysqldump –uroot –A –F  --single-transaction --master-data=2  --default-character-set=utf8 --hex-blob >/date/fullbak_`date +%F`.sql
 3 完全备份数据库 
mysqldump -A --single-transaction -F --master-data=2 |gzip > /date/all_bak_`date +%F`.sql.gz
4 对数据库修改  插入一些记录
5 删除数据库    重启服务
 rm -rf /var/lib/mysql/*
systemctl restart mariadb 
6  禁止用户访问
7   解压缩 
gzip -d /date/all_bak_2018-12-05.sql.gz
8 重启服务
systemctl restart mariadb 
9 旧终端   禁用二进制
mysql> set sql_log_bin=off;
10 旧终端  导入备份数据
mysql>source /date/all_bak_2018-12-05.sql
11新开终端查看备份的位置 
 cat /date/all_bak_2018-12-05.sql |less
找到二进制备份的位置从00002往后的都恢复

12 新开终端   恢复二进制文件
ll /date/logbin
 mysqlbinlog  /date/logbin/mysql-bin.0000{07,08,09,10} > incr.sql
13 返回旧终端   这个过程中mysql不要退出来
 mysql>source /date/incr.sql
 14 开启二进制
 mysql> set sql_log_bin=on;
 15 检查无误后，恢复用户访问

实验：恢复误删除的表
1 备份
mysqldump -A -F --master-data=2 --single-transaction > all_bak.sql
2 对数据库修改  插入一些记录
3  删表  
drop table teachers;
4 还原
1）禁止用户访问
2）禁用二进制
mysql>set sql_log_bin=off;
3）导入备份的表 数据
mysql>source /data/all_bak.sql;
4）查看 all_bak.sql，确定二进制日志位置
5）恢复二进制  导入文件
mysqlbinlog  mysql-bin.000011 > /data/incr.sql
6）打开文件，找到删除文件的命令注释
vim /data/incr.sql
 将drop table 命令注释
7）导入二进制
mysql>source /data/all_bak.sql;
 8）恢复访问：启用二进制
set sql_log_bin=on;

--------------------------------------------------------------------------------
xtrabackup 
xtrabackup安装：  yum install percona-xtrabackup  在EPEL源中 
最新版本下载安装：  https://www.percona.com/downloads/XtraBackup/LATEST/ 
传入rpm包 然后安装
yum install  percona-xtrabackup-24-2.4.12-1.el7.x86_64.rpm
示例：旧版xtrabackup完全备份及还原 
1 在原主机 
  innobackupex --user=root  /backups         备份数据库到文件中   生成带日期的文件
  scp -r /backups/2018-02-23_11-55-57/     目标主机:/data/     将备份的文件复制到别的主机还原
2 在目标主机 
 innobackupex --apply-log /data/2018-02-23_11-55-57/     整理数据    整理后数据就是完整的
 systemctl stop mariadb 
 rm  -rf /var/lib/mysql/*     
 innobackupex  --copy-back /data/2018-02-23_11-55-57/  复制备份文件 
 chown -R mysql.mysql /var/lib/mysql/    改属性 
  systemctl start mariadb 
示例：新版xtrabackup完全备份及还原 
1 在原主机做完全备份到/backups    
 xtrabackup --backup --target-dir=/backups/           
 scp -r /backups/*    目标主机:/backups 
2 在目标主机上     
1）预准备：确保数据一致，提交完成的事务，回滚未完成的事务      
xtrabackup --prepare --target-dir=/backups/     
2）复制到数据库目录       
注意：数据库目录必须为空，MySQL服务不能启动     
 xtrabackup --copy-back --target-dir=/backups/    
 3）还原属性      
 chown -R mysql:mysql /var/lib/mysql    
 4）启动服务     
 systemctl start mariadb
示例：旧版xtrabackup完全，增量备份及还原 
1 在原主机  
innobackupex  /backups   做完全备份
mkdir /backups/inc{1,2}     创建二个目录  1 第一次增量的内容   2是第二次增量内容
修改数据库内容 
做第一次增量备份
 innobackupex  --incremental /backups/inc1 --incrementalbasedir=/backups/2018-02-23_14-21-42（完全备份生成的路径）  
再次修改数据库内容  
做第二次增量备份
innobackupex  --incremental /backups/inc2 --incrementalbasedir=/backups/inc1/2018-02-23_14-26-17 （上次增量备份生成的路径） 
 复制整个目录到目标主机
 scp -r /backups/* 目标主机:/data/ 

2 在目标主机
  不启动mariadb 
 rm -rf /var/lib/mysql/* 
先恢复完全备份
 innobackupex  --apply-log --redo-only /data/2018-02-23_14-21-42/  
再恢复增量备份
 innobackupex  --apply-log --redo-only /data/2018-02-23_14-21-42/ -incremental-dir=/data/inc1/2018-02-23_14-26-17  
最后一次增量备份还原   不用加redo-only
 innobackupex  --apply-log  /data/2018-02-23_14-21-42/ --incrementaldir=/data/inc2/2018-02-23_14-28-29/ 
 ls /var/lib/mysql/ 
 innobackupex  --copy-back /data/2018-02-23_14-21-42/ 
 chown -R mysql.mysql /var/lib/mysql/  
systemctl start mariadb 
示例：新版xtrabackup完全，增量备份及还原 
1 备份过程     
mkdir /backups/{base,inc1,inc2}
1）完全备份：xtrabackup --backup --target-dir=/backups/base     
 2）第一次修改数据      
3）第一次增量备份      xtrabackup --backup --target-dir=/backups/inc1 --incrementalbasedir=/backups/base     
 4）第二次修改数据      
5）第二次增量        
 xtrabackup --backup --target-dir=/backups/inc2 --incrementalbasedir=/backups/inc1     
 6）scp -r /backups/* 目标主机:/backups/ 
备份过程生成三个备份目录
 /backups/{base，inc1，inc2} 

2还原过程
 1）预准备完成备份，此选项--apply-log-only  阻止回滚未完成的事务 
xtrabackup --prepare --apply-log-only --target-dir=/backups/base
 2）合并第1次增量备份到完全备份，
 xtrabackup --prepare --apply-log-only --target-dir=/backups/base  -incremental-dir=/backups/inc1 
3）合并第2次增量备份到完全备份：最后一次还原不需要加选项--apply-log-only 
xtrabackup --prepare --target-dir=/backups/base --incremental-dir=/backups/inc2 
4）复制到数据库目录，注意数据库目录必须为空，MySQL服务不能启动 
xtrabackup --copy-back --target-dir=/backups/base 
5）还原属性：chown -R mysql:mysql /var/lib/mysql
 6）启动服务：systemctl start mariadb 

示例： xtrabackup单表导出和导入  （不怎么用 不能在5.5）
1 单表备份 
  innobackupex  --include='hellodb.students' /backups 
2备份表结构  
 mysql -e 'show create table hellodb.students' > student.sql 
3删除表  
mysql -e 'drop table  hellodb.students‘ 
4  innobackupex  --apply-log --export /backups/2018-02-23_15-03-23/ 
 5创建表      在导出的表结构文件中
  mysql>CREATE TABLE `students` (    `StuID` int(10) unsigned NOT NULL AUTO_INCREMENT,    `Name` varchar(50) NOT NULL,    `Age` tinyint(3) unsigned NOT NULL,    `Gender` enum('F','M') NOT NULL,    `ClassID` tinyint(3) unsigned DEFAULT NULL,    `TeacherID` int(10) unsigned DEFAULT NULL,    PRIMARY KEY (`StuID`)  ) ENGINE=InnoDB AUTO_INCREMENT=26 DEFAULT CHARSET=utf8 
 6 删除表空间  
alter table students discard tablespace; 
 7 cp /backups/2018-02-23_15-03-23/hellodb/students.{cfg,exp,ibd} /var/lib/mysql/hellodb/ 
 8 chown -R mysql.mysql /var/lib/mysql/hellodb/ 
9 mysql>alter table students import tablespace; 
--------------------------------------------------------------------------------
Percona XtraBackup 完全及增量备份与恢复的方法
安装及备份、恢复实现
安装：其最新版的软件可从 http://www.percona.com/software/percona-xtrabackup/ 获得。本文基于CentOS6.x的系统，因此，直接下载相应版本的rpm包安装即可，这里不再演示其过程。
1 yum -y install percona-toolkit-2.2.4-1.noarch.rpm percona-xtrabackup-2.1.8-733.rhel6.x86_64.rpm
完全备份及删除数据目录实现恢复：
mysql>set session_sql_log_bin=0; #导入数据时让其不记录二进制日志
mysql>source /root/hellodb.sql #导入数据
mysql>set session_sql_log_bin=1; #开启二进制日志
[root@centos6]#innobackupex --user=root /mybackups/ #全量备份
[root@centos6]#service mysqld stop #停止数据库，删除数据目录
[root@centos6]#rm -rf /mydata/data/*
[root@centos6]#innobackupex --apply-log /mybackups/2016-11-22_15-39-09/ #准备完全备份
[root@centos6]#innobackupex --copy-back /mybackups/2016-11-22_15-39-09/ #从完全备份中恢复数据
[root@centos6 data]# ll #数据已然恢复成功
总用量 28688
drwxr-xr-x. 2 root root 4096 11月 22 15:43 hellodb
-rw-r--r--. 1 root root 18874368 11月 22 15:43 ibdata1
-rw-r--r--. 1 root root 5242880 11月 22 15:43 ib_logfile0
-rw-r--r--. 1 root root 5242880 11月 22 15:43 ib_logfile1
drwxr-xr-x. 2 root root 4096 11月 22 15:43 mysql
drwxr-xr-x. 2 root root 4096 11月 22 15:43 performance_schema
drwxr-xr-x. 2 root root 4096 11月 22 15:43 test
[root@centos6 data]# chown -R mysql.mysql ./* #改变属组和属组
[root@centos6 data]# ll
总用量 28688
drwxr-xr-x. 2 mysql mysql
4096 11月 22 15:43 hellodb
-rw-r--r--. 1 mysql mysql 18874368 11月 22 15:43 ibdata1
-rw-r--r--. 1 mysql mysql 5242880 11月 22 15:43 ib_logfile0
-rw-r--r--. 1 mysql mysql 5242880 11月 22 15:43 ib_logfile1
drwxr-xr-x. 2 mysql mysql 4096 11月 22 15:43 mysql
drwxr-xr-x. 2 mysql mysql 4096 11月 22 15:43 performance_schema
drwxr-xr-x. 2 mysql mysql 4096 11月 22 15:43 test
[root@centos6 data]#service mysqld start #启动数据库，在恢复数据库时数据库无需启动
从安全角度考虑，如果要使用一个最小权限的用户进行备份，可创建此用户进行完全备份
mysql>create user 'bkuser'@'localhost' identified by 'passw ord'
mysql>revoke all privileges,grant option from 'bkuser'; 
mysql>grant reload,lock tables,replication clinet on *.* to 'bkuser'@'localhost' 
mysql>flush privileges;
xtrabackup备份文件说明：
使用innobakupex备份时，其会调用xtrabackup备份所有的InnoDB表，复制所有关于表结构定义的相关文件(.frm)、以及MyISAM、MERGE、CSV和ARCHIVE表的相关文件，同时还会备份触发器和数据库配置信息相关的文件。这些文件会被保存至一个以时间命令的目录中。
[root@centos6 2016-11-22_19-06-45]# ll
总用量 18472
-rw-r--r--. 1 root root 260 11月 22 19:06 backup-my.cnf
drwx------. 2 root root 4096 11月 22 19:06 hellodb
-rw-r-----. 1 root root 18874368 11月 22 19:06 ibdata1
drwx------. 2 root root 4096 11月 22 19:06 mydb
drwxr-xr-x. 2 root root 4096 11月 22 19:06 mysql
drwxr-xr-x. 2 root root 4096 11月 22 19:06 performance_schema
drwxr-xr-x. 2 root root 4096 11月 22 19:06 test
-rw-r--r--. 1 root root 13 11月 22 19:06 xtrabackup_binary
-rw-r--r--. 1 root root 24 11月 22 19:06 xtrabackup_binlog_info
-rw-r-----. 1 root root 89 11月 22 19:06 xtrabackup_checkpoints
-rw-r-----. 1 root root 2560 11月 22 19:06 xtrabackup_logfile
[root@centos6 2016-11-22_19-06-45]# cat xtrabackup_checkpoints
backup_type = full-backuped #备份类型，例如完全备份、增量备份等
from_lsn = 0 #日志序列号从0开始
to_lsn = 1649842 #日志序列号到哪
last_lsn = 1649842 #日志序列号到哪结束
compact = 0
[root@centos6 2016-11-22_19-06-45]# cat xtrabackup_binlog_info #当前正使用的二进制日志文件及其二进制位置
master-bin.000005 245
[root@centos6 2016-11-22_19-06-45]# cat xtrabackup_binary #备份中用到的xtrabackup的可执行文件
xtrabackup_55
[root@centos6 2016-11-22_19-06-45]# cat backup-my.cnf #备份命令中用到的配置选项信息
# This MySQL options file was generated by innobackupex.
# The MySQL server
[mysqld]
innodb_data_file_path=ibdata1:10M:autoextend
innodb_log_files_in_group=2
innodb_log_file_size=5242880
innodb_fast_checksum=0
innodb_page_size=16384
innodb_log_block_size=512
[root@centos6 2016-11-22_19-06-45]#
使用innobackupex进行增量备份及数据恢复：
每个InnoDB的页面都会包含一个LSN信息，每当相关的数据发生改变，相关的页面的LSN就会自动增长。这正是InnoDB表可以进行增量备份的基础，即innobackupex通过备份上次完全备份之后发生改变的页面来实现。
使用innobackupex进行增量备份及数据恢复：
每个InnoDB的页面都会包含一个LSN信息，每当相关的数据发生改变，相关的页面的LSN就会自动增长。这正是InnoDB表可以进行增量备份的基础，即innobackupex通过备份上次完全备份之后发生改变的页面来实现。
要实现第一次增量备份，可以使用下面的命令进行：
# innobackupex --incremental /backup --incremental-basedir=BASEDIR
其中，BASEDIR指的是完全备份所在的目录，此命令执行结束后，innobackupex命令会在/backup目录中创建一个新的以时间命名的目录以存放所有的增量备份数据。另外，在执行过增量备份之后再一次进行增量备份时，其--incremental-basedir应该指向上一次的增量备份所在的目录。
需要注意的是，增量备份仅能应用于InnoDB或XtraDB表，对于MyISAM表而言，执行增量备份时其实进行的是完全备份。
“准备”(prepare)增量备份与整理完全备份有着一些不同，尤其要注意的是：
(1)需要在每个备份(包括完全和各个增量备份)上，将已经提交的事务进行“重放”。“重放”之后，所有的备份数据将合并到完全备份上。
(2)基于所有的备份将未提交的事务进行“回滚”。
于是，操作就变成了：
# innobackupex --apply-log --redo-only BASE-DIR
接着执行：
# innobackupex --apply-log --redo-only BASE-DIR --incremental-dir=INCREMENTAL-DIR-1
而后是第二个增量：
# innobackupex --apply-log --redo-only BASE-DIR --incremental-dir=INCREMENTAL-DIR-2
innobackupex --copy-back /path/to/BACKUP-DIR #最起始的数据
其中BASE-DIR指的是完全备份所在的目录，而INCREMENTAL-DIR-1指的是第一次增量备份的目录，INCREMENTAL-DIR-2指的是第二次增量备份的目录，其它依次类推，即如果有多次增量备份，每一次都要执行如上操作
实例说明：本次实战练习增量数据的备份和恢复，在实验过程中经历过两次增量，并对相应的数据库做出了修改，增量完成后，停止mysql数据库，删除数据目录，进行数据恢复操作。
实现代码如下：
innobackupex --user=root /mybackups/ #第一次完全备份
innobackupex --incremental /mybackups/ --incremental-basedir=/mybackups/2016-11-22_19-24-37/ #第一次增量备份，向hellodb数据库中插入了表及其删除了表t1 
innobackupex --incremental /mybackups/ --incremental-basedir=/mybackups/2016-11-22_19-27-55/ #第二次增量备份，此时修改了数据库mydbs,创建了表和插入了数据
innobackupex --apply-log --redo-only /mybackups/2016-11-22_19-24-37/ #将已近提交的事务重放
innobackupex --apply-log --redo-only /mybackups/2016-11-22_19-24-37/ --incremental-dir=/mybackups/2016-11-22_19-27-55/ #重放之后，所有的备份数据合并到完全备份中
innobackupex --apply-log --redo-only /mybackups/2016-11-22_19-24-37/ --incremental-dir=/mybackups/2016-11-22_19-29-57/ #重放之后，所有的备份数据合并到完全备份中
innobackupex --copy-back /mybackups/2016-11-22_19-24-37/ #此时数据已经和最后一次增量同步，使用此数据进行恢复
[root@centos6 data]# cd /mybackups/2016-11-22_19-24-37/
[root@centos6 2016-11-22_19-24-37]# cat xtrabackup_checkpoints
backup_type = full-prepared #第一次完全备份，重放后将和最后一次增量备份中的序列号信息一致
from_lsn = 0
to_lsn = 1708653
last_lsn = 1708653
compact = 0
[root@centos6 2016-11-22_19-24-37]# cat ../2016-11-22_19-29-57/xtrabackup_checkpoints
backup_type = incremental
from_lsn = 1703868
to_lsn = 1708653
last_lsn = 1708653
compact = 0
[root@centos6 2016-11-22_19-24-37]# cat xtrabackup_binlog_info
master-bin.000006 8434
[root@centos6 2016-11-22_19-24-37]# cat ../2016-11-22_19-29-57/xtrabackup_binlog_info
master-bin.000006 8434
[root@centos6 2016-11-22_19-24-37]#
[root@centos6 data]# ll
总用量 18456
drwxr-xr-x. 2 root root 4096 11月 22 19:37 hellodb
-rw-r--r--. 1 root root 18874368 11月 22 19:37 ibdata1
drwxr-xr-x. 2 root root 4096 11月 22 19:37 mydb
drwxr-xr-x. 2 root root 4096 11月 22 19:37 mydbs
drwxr-xr-x. 2 root root 4096 11月 22 19:37 mysql
drwxr-xr-x. 2 root root 4096 11月 22 19:37 performance_schema
drwxr-xr-x. 2 root root 4096 11月 22 19:37 test
[root@centos6 data]# chown -R mysql.mysql ./* 
[root@centos6 data]# cd ../binlogs/
[root@centos6 binlogs]# mysqlbinlog --start-position=245 master-bin.000007
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=1*/;
/*!40019 SET @@session.max_insert_delayed_threads=0*/;
/*!50003 SET @OLD_COMPLETION_TYPE=@@COMPLETION_TYPE,COMPLETION_TYPE=0*/;
DELIMITER /*!*/;
# at 4
#161122 19:31:08 server id 1 end_log_pos 245 Start: binlog v 4, server v 5.5.32-MariaDB-log created 161122 19:31:08
# Warning: this binlog is either in use or was not closed properly.
BINLOG '
fCw0WA8BAAAA8QAAAPUAAAABAAQANS41LjMyLU1hcmlhREItbG9nAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAEzgNAAgAEgAEBAQEEgAA2QAEGggAAAAICAgCAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAA+1uD6g==
'/*!*/;
# at 245
#161122 19:31:49 server id 1 end_log_pos 355 Querythread_id=9exec_time=0error_code=0
use `mydbs`/*!*/;
SET TIMESTAMP=1479814309/*!*/;
SET @@session.pseudo_thread_id=9/*!*/;
SET @@session.foreign_key_checks=1, @@session.sql_auto_is_null=0, @@session.unique_checks=1, @@session.autocommit=1/*!*/;
SET @@session.sql_mode=0/*!*/;
SET @@session.auto_increment_increment=1, @@session.auto_increment_offset=1/*!*/;
/*!\C utf8 *//*!*/;
SET @@session.character_set_client=33,@@session.collation_connection=33,@@session.collation_server=8/*!*/;
SET @@session.lc_time_names=0/*!*/;
SET @@session.collation_database=DEFAULT/*!*/;
create table students(id int,name varchar(20))
/*!*/;
# at 355
#161122 19:32:26 server id 1 end_log_pos 424 Querythread_id=9exec_time=0error_code=0
SET TIMESTAMP=1479814346/*!*/;
BEGIN
/*!*/;
# at 424
#161122 19:32:26 server id 1 end_log_pos 537 Querythread_id=9exec_time=0error_code=0
SET TIMESTAMP=1479814346/*!*/;
insert into students values (1,'tom'),(2,'jerry')
/*!*/;
# at 537
#161122 19:32:26 server id 1 end_log_pos 564 Xid = 155
COMMIT/*!*/;
DELIMITER ;
# End of log file
ROLLBACK /* added by mysqlbinlog */;
/*!50003 SET COMPLETION_TYPE=@OLD_COMPLETION_TYPE*/;
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=0*/;
[root@centos6 binlogs]# mysqlbinlog --start-position=245 master-bin.000007>/root/incr.sql
[root@centos6 ~]# mysql
Welcome to the MariaDB monitor. Commands end with ; or \g.
Your MariaDB connection id is 3
Server version: 5.5.32-MariaDB-log MariaDB Server
Copyright (c) 2000, 2013, Oracle, Monty Program Ab and others.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
MariaDB [(none)]>set sql_log_bin=0; 
MariaDB [hellodb]>source /root/incr.sql
MariaDB [hellodb]>set sql_log_bin=1;
MariaDB [hellodb]>use mydbs;
MariaDB [mydbs]> show tables;
+-----------------+
| Tables_in_mydbs |
+-----------------+
| students |
| t1 |
+-----------------+
2 rows in set (0.00 sec)
MariaDB [mydbs]> select * from studnets;
ERROR 1146 (42S02): Table 'mydbs.studnets' doesn't exist
MariaDB [mydbs]> select * from students;
+------+-------+
| id | name |
+------+-------+
| 1 | tom |
| 2 | jerry |
+------+-------+
2 rows in set (0.00 sec)
MariaDB [mydbs]> use hellodb;
Database changed
MariaDB [hellodb]> show tables;
+-------------------+
| Tables_in_hellodb |
+-------------------+
| classes |
| coc |
| courses |
| scores |
| students |
| tbl |
| teachers |
| toc |
+-------------------+
8 rows in set (0.00 sec)
MariaDB [hellodb]>
经过查看数据和删除数据目录前进行对比，如数据一致则代表数据恢复成功，xtrabackup+二进制日志可实现数据的完全备份、增量备份、完全备份恢复和增量数据恢复，同时在使用此款备份工具时，不会影响客户的正常访问，达到提高用户体验。
更多XtraBackup相关教程见以下内容：
MySQL管理之使用XtraBackup进行热备 http://www.linuxidc.com/Linux/2014-04/99671.htm
使用Xtrabackup进行MySQL备份 http://www.codesec.net/Linux/2016-11/137734.htm
MySQL开源备份工具Xtrabackup备份部署 http://www.codesec.net/Linux/2013-06/85627.htm
MySQL Xtrabackup备份和恢复 http://www.codesec.net/Linux/2011-12/50275.htm
Percona Xtrabackup 安装 http://www.codesec.net/Linux/2016-11/137735.htm
--------------------------------------------------------------------------------
实现MySQL主从复制需要进行的配置：
主服务器：
开启二进制日志
配置唯一的server-id
获得master二进制日志文件名及位置
创建一个用于slave和master通信的用户账号
从服务器：
配置唯一的server-id
使用master分配的用户账号读取master二进制日志
启用slave服务
具体实现过程如下：
一、准备工作：
1.主从数据库版本最好一致
2.主从数据库内数据保持一致
主数据库：182.92.172.80 /linux
从数据库：123.57.44.85 /linux
二、主数据库master修改：
1.修改mysql配置
找到主数据库的配置文件my.cnf(或者my.ini)，我的在/etc/mysql/my.cnf,在[mysqld]部分插入如下两行：
[mysqld]
log-bin=mysql-bin #开启二进制日志
server-id=1 #设置server-id
2.重启mysql，创建用于同步的用户账号
打开mysql会话shell>mysql -hlocalhost -uname -ppassword
创建用户并授权：用户：rel1密码：slavepass
mysql> CREATE USER 'repl'@'123.57.44.85' IDENTIFIED BY 'slavepass';#创建用户
mysql> GRANT REPLICATION SLAVE ON *.* TO 'repl'@'123.57.44.85';#分配权限
mysql>flush privileges;   #刷新权限
3.查看master状态，记录二进制文件名(mysql-bin.000003)和位置(73)：

mysql > SHOW MASTER STATUS;
+------------------+----------+--------------+------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB |
+------------------+----------+--------------+------------------+
| mysql-bin.000003 | 73       | test         | manual,mysql     |
+------------------+----------+--------------+------------------+

二、从服务器slave修改：
1.修改mysql配置
同样找到my.cnf配置文件，添加server-id
[mysqld]
server-id=2 #设置server-id，必须唯一
2.重启mysql，打开mysql会话，执行同步SQL语句(需要主服务器主机名，登陆凭据，二进制文件的名称和位置)：

mysql> CHANGE MASTER TO
    ->     MASTER_HOST='182.92.172.80',
    ->     MASTER_USER='rep1',
    ->     MASTER_PASSWORD='slavepass',
    ->     MASTER_LOG_FILE='mysql-bin.000003',
    ->     MASTER_LOG_POS=73;

3.启动slave同步进程：
mysql>start slave;
4.查看slave状态：

mysql> show slave status\G;
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 182.92.172.80
                  Master_User: rep1
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000013
          Read_Master_Log_Pos: 11662
               Relay_Log_File: mysqld-relay-bin.000022
                Relay_Log_Pos: 11765
        Relay_Master_Log_File: mysql-bin.000013
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
        ...

当Slave_IO_Running和Slave_SQL_Running都为YES的时候就表示主从同步设置成功了。接下来就可以进行一些验证了，比如在主master数据库的test数据库的一张表中插入一条数据，在slave的test库的相同数据表中查看是否有新增的数据即可验证主从复制功能是否有效，还可以关闭slave（mysql>stop slave;）,然后再修改master，看slave是否也相应修改（停止slave后，master的修改不会同步到slave），就可以完成主从复制功能的验证了。
还可以用到的其他相关参数：
master开启二进制日志后默认记录所有库所有表的操作，可以通过配置来指定只记录指定的数据库甚至指定的表的操作，具体在mysql配置文件的[mysqld]可添加修改如下选项：

# 不同步哪些数据库  
binlog-ignore-db = mysql  
binlog-ignore-db = test  
binlog-ignore-db = information_schema  
  
# 只同步哪些数据库，除此之外，其他不同步  
binlog-do-db = game  

如之前查看master状态时就可以看到只记录了test库，忽略了manual和mysql库。
--------------------------------------------------------------------------------

MySQL复制 
主从配置过程：参看官网     
https://mariadb.com/kb/en/library/setting-up-replication/      https://dev.mysql.com/doc/refman/5.5/en/replication-configuration.html 
主节点配置：   
(1) 启用二进制日志            
 [mysqld]            
 log_bin    
(2) 为当前节点设置一个全局惟一的ID号            
[mysqld]           
 server_id=#   
log-basename=master  可选项，设置datadir中日志名称，确保不依赖主机名     
(3) 创建有复制权限的用户账号             
GRANT REPLICATION SLAVE  ON *.* TO 'repluser'@'HOST' IDENTIFIED BY 'replpass'; 

从节点配置： 
(1) 启动中继日志     
[mysqld]  server_id=#  为当前节点设置一个全局惟的ID号  
read_only=ON  设置数据库只读  
relay_log=relay-log  relay log的文件路径，默认值hostname-relay-bin  
relay_log_index=relay-log.index  默认值 hostname -relay-bin.index 
(2) 使用有复制权限的用户账号连接至主服务器，并启动复制线程
 mysql> CHANGE MASTER TO MASTER_HOST='host',  MASTER_USER='repluser', MASTER_PASSWORD='replpass',  MASTER_LOG_FILE=' mariadb-bin.xxxxxx', MASTER_LOG_POS=#;    
mysql> START SLAVE [IO_THREAD|SQL_THREAD]; 

在从节点配置访问主节点的参数信息
添加 主节点主机，访问主节点的用户名及密码，主节点二进制文件信息。
注意：主节点的二进制文件一定要是二进制列表中的最后一个二进制文件。


实验 
 调配实验环境 干净系统 清除二进制文件
从服务器
1 vim /etc/my.cnf
[mysql]
read_only=ON
server-id=2
重启服务
主服务器
vim /etc/my.cnf
[mysql]
 log_bin=/date/logbin/mysql-bin
server-id=1
 
重启服务systemctl restart mysql

主服务器 
创建有复制权限的用户账号  
grant replication slave on *.* to repluser@'172.18.134.&%' identified  by '123456';
 
show  master logs;  查询一下二进制日志

从服务器
启动服务
使用有复制权限的用户账号连接至主服务器，并启动复制线程
mysql>  help change master to    有例子改一下就行    然后执行一下

CHANGE MASTER TO
  MASTER_HOST='172.18.133.7',     主服务器的IP
  MASTER_USER='repluser',       在主服务器上创建的用户
  MASTER_PASSWORD='123456',   密码
  MASTER_PORT=3306,
  MASTER_LOG_FILE=' mysql-bin.000004',  二进制日志 在主服务器上查询到的
  MASTER_LOG_POS=328,      从二进制日志什么位置开始复制

mysql >  show slave status\G     查看从节点的状态
里面有主服务器的IP和创建的用户 存放二进制日志文件位置
mysql> start   slave;   开始复制       状态变成yes 
mysql> show  processlist;  查看线程   开始复制会有显示东西
数据库会同步主服务器的过来 
补充
mysql replication 中slave机器上有两个关键的进程，死一个都不行，一个是slave_sql_running，一个是Slave_IO_Running，一个负责与主机的io通信，一个负责自己的slave mysql进程。
 2 
 3 如果是slave_io_running no了，那么就我个人看有三种情况，一个是网络有问题，连接不上，像有一次我用虚拟机搭建replication，使用了nat的网络结构，就是死都连不上，第二个是有可能my.cnf有问题，配置文件怎么写就不说了，网上太多了，最后一个是授权的问题，replication slave和file权限是必须的。
 4 一旦io为no了先看err日志，看看爆什么错，很可能是网络，也有可能是包太大收不了，这个时候主备上改max_allowed_packet这个参数。 
 5 
 6 如果是slave_sql_running no了，那么也有两种可能，一种是slave机器上这个表中出现了其他的写操作，就是程序写了，这个是会有问题的，今天我想重现，但是有时候会有问题，有时候就没有问题，现在还不是太明了，后面再更新，还有一种占绝大多数可能的是slave进程重启，事务回滚造成的，这也是mysql的一种自我保护的措施，像关键时候只读一样。 
 7 
 8 这个时候想恢复的话，只要停掉slave，set GLOBAL SQL_SLAVE_SKIP_COUNTER=1;再开一下slave就可以了，这个全局变量赋值为N的意思是：

建第三个从服务器   不是一开始加 是后面开始加 
1 配置文件 设置成从节点
vim /etc/my.cnf
[mysql]
server-id=3
read-only
启动数据库服务  systemctl start mysql

主服务器
备份数据库
mysqldump -A -F --single-transaction --master-sata=1 > /date/all_bak.sql
复制备份文件到从服务器3
scp /date/all_bak.sql 172.18.134.234:/data
从服务器
vim all_bak.sql 
加入下面命令到二进制日志行上面  
CHANGE MASTER TO
  MASTER_HOST='172.18.133.7',  
  MASTER_USER='repluser',      
  MASTER_PASSWORD='123456',   
  MASTER_PORT=3306,
导入备份文件到mysql
mysql < all_bak.sql
启动复制
 mysql > start slave

主服务器down机  提升从服务器为主服务器
mysql >  show slave status\G     查看每个从服务器节点的状态  log_pos
假设2为新的主服务器  在从服务器2 上面
1停止线程  stop slave 
 清除所有从服务器上设置的主服务器同步信息如： PORT, HOST, USER和 PASSWORD 
mysql> RESET SLAVE  ALL
2vim /etc/my.cnf
去掉read_only=ON
加入二进制
[mysql]
log-bin
重启服务  systemctl  restart mariadb
在从服务器3上面
mysql > stop slave
mysql> reset slave all;
查看主2 上面的二进制日志   写上位置  执行下面
如果没有用户就创建 同上面一开始创建一样
CHANGE MASTER TO
  MASTER_HOST='172.18.133.17',  
  MASTER_USER='repluser',      
  MASTER_PASSWORD='123456',   
  MASTER_PORT=3306,
  MASTER_LOG_FILE=' mysql-bin.000004',  
  MASTER_LOG_POS=328；
启动复制
 mysql > start slave；

扩展  级联复制  (在主服务器修改文件 会经过第二个到第三个上面）
   步骤同上面   在中间从服务器需要改下面配置     
如果要启用级联复制,需要在第二个从服务器启用以下配置   
中间的节点的配置
[mysql]
server-id=2
log-bin
read-only
log_slave_updates   必须有

第三个从服务器
执行这个命令需要修改
修改IP为2的IP  用户用以前的，改二进制日志 是2上面的从头复制
CHANGE MASTER TO
  MASTER_HOST='172.18.133.17',  
  MASTER_USER='repluser',      
  MASTER_PASSWORD='123456',   
  MASTER_PORT=3306,
  MASTER_LOG_FILE=' mysql-bin.000004',  
  MASTER_LOG_POS=328；
启动复制 start  slave

实验：复制出错，解决实战
如果在从服务器上修改了数据库或者是表数据，会和主服务器发生冲突
解决办法在更改的从服务器上 先stop slave   然后 执行完启动
  set global sql_slave_skip_counter = 1

性能优化
 4、如何保证主从复制的事务安全  参看https://mariadb.com/kb/en/library/server-system-variables/ 
在master节点启用参数：  
          sync_binlog=1 每次写后立即同步二进制日志到磁盘，性能差       
          如果用到的为InnoDB存储引擎：      
          innodb_flush_log_at_trx_commit=1  每次事务提交立即同步日志写磁盘       
          innodb_support_xa=ON   默认值，分布式事务MariaDB10.3.0废除  
          sync_master_info=#    #次事件后master.info同步到磁盘 
在slave节点启用服务器选项：   
           skip_slave_start=ON 不自动启动slave 
在slave节点启用参数：  
            sync_relay_log=#    #次写后同步relay log到磁盘      
            sync_relay_log_info=# #次事务后同步relay-log.info到磁盘 
--------------------------------------------------------------------------------
主主复制 
主主复制：互为主从 
容易产生的问题：数据不一致；因此慎用 
考虑要点：自动增长id         
配置一个节点使用奇数id              
        auto_increment_offset=1     开始点               
        auto_increment_increment=2   增长幅度         
另一个节点使用偶数id               
         auto_increment_offset=2              
         auto_increment_increment=2 
 主主复制的配置步骤：  
(1) 各节点使用一个惟一server_id 
(2) 都启动binary log和relay log  
(3) 创建拥有复制权限的用户账号 
(4) 定义自动增长id字段的数值范围各为奇偶  
(5) 均把对方指定为主节点，并启动复制线程 

练习
1配置一个节点使用奇数id              
        auto_increment_offset=1     开始点               
        auto_increment_increment=2   增长幅度         
另一个节点使用偶数id               
         auto_increment_offset=2              
         auto_increment_increment=2  
重启启用服务
互为主从，先把一个设置为主把另一个设置为从，然后再把另一个设置为主，一个为从
（3）第一个机  用户创建
grant replication slave on *.* to rpl@'172.18.133.%' identified by '123456';
查看二进制 show master logs;
4在另一台机
执行复制   改为在第一个机的二进制
CHANGE MASTER TO
  MASTER_HOST='172.18.133.7',  
  MASTER_USER='rpl',      
  MASTER_PASSWORD='123456',   
  MASTER_PORT=3306,
  MASTER_LOG_FILE=' mysql-bin.000002',  
  MASTER_LOG_POS=328；
开始复制  start slave
查看线程  show slave status \G
在一开始主机设置重复上面的操作
执行复制
CHANGE MASTER TO
  MASTER_HOST='172.18.133.27',  
  MASTER_USER='repluser',      
  MASTER_PASSWORD='123456',   
  MASTER_PORT=3306,
  MASTER_LOG_FILE=' mysql-bin.000004',  
  MASTER_LOG_POS=328；
开始复制  start slave
--------------------------------------------------------------------------------
半同步复制 
一主多从
主服务器配置
[mysql]
server-id=1
log-bin
从服务器配置
[mysql]      x2
server-id=2
read-only
主
创建用户
grant replication slave on *.* to rpl@'172.18.133.%' identified by '123456’；
从
同步用户   主服务器上的二进制位置
CHANGE MASTER TO
  MASTER_HOST='172.18.133.27',  
  MASTER_USER='repluser',      
  MASTER_PASSWORD='123456',   
  MASTER_PORT=3306,
  MASTER_LOG_FILE=' mysql-bin.000004',  
  MASTER_LOG_POS=328；
开始复制  start slave
第二个从一样的操作
在主服务器在导入数据库 其他俩个从服务器也会同步
实现半同步
1在主服务器上安装master插件      查看插件 show plugins;
mysql> INSTALL PLUGIN rpl_semi_sync_master SONAME 'semisync_master.so'; 
2启用插件    查看是否启用  select @@rpl_semi_sync_master_enabled;
mysql>SET GLOBAL rpl_semi_sync_master_enabled=1; 
永久生效写入配置文件中  /etc/my.cnf
rpl_semi_sync_master_enabled 
3 mysql>SET GLOBAL rpl_semi_sync_master_timeout = 1000;超时长为1s 
mysql>SHOW GLOBAL VARIABLES LIKE '%semi%';    查看变量
mysql>SHOW GLOBAL STATUS LIKE '%semi%‘;   查看状态
4从服务器配置:   x2
安装插件
mysql> INSTALL PLUGIN rpl_semi_sync_slave SONAME 'semisync_slave.so'; 
启用插件
mysql> SET GLOBAL rpl_semi_sync_slave_enabled=1; 永久生效同上
5重新启动线程
在主服务器上修改表 看从服务器同步   insert teachers (name)values('c'）；
--------------------------------------------------------------------------------
MySQL复制 
白名单
replicate_do_db=    指定复制库的白名单      加库的名字     写入从服务器配置中 只有这个库能同步主服务器上面的文件
 binlog_ignore_db =   数据库黑名单列表 
在主服务器配置文件中加入 binlog_do_db=  数据库的名字     在他的从服务器上只同步这个库的东西   
replicate_ignore_db=   指定复制库黑名单 
遇到错误可以先跳过这个错误、sql_slave_skip_counter = N  从服务器忽略几个主服务器的复制事件， global变量      先关闭线程   然后再开启
--------------------------------------------------------------------------------
MySQL复制加密 
配置实现：  参看：https://mariadb.com/kb/en/library/replication-with-secureconnections/ 
主服务器开启SSL：[mysqld] 加一行ssl 
主服务器配置证书和私钥；并且创建一个要求必须使用SSL连接的复制账号 
从服务器使用CHANGER MASTER TO 命令时指明ssl相关选项 

主服务器 
cd /etc/my.cnf.d/
建立一个目录  存放ca 文件  mkdir ssl   
cd /ssl
生成私钥 
(umask 066;openssl genrsa 2048 > cakey.pem)
 生成自签名证书
openssl req -new -x509 -key cakey.pem -out cacert.pem -days 3650

给主服务器申请证书
openssl req -newkey rsa: -days 365 -nodes -keyout  master.key  >master.csr

给从服务器申请证书
openssl req -newkey rsa: -days 365 -nodes -keyout  slave.key  >slave.csr

颁发证书
openssl x509 -req -in master.csr -days 365  -CA cacert.pem -CAkey  cakey.pem -set_serial 01 > master.crt

查看证书
openssl x509 -in master.crt -noout --text;
颁发证书
openssl x509 -req -in slave.csr -days 365  -CA cacert.pem -CAkey  cakey.pem -set_serial 02 > slave.crt

 验证证书的有效性
openssl verify -CAfile cacert.pem master.crt 
把相应的证书复制到从服务器
在从服务器上建文件
mkdir /etc/my.cnf/ssl/
在主服务器上复制到从服务器证书
scp cacert.pem slave.crt slave.key 172.18.134.253:/etc/my.cnf/ssl/

主服务器配置证书
查看ssl是否启用
show variables like '%ssl%';
启动配置 vim /etc/my.cnf
[mysqld]
log-bin
server_id=1 
ssl 
ssl-ca=/etc/my.cnf.d/ssl/cacert.pem 
ssl-cert=/etc/my.cnf.d/ssl/master.crt 
ssl-key=/etc/my.cnf.d/ssl/master.key 

创建用户
grant replication slave 0n *.* to rplssl@'172.18.134.%' identified by '123456' require ssl;

从服务器登录


从服务器停止线程
stop slave;
清除
reset slave all;
重新建立主从关系
查看主服务器二进制日志 选择 二进制位置  执行
mysql> CHANGE MASTER TO 
 MASTER_HOST='172.18.1334.253', 
 MASTER_USER='rplssl', 
 MASTER_PASSWORD='centos', 
MASTER_PORT=3306,
MASTER_LOG_FILE='mariadb-bin.000001',
 MASTER_LOG_POS=245, 
MASTER_SSL=1,  
MASTER_SSL_CA = '/etc/my.cnf.d/ssl/cacert.pem',  
MASTER_SSL_CERT = '/etc/my.cnf.d/ssl/slave.crt',  
MASTER_SSL_KEY = '/etc/my.cnf.d/ssl/slave.key'; 
启动线程复制   start slave
查看状态 show slave  status\G
--------------------------------------------------------------------------------
复制的监控和维护 
(1) 清理日志   PURGE { BINARY | MASTER } LOGS   { TO 'log_name' | BEFORE  datetime_expr }   
RESET MASTER  
RESET SLAVE 
(2) 复制监控         
SHOW MASTER STATUS          
SHOW BINLOG EVENTS          
SHOW BINARY LOGS          
SHOW SLAVE STATUS          
SHOW PROCESSLIST 
--------------------------------------------------------------------------------
MySQL读写分离 ProxySQL 
基于YUM仓库安装 
     cat <<EOF | tee /etc/yum.repos.d/proxysql.repo 
     [proxysql_repo] 
     name= ProxySQL YUM repository 
     baseurl=http://repo.proxysql.com/ProxySQL/proxysql-1.4.x/centos/\$releasever 
     gpgcheck=1 
     gpgkey=http://repo.proxysql.com/ProxySQL/repo_pub_key
     EOF
基于RPM下载安装：https://github.com/sysown/proxysql/releases 
ProxySQL组成  
     服务脚本：/etc/init.d/proxysql  
     配置文件：/etc/proxysql.cnf  
     主程序：/usr/bin/proxysql 
实验  
环境配置    主从复制环境  一个调度器

从服务器配置必须加 read-only 
在另一台机配置调度器  34.7 
安装 配置yum
cat <<EOF | tee /etc/yum.repos.d/proxysql.repo 
     [proxysql_repo] 
     name= ProxySQL YUM repository 
     baseurl=http://repo.proxysql.com/ProxySQL/proxysql-1.4.x/centos/\$releasever 
     gpgcheck=1 
     gpgkey=http://repo.proxysql.com/ProxySQL/repo_pub_key
     EOF
 安装    yum  install proxysql
启动数据库服务器 service proxysql start 
6032：ProxySQL的管理端
6033：ProxySQL对外提供服务的端口 
连接数据库  ，mysql  -uadmin -padmin -h127.0.0.1 -p 6032
说明：在main和monitor数据库中的表， runtime_开头的是运行时的配置，不 能修改，只能修改非runtime_表，修改后必须执行LOAD … TO RUNTIME才能加 载到RUNTIME生效，执行save … to disk将配置持久化保存到磁盘 

向ProxySQL中添加MySQL节点，以下操作不需要use main也可成功 
MySQL > select * from  mysql_servers;    
MySQL > insert into mysql_servers(hostgroup_id,hostname,port) values(10,'192.168.8.17',3306);  
MySQL > insert into mysql_servers(hostgroup_id,hostname,port) values(10,'192.168.8.27',3306); 
MySQL > load mysql servers to runtime;   生效 
MySQL > save mysql servers to disk;    保存 

在主服务器上创建账号
mysql> grant replication client on *.* to monitor@'192.168.34.17%'  identified by 'magedu'; 

ProxySQL上配置监控
MySQL [(none)]> set mysql-monitor_username='monitor'; 
MySQL [(none)]> set mysql-monitor_password='magedu'; 
加载到RUNTIME，并保存到disk 
MySQL [(none)]> load mysql variables to runtime; 
MySQL [(none)]> save mysql variables to disk; 

监控模块的指标保存在monitor库的log表中 查看监控连接是否正常的 (对connect指标的监控)：(如果connect_error的结果 为NULL则表示正常)
             mysql> select * from mysql_server_connect_log; 
查看监控心跳信息 (对ping指标的监控)： 
            mysql> select * from mysql_server_ping_log; 
查看read_only和replication_lag的监控日志 
            mysql> select * from mysql_server_read_only_log; 
            mysql> select * from mysql_server_replication_lag_log; 

 设置分组信息 
 需要修改的是main库中的mysql_replication_hostgroups表，该表有3个字段： writer_hostgroup，reader_hostgroup，comment, 指定写组的id为10，读组的id为20 
增加组 insert into mysql_replication_hostgroups values(10,20,"test"); 
将mysql_replication_hostgroups表的修改加载到RUNTIME生效
load mysql servers to runtime; 
save mysql servers to disk; 
Monitor模块监控后端的read_only值，按照read_only的值将节点自动移动到读/写组 
select hostgroup_id,hostname,port,status,weight from mysql_servers;


在master节点上创建访问用户 
grant all on *.* to sqluser@'192.168.34.%' identified by 'magedu'; 

在ProxySQL配置，将用户sqluser添加到mysql_users表中， default_hostgroup默认 组设置为写组10，当读写分离的路由规则不符合时，会访问默认组的数据库 
insert into mysql_users(username,password,default_hostgroup) values('sqluser','magedu',10); 
load mysql users to runtime; 
save mysql users to disk; 

使用sqluser用户测试是否能路由到默认的10写组实现读、写数据 用户端提供连接Proxsql
mysql -usqluser -pmagedu -P6033 -h127.0.0.1 -e 'select @@server_id'  
mysql -usqluser -pmagedu -P6033 -h127.0.0.1 -e 'create database testdb'           
mysql -usqluser -pmagedu -P6033 -h127.0.0.1 -e 'use testdb;create table t(id int)' 

配置路由规则，实现读写分离 
与规则有关的表：mysql_query_rules和mysql_query_rules_fast_routing，后 者是前者的扩展表，1.4.7之后支持 
插入路由规则：将select语句分离到20的读组，select语句中有一个特殊语句 SELECT...FOR UPDATE它会申请写锁，应路由到10的写组 
insert into mysql_query_rules  
(rule_id,active,match_digest,destination_hostgroup,apply)VALUES  (1,1,'^SELECT.*FOR UPDATE$',10,1),(2,1,'^SELECT',20,1);          定义规则
load mysql query rules to runtime; 
save mysql query rules to disk; 
注意：因ProxySQL根据rule_id顺序进行规则匹配，select ... for update规则的 rule_id必须要小于普通的select规则的rule_id 

测试读操作是否路由给20的读组 
mysql -usqluser -pmagedu -P6033 -h127.0.0.1 -e 'select @@server_id' 
测试写操作，以事务方式进行测试 
mysql -usqluser -pmagedu -P6033 -h127.0.0.1 \ 
-e 'start transaction;select @@server_id;commit;select @@server_id' 
mysql -usqluser -pmagedu -P6033 -h127.0.0.1 -e 'insert testdb.t values (1)' 
mysql -usqluser -pmagedu -P6033 -h127.0.0.1 -e 'select id from  testdb.t'   
路由的信息：查询stats库中的stats_mysql_query_digest表 
SELECT hostgroup hg,sum_time, count_star, digest_text FROM stats_mysql_query_digest  ORDER BY sum_time DESC；


--------------------------------------------------------------------------------
MHA
MHA工作原理 
1 从宕机崩溃的master保存二进制日志事件（binlog events） 
2 识别含有最新更新的slave 
3 应用差异的中继日志（relay log）到其他的slave 
4 应用从master保存的二进制日志事件（binlog events） 
5 提升一个slave为新的master 6 使其他的slave连接新的master进行复制 
实验       （环境配置）
4台机器  17是主服务器 27 和37 是从服务器  另外7是master 用于监控
在主从服务器上加一个  忽略名称解析 skip-name-resolv=1    （etc/my.cnf)
  
主        实现Master     
 vim /etc/my.cnf      
[mysqld] 
log-bin 
server_id=1 
skip_name_resolve=1  
 
 建立账户
mysql>show master logs 
mysql>grant  replication slave 0n *.* to rpluser@'172.18.34.%' identified by '123456'；
建立一个管理账户  以后用来提升从节点为主节点
mysql>grant all on *.* to mhauser@'172.18.133.%’identified by'123456'; 

从  （ 实现slave ）   
 vim /etc/my.cnf
 [mysqld]
server_id=2    不同节点此值各不相同 
log-bin 
read_only 
relay_log_purge=0       中继日志默认清除 =0 不清除   
skip_name_resolve=1 
  设置同步
mysql> CHANGE MASTER TO 
 MASTER_HOST='172.18.34.17', 
 MASTER_USER='rpluser', 
 MASTER_PASSWORD='centos', 
MASTER_PORT=3306,
MASTER_LOG_FILE='mariadb-bin.000001',
 MASTER_LOG_POS=245, 
mysql > start slave  开启复制

同步时间 ntpdate 172.18.0.1    全部都

管理机 (7)
 在管理节点建立配置文件   （放在哪不重要） 
 vim /etc/mastermha/app1.cnf 
 [server default]
 user=mhauser
 password=123456
 manager_workdir=/data/mastermha/app1/ 
manager_log=/data/mastermha/app1/manager.log 
remote_workdir=/data/mastermha/app1/ 
ssh_user=root 
repl_user=repluser 
repl_password=123456
ping_interval=1 
 
[server1]
   hostname=192.168.8.17
   candidate_master=1     可以当主服务器 
[server2] 
   hostname=192.168.8.27 
   candidate_master=1 
[server3] 
  hostname=192.168.8.37 
 candidate_master=1 
在所有节点实现相互之间ssh key验证 
(7) ssh-keygen
cd .ssh
ssh-copy-id 172.18.133.7 复制到本机 生成一个公钥文件
复制到所有的机上
scp -rp /root/.ssh 172.18.133.17;/root/
scp -rp /root/.ssh 172.18.133.27;/root/
scp -rp /root/.ssh 172.18.133.37;/root/

管理机（7）上安装Manager工具包和Node工具包 
被管理（17.27.37，）上安装Node工具包 
:https://code.google.com/archive/p/mysql-master-ha/   官网下载包
在管理节点上安装两个包：(7)  
mha4mysql-manager     
mha4mysql-node 
在被管理节点安装：(17,27,37)   
mha4mysql-node 

管理机  （7)
Mha验证和启动 
masterha_check_ssh --conf=/etc/mastermha/app1.cnf    检查
masterha_check_repl --conf=/etc/mastermha/app1.cnf     检查
masterha_manager --conf=/etc/mastermha/app1.cnf     启动  最好在后台   就执行一次就结束另 

排错日志：     /data/mastermha/app1/manager.log 

假设主服务器 坏了‘掉电 关机
会提升一个新的主服务器   查看日志里面有内容  提升的是谁
如果修复以前的主服务器 开机也不会 再继续当主   可以把它加入到从服务器中


--------------------------------------------------------------------------------
Galera Cluster 
MariaDB Galera Cluster    
参考仓库：https://mirrors.tuna.tsinghua.edu.cn/mariadb/mariadb-5.5.62/yum/centos7-amd64/ 
实验环境  3台机器   7， 17， 27    都是主服务器  平等  都是主都是从 互相同步
全部配置yum 
vim /etc/yum.repos.d/mysql.repo
[mysql]
baseurl=https://mirrors.tuna.tsinghua.edu.cn/mariadb/mariadb-5.5.62/yum/centos7-amd64/ 
gpgcheck=0
安装
yum install MariaDB-Galera-server 
全部修改这个配置文件
配置文件
vim  /etc/my.cnf.d/server.cnf     
[galera] 
wsrep_provider = /usr/lib64/galera/libgalera_smm.so        提供文件路径
wsrep_cluster_address="gcomm://172.18.133.7,172.18.134.17,172.18.134.27"   包括服务器IP
binlog_format=row     二进制格式行
default_storage_engine=InnoDB 
innodb_autoinc_lock_mode=2 
bind-address=0.0.0.0 
下面配置可选项
wsrep_cluster_name = ‘mycluster‘  默认my_wsrep_cluster 
wsrep_node_name = 'node1' 
wsrep_node_address = ‘192.168.8.7’ 
可以复制过去
scp server.cnf 172.18.137.17:/etc/my.cnf/server.cnf

启动服务
首次启动时，需要初始化集群，在其中一个节点上执行命令     
/etc/init.d/mysql start --wsrep-new-cluster 
而后正常启动其它节点     
service mysql start 
查看集群中相关系统变量和状态变量    
SHOW VARIABLES LIKE 'wsrep_%‘;    
SHOW STATUS LIKE 'wsrep_%‘;    
SHOW STATUS LIKE 'wsrep_cluster_size‘; 

创建一个表create table t1(id int auto_increment primary key,name char(20));
都同步到别的表  可以在表中添加记录   看看其他表有没有复制过来
在所有服务器上添加同一个命令  自动添加递增序号


mysqlslap
单线程测试    
mysqlslap -a -uroot -pmagedu 
多线程测试。使用–concurrency来模拟并发连接    
mysqlslap -a -c 100 -uroot -pmagedu 
迭代测试。用于需要多次执行测试得到平均值    
mysqlslap -a -i 10 -uroot -pmagedu    
mysqlslap ---auto-generate-sql-add-autoincrement -a  
mysqlslap -a --auto-generate-sql-load-type=read    
mysqlslap -a --auto-generate-secondary-indexes=3    
mysqlslap -a --auto-generate-sql-write-number=1000    
mysqlslap --create-schema world -q "select count(*) from City”    
mysqlslap -a -e innodb -uroot -pmagedu    
mysqlslap -a --number-of-queries=10 -uroot -pmagedu 
测试同时不同的存储引擎的性能进行对比  
 mysqlslap -a --concurrency=50,100 --number-of-queries 1000 -iterations=5 --engine=myisam,innodb --debug-info -uroot -pmagedu 
执行一次测试，分别50和100个并发，执行1000次总查询  
mysqlslap -a --concurrency=50,100 --number-of-queries 1000 --debuginfo -uroot -pmagedu 
50和100个并发分别得到一次测试结果(Benchmark)，并发数越多，执行完所 有查询的时间越长。为了准确起见，可以多迭代测试几次  
mysqlslap -a --concurrency=50,100 --number-of-queries 1000 -iterations=5 --debug-info -uroot -pmagedu 
--------------------------------------------------------------------------------
生产环境my.cnf配置示例 
 硬件：内存32G
 innodb_file_per_table = 1 
打开独立表空间 
max_connections = 8000  #MySQL 服务所允许的同时会话数的上限，经常出现Too Many Connections的错误提 示，则需要增大此值 
back_log = 300 #back_log 是操作系统在监听队列中所能保持的连接数 
max_connect_errors = 1000 #每个客户端连接最大的错误允许数量，当超过该次数，MYSQL服务器将禁止此主机的连 接请求，直到MYSQL服务器重启或通过flush hosts命令清空此主机的相关信息 
open_files_limit = 10240 #所有线程所打开表的数量 
 max_allowed_packet = 32M #每个连接传输数据大小.最大1G，须是1024的倍数，一般设为最大的BLOB的值 
wait_timeout = 10 #指定一个请求的最大连接时间 
sort_buffer_size = 16M  #排序缓冲被用来处理类似ORDER BY以及GROUP BY队列所引起的排序 
join_buffer_size = 16M  #不带索引的全表扫描.使用的buffer的最小值 
query_cache_size = 128M #查询缓冲大小 
query_cache_limit = 4M     #指定单个查询能够使用的缓冲区大小，缺省为1M 
transaction_isolation = REPEATABLE-READ  # 设定默认的事务隔离级别 
thread_stack = 512K  # 线程使用的堆大小. 此值限制内存中能处理的存储过程的递归深度和SQL语句复杂性， 此容量的内存在每次连接时被预留. 
log-bin # 二进制日志功能 
binlog_format=row #二进制日志格式
 innodb_buffer_pool_size = 24G  #InnoDB使用一个缓冲池来保存索引和原始数据, 可设置这个变量到服务器物理内存大小 的80%
 innodb_file_io_threads = 4 #用来同步IO操作的IO线程的数量  
innodb_thread_concurrency = 16 #在InnoDb核心内的允许线程数量，建议的设置是CPU数量加上磁盘数量的两倍
 innodb_log_buffer_size = 16M # 用来缓冲日志数据的缓冲区的大小
 innodb_log_file_size = 512M  在日志组中每个日志文件的大小 
innodb_log_files_in_group = 3 # 在日志组中的文件总数
 innodb_lock_wait_timeout = 120 # SQL语句在被回滚前,
InnoDB事务等待InnoDB行锁的时间
 long_query_time = 2 #慢查询时长 
log-queries-not-using-indexes #将没有使用索引的查询也记录下来
