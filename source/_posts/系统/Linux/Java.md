
17.1安装java环境JDK

17.1.1上传JDK压缩包


17.1.2创建java目录
[root@localhost /]# mkdir /usr/java

17.1.3解压文件到新建的java目录
[root@localhost /]# tar -xzvf jdk-7u55-linux-i586.tar.gz -C /usr/java/


17.1.4配置环境变量
[root@localhost /]# vim /etc/profile

编辑文件里的使用命令：  
1）输入环境变量 JAVA_HOME=/usr/java/jdk1.7.0_55/


3）添加命令 export  PATH=$JAVA_HOME/bin:$PATH

4）保存文件   :wq!

5）重新加载 文件  source  /etc/profile

6）验证是否安装成功   java -version
