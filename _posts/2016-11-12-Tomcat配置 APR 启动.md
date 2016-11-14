###Tomcat7 配置 APR 模式启动

>具体的配置环境为 Centos7、JDK1.7、Tomcat8。要为tomcat配置 APR 启动模式需要如下系统配置。

```
yum install apr-devel	需要安装
yum install openssl-devel	需要1.0.2或以上的版本
yum install gcc	系统已有
yum install make		系统已有

```
>首先看一下当前系统 openssl 的版本,如果是 1.0.2 及以上的版本则无需再进行安装，否则需要进行安装。

```
openssl version -a
```
> 执行如下命令进行 openssl 的安装

```
wget https://www.openssl.org/source/openssl-1.0.2-latest.tar.gz
tar -zxvf openssl-1.0.2-latest.tar.gz
cd openssl-1.0.2j

./config --prefix=/usr/local/openssl
make
make install
```
> 接下里需要对openssl进行配置，进行软连配置

```
ln -s /usr/local/openssl/bin/openssl /usr/bin/openssl

配置APR本地库到系统共享库搜索路径中
cp /usr/local/apr/lib/libtcnative* /usr/lib/

如果 /usr/bin/openssl 已经存在，则将将原有的进行备份
mv /usr/bin/openssl /usr/bin/openssl.bak
```
> 接下来需要对 tomcat 的 server.xml 进行配置，具体配置如下：

```
Once the libraries are properly installed and available to Java (if loading fails, the library path will be displayed), the Tomcat connectors will automatically use APR.

根据 tomcat 官方文档描述，在配置好以上的环境后，tomcat Connectors 会自动切换到 APR 模式
```
> 接下来启动 Tomcat 来检查是否配置成功
```
十一月 13, 2016 11:36:28 上午 org.apache.coyote.AbstractProtocol start
信息: Starting ProtocolHandler ["http-apr-8080"] 
十一月 13, 2016 11:36:29 上午 org.apache.coyote.AbstractProtocol start
信息: Starting ProtocolHandler ["ajp-apr-8009"]
十一月 13, 2016 11:36:29 上午 org.apache.catalina.startup.Catalina start
```

> 下面是 tomcat 线程池参数说明

```
maxThreads			最大并发数
minSpareThreads		初始化时创建的线程数
maxSpareThreads		一旦创建的线程超过这个值，Tomcat就会关闭不再需要的线程
acceptCount			指定当所有可以使用的处理请求的线程数都被使用时，可以放到处理队列中的请求数，超过这个数的请求将不予处理
```
