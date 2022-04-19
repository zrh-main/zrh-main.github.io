17.2安装tomcat服务器
17.2.1拖拽apache-tomcat-7.0.47.tar.gz文件到linux目录下

17.2.2解压文件
输入命令：[root@localhost ~]# tar -zvxf apache-tomcat-7.0.47.tar.gz

17.2.3启动tomcat
[root@localhost apache-tomcat-7.0.47]# ./bin/startup.sh
17.2.4查看tomcat启动日志
[root@localhost apache-tomcat-7.0.47]# tail -f logs/catalina.out

18.防火墙
因为linux系统默认的对外开发的端口号是22,  tomcat的端口号是8080,所以不能访问

1.关闭防火墙
[root@localhost ~]# service iptables stop;
下次重启linux系统后,防火墙又会自动开启.
[root@localhost ~]# chkconfig iptables --list
关闭所有的端口
[root@localhost ~]# chkconfig iptables off