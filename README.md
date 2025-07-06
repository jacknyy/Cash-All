ImmortalWrt安装Docker后无法访问容器的解决办法

1. 打开/etc/config/firewall 后，添加如下规则：

config zone 'docker'
	option name 'docker'
	option input 'ACCEPT'
	option output 'ACCEPT'
	option forward 'ACCEPT'
	list subnet '172.16.0.0/12'

config nat 'docker_nat'
	option name 'DockerNAT'
	option family 'ipv4'
	option proto 'all'
	option src 'lan'
	option target 'MASQUERADE'
	option extra '-i docker0'
	option src_ip '172.16.0.0/12'

 
2.打开/etc/config/dockerd 后，修改如下：

config globals 'globals'
	option data_root '你的Docker 根目录'
	option log_level 'warn'
	option iptables '0'

config proxies 'proxies'

config firewall 'firewall'
	option device 'docker0'


3.打开/tmp/dockerd/daemon.json 后，找到 iptables 并修改为"iptables": false


