## 集群

机器性能有限，使用VMware装的整个虚拟机

主机 | ip |角色（端口）
---|---|---
node1 |	192.168.192.161 |	rocketmq-ns1（9876）rocketmq-bs-m1（10911）
node2 |	192.168.192.162 |	rocketmq-ns2（9876） rocketmq-bs-m2（10911）rocketmq-bs-s1（11011）
node3 |192.168.192.163 |	rocketmq-ns3（9876）rocketmq-bs-s2（10911）rockermq-console（8080）

## 所有机器 host

```
192.168.192.161 node1
192.168.192.162 node2
192.168.192.163 node3

192.168.192.161 rocketmq-ns1
192.168.192.162 rocketmq-ns2
192.168.192.163 rocketmq-ns3

192.168.192.161 rocketmq-bs-m1
192.168.192.162 rocketmq-bs-m2 rocketmq-bs-s1
192.168.192.163 rocketmq-bs-s2
```

### node1
```bash
mkdir -p /usr/local/rocketmq/store/master1
mkdir -p /usr/local/rocketmq/store/master1/commitlog
mkdir -p /usr/local/rocketmq/store/master1/consumequeue
mkdir -p /usr/local/rocketmq/store/master1/index
cp /opt/rocketmq-4.8.0/conf/m1.properties /usr/local/rocketmq/conf/m1.properties
```

### node2
```bash
mkdir -p /usr/local/rocketmq/store/master2
mkdir -p /usr/local/rocketmq/store/master2/commitlog
mkdir -p /usr/local/rocketmq/store/master2/consumequeue
mkdir -p /usr/local/rocketmq/store/master2/index
mkdir -p /usr/local/rocketmq/store/master1-slave1
mkdir -p /usr/local/rocketmq/store/master1-slave1/commitlog
mkdir -p /usr/local/rocketmq/store/master1-slave1/consumequeue
mkdir -p /usr/local/rocketmq/store/master1-slave1/index
cp /opt/rocketmq-4.8.0/conf/m2.properties /usr/local/rocketmq/conf/m2.properties
cp /opt/rocketmq-4.8.0/conf/m1-s1.properties /usr/local/rocketmq/conf/m1-s1.properties
```

### node3
```bash
mkdir -p /usr/local/rocketmq/store/master2-slave1
mkdir -p /usr/local/rocketmq/store/master2-slave1/commitlog 
mkdir -p /usr/local/rocketmq/store/master2-slave1/consumequeue
mkdir -p /usr/local/rocketmq/store/master2-slave1/index
cp /opt/rocketmq-4.8.0/conf/m2-s1.properties /usr/local/rocketmq/conf/m2-s1.properties
```

## 执行命令

1. node1

```bash
nohup sh /opt/rocketmq-4.8.0/bin/mqbroker -c /usr/local/rocketmq/conf/m1.properties &
nohup sh  /opt/rocketmq-4.8.0/bin/mqnamesrv &
```

2. node2
```bash
nohup sh  /opt/rocketmq-4.8.0/bin/mqnamesrv &
nohup sh /opt/rocketmq-4.8.0/bin/mqbroker -c /usr/local/rocketmq/conf/m1-s1.properties &
nohup sh /opt/rocketmq-4.8.0/bin/mqbroker -c /usr/local/rocketmq/conf/m2.properties &
```

3. node3

```bash
nohup sh  /opt/rocketmq-4.8.0/bin/mqnamesrv &
nohup sh /opt/rocketmq-4.8.0/bin/mqbroker -c /usr/local/rocketmq/conf/m2-s1.properties &
```

