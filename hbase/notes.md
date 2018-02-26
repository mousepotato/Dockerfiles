- 配置


    - `make`
    - `make run`
    - `docker ps` # to get Container ID

    
    ```
8b756853c298        harisekhon/hbase:1.3   "/bin/sh -c \"/entryp…"   28 seconds ago      Up 29 seconds       0.0.0.0:2181->2181/tcp, 0.0.0.0:8080->8080/tcp, 0.0.0.0:8085->8085/tcp, 0.0.0.0:9090->9090/tcp, 0.0.0.0:9095->9095/tcp, 0.0.0.0:16000->16000/tcp, 0.0.0.0:16010->16010/tcp, 0.0.0.0:16201->16201/tcp, 0.0.0.0:16301->16301/tcp   unruffled_almeida
    ```
    
    - `sudo vi /etc/hosts` then add `127.0.0.1 8b756853c298`

    - `docker exec -it 8b756853c298 bash`


- JAVA API

    - set `hbase.zookeeper.quorum` to Container ID `8b756853c298`

```
        conf = HBaseConfiguration.create();
        conf.set("hbase.zookeeper.quorum", "8b756853c298");
        conf.set("hbase.zookeeper.property.clientPort", "2181");
```

- Web UI
    - `http://localhost:16010/master-status`   
    - `http://localhost:8085/rest.jsp`
    - `http://localhost:8085/conf`
    - `http://localhost:8080/`


- zk 使用

```
$==> echo stat|nc 127.0.0.1 2181
Zookeeper version: 3.4.6-1569965, built on 02/20/2014 09:09 GMT
Clients:
 /127.0.0.1:47076[1](queued=0,recved=31,sent=31)
 /127.0.0.1:47078[1](queued=0,recved=98,sent=98)
 /127.0.0.1:47070[1](queued=0,recved=2130,sent=2157)
 /127.0.0.1:47074[1](queued=0,recved=30,sent=30)
 /127.0.0.1:47080[1](queued=0,recved=32,sent=32)
 /172.17.0.1:50610[1](queued=0,recved=3,sent=3)
 /127.0.0.1:47106[1](queued=0,recved=29,sent=29)
 /127.0.0.1:47072[1](queued=0,recved=36,sent=36)
 /127.0.0.1:47084[1](queued=0,recved=30,sent=30)
 /172.17.0.1:50612[0](queued=0,recved=1,sent=0)

Latency min/avg/max: 0/0/15
Received: 2465
Sent: 2491
Connections: 10
Outstanding: 0
Zxid: 0x65
Mode: standalone
Node count: 41
```

```
sli@slimbp13:[/Users/sli/dev/zookeeper-3.4.11/bin] @23:00
$==> zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /Users/sli/dev/zookeeper-3.4.11/bin/../conf/zoo.cfg
Mode: standalone
```

 - `zkCli.sh -server 192.168.255.133:2181` 连接某个server

```
[zk: localhost:2181(CONNECTED) 2] ls /
[zookeeper, hbase]
[zk: localhost:2181(CONNECTED) 3] get /hbase

cZxid = 0x2
ctime = Sun Feb 25 22:45:26 PST 2018
mZxid = 0x2
mtime = Sun Feb 25 22:45:26 PST 2018
pZxid = 0x3c
cversion = 17
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 0
numChildren = 17
```

```
[zk: localhost:2181(CONNECTED) 6] ls /hbase
[replication, meta-region-server, rs, splitWAL, backup-masters, table-lock, flush-table-proc, region-in-transition, online-snapshot, master, switch, running, recovering-regions, draining, namespace, hbaseid, table]
```


- 查看hbase下面的表名

```
[zk: localhost:2181(CONNECTED) 18] ls /hbase/table
[hbase:meta, hbase:namespace, student, customer]
```


- 传递四个字母的字符串给ZooKeeper，ZooKeeper会返回一些有用的信息。

```
sli@slimbp13:[/Users/sli/dev/zookeeper-3.4.11/bin] @23:00
$==> echo conf |nc 127.0.0.1 2181
clientPort=2181
dataDir=/tmp/hbase-root/zookeeper/version-2
dataLogDir=/tmp/hbase-root/zookeeper/version-2
tickTime=3000
maxClientCnxns=300
minSessionTimeout=6000
maxSessionTimeout=90000
serverId=0
```

```
ZooKeeper 四字命令 功能描述

conf 输出相关服务配置的详细信息。

cons 列出所有连接到服务器的客户端的完全的连接 / 会话的详细信息。包括“接受 / 发送”的包数量、会话 id 、操作延迟、最后的操作执行等等信息。

dump 列出未经处理的会话和临时节点。

envi 输出关于服务环境的详细信息（区别于 conf 命令）。

reqs 列出未经处理的请求

ruok 测试服务是否处于正确状态。如果确实如此，那么服务返回“imok ”，否则不做任何相应。

stat 输出关于性能和连接的客户端的列表。

wchs 列出服务器 watch 的详细信息。

wchc 通过 session 列出服务器 watch 的详细信息，它的输出是一个与watch 相关的会话的列表。

wchp 通过路径列出服务器 watch 的详细信息。它输出一个与 session相关的路径。
```


