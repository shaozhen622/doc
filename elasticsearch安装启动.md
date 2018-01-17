创建elasticsearch用户
1下载
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.1.1.tar.gz
2解压
 tar xvf elasticsearch-6.1.1.tar.gz
3，修改端口为9200
4，后台启动
./home/elasticsearch/elasticsearch-6.1.1/bin/elasticsearch -d
备注：
1，要访问主机的ip,需要修改config/elasticsearch.yml文件 
network.host: 0.0.0.0

修改
解决 max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]问题
	切换到root用户修改配置sysctl.conf 
	vi /etc/sysctl.conf  
	添加下面配置： 
	vm.max_map_count=655360
	括号内的内容，不确定是否有效(
		
			修改config/elasticsearch.yml文件，找到如下配置，开放discovery.zen.ping.unicast.hosts及discovery.zen.minimum_master_nodes
			   修改为discovery.zen.ping.unicast.hosts["0.0.0.0"]
			   discovery.zen.minimum_master_nodes: 1
	)

解决 max file descriptors [4096] for elasticsearch process likely too low, increase to at least [65536] 问题
	[root@localhost ~]# cp /etc/security/limits.conf /etc/security/limits.conf.bak
	[root@localhost ~]# cat /etc/security/limits.conf | grep -v "elasticsearch" > /tmp/system_limits.conf
	[root@localhost ~]# echo "elasticsearch hard nofile 65536" >> /tmp/system_limits.conf 
	[root@localhost ~]# echo "elasticsearch soft nofile 65536" >> /tmp/system_limits.conf 
	[root@localhost ~]# mv /tmp/system_limits.conf /etc/security/limits.conf
	监测是否修改成功
	[seven@localhost ~]$ ulimit -Hn
	65536



elasticsearch-kopf安装
ps尚未安装