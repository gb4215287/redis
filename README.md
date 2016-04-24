# redis
1.     redis简介      

               Redis是个高性能的key-value数据库，它的key具有丰富的数据结构：string，hash，list set和sorted set。作为NOSQL，比起memcache之类，不仅仅key数据结构丰富，而且具有持久化的功能，并且能够支持主从复制，很方便构建集群。redis高性能很大程度上源于它是个内存型数据库，它的高性能表现在：set操作11w/s，get操作8.1w/s，与其他类型数据库性能差异。为了进一步加深对redis的理解总结，我打算写个redis系列的博客。这里主要谈谈redis安装部署及运维维护。
2.下载安装
[root@localhost newenv]# wget download.redis.io/releases/redis-2.6.15.tar.gz
[root@localhost newenv]# tar -xvf redis-2.6.15.tar.gz
[root@localhost newenv]# cd redis-2.6.15
[root@localhost redis-2.6.15]# make
[root@localhost redis-2.6.15]# cd src && make install

3.上面的基本就安装成功了,下面是移动文件,便于管理
[root@localhost redis-2.6.15]# mkdir -p /usr/local/redis/bin
[root@localhost redis-2.6.15]# mkdir -p /usr/local/redis/etc
移动redis配置文件如下所示:
[root@localhost redis-2.6.15]# mv redis.conf /usr/local/redis/etc/
[root@localhost src]# mv mkreleasehdr.sh redis-benchmark redis-check-aof redis-check-dump redis-cli redis-server /usr/local/redis/bin/

4.启动redis服务
[root@localhost src]# /usr/local/redis/bin/redis-server /usr/local/redis/etc/redis.conf

5.开启redis的后台运行
[root@localhost src]# vim /usr/local/redis/etc/redis.conf
将daemonize的值改为yes.

6.客户端连接
[root@localhost src]# /usr/local/redis/bin/redis-cli 
redis 127.0.0.1:6379> 

7.停止redis实例
[root@localhost src]# /usr/local/redis/bin/redis-cli shutdown
也可以用pkill redis-server

注意:
启动 redis 会出现的问题

Warning: 32 bit instance detected but no memory limit set. Setting 3 GB maxmemory limit with 'noeviction' policy now.

解决方法：修改配置文件 redis.conf  将 maxmemory 设置为 maxmemory 1024000000 #分配256M内存

WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.

解决方法：警告：过量使用内存设置为0！在低内存环境下，后台保存可能失败。为了修正这个问题，请在/etc/sysctl.conf 添加一项 'vm.overcommit_memory = 1' ，然后重启（或者运行命令'sysctl vm.overcommit_memory=1' ）使其生效。

当启动的时候没有任何信息，表明启动成功。也可以使用 "netstat -tnl" 查看6379端口是否启动
