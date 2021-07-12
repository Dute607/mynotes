[常用命令](#常用命令)

### 常用命令
````shell
which bash #查看bash路径
cat /etc/shells #查看当前支持的shell
history #查看历史命令
````

### 编辑环境变量
* 当前用户
  ````shell
  vim ~/.bash_profile
  source ~/.bash_profile
  ````
* 所有用户
  ````shell
  vim /etc/profile
  source /etc/profile
  ````
### Linux安装oracle客户端
1. 下载oracle客户端安装包：  
  http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html  
  instantclient-basic-linux.x64-21.1.0.0.0.zip  
  instantclient-sqlplus-linux.x64-21.1.0.0.0.zip  
   <br/>
   安装解压缩到/usr/local/oracle  
   ````shell
   mkdir /usr/local/oracle
   unzip /usr/local/oracle/instantclient-basic-linux.x64-11.2.0.4.0.zip
   unzip /usr/local/oracle/instantclient-sqlplus-linux.x64-11.2.0.4.0.zip
   ````
2. 创建监听程序
    ````shell
    cd /usr/local/oracle/instantclient_11_2
    mkdir -p network/admin
    cd network/admin
    ````
    新建tnsnames.ora文件：
    ````
    ORCL =
      (DESCRIPTION =
        (ADDRESS = (PROTOCOL = TCP)(HOST = 10.12.11)(PORT = 1521))
        (CONNECT_DATA =
          (SERVER = DEDICATED)
          (SERVICE_NAME = orcl)
        )
      )
    ````
3. 添加环境变量
   ````shell
   vi ~/.bash_profile
   ````
    ````shell
    export ORACLE_HOME=/usr/local/oracle/instantclient_11_2
    export TNS_ADMIN=$ORACLE_HOME/network/admin
    ##export NLS_LANG=AMERICAN_AMERICA.ZHS16GBK
    export NLS_LANG=AMERICAN_AMERICA.AL32UTF8
    export LD_LIBRARY_PATH=$ORACLE_HOME
    export PATH=$ORACLE_HOME:$PATH
    ````
   执行环境变量
   ```shell
   source ~/.bash_profile
   ```
   测试数据库连接
   ```shell
   sqlplus username/passwd@orcl
   ```
4. 可能出现的问题
    * 升级glibc服务：  
      https://blog.csdn.net/wngpenghao/article/details/87918493  
      查看服务器执行的glibc版本
      ```shell
      strings /lib64/libc.so.6 |grep GLIBC_  
      ```
      下载
      ```shell
      wget http://ftp.gnu.org/gnu/glibc/glibc-2.14.tar.gz
      wget http://ftp.gnu.org/gnu/glibc/glibc-ports-2.14.tar.gz
      ```
      解压
      ```shell
      tar -xvf  glibc-2.14.tar.gz
      tar -xvf  glibc-ports-2.14.tar.gz
      ```
      编译(时长较久)
      ```shell
      mv glibc-ports-2.14 glibc-2.14/ports
      mkdir glibc-2.14/build
      cd glibc-2.14/build
      ../configure  --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin
      make
      ```
      安装
      ```shell
      make install
      ```
      查看软件链接指向
      ```shell
      ll /lib64/libc.so.6
      ```
