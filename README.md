# spark1.6.0-bin-without-hadoop 部署
1. 在master和每台slave上，解压spark-1.6.0-bin-without-hadoop.tgz到/homed目录下面，并重命名为sparkcd /homed
    tar xf /root/tmp/spark-1.6.0-bin-without-hadoop.tgz
    mv spark-1.6.0-bin-without-hadoop spark
2. 在master服务器上，进入/homed/spark/conf目录，`cp spark-env.sh.template spark-env.sh`，然后编辑spark-env.sh，添加
  ```shell
  export JAVA_HOME=/usr/java/jdk1.7.0_55
  export SPARK_DIST_CLASSPATH=$(/hadoop/hadoop-2.6.4/bin/hadoop classpath)
  export SPARK_MASTER_WEBUI_PORT=9090
  export HADOOP_CONF_DIR=/work/hadoop/hadoop-2.6.4/etc/hadoop/
  export SPARK_HOME=/homed/spark
  export SPARK_WORKER_OPTS="-Dspark.worker.cleanup.enabled=true -Dspark.worker.cleanup.interval=1800"
  ```
3. 同步spark-env.sh到各个slave
  ```shell
  cd /homed
  ./updatefiletoallslave.sh /homed/spark/conf/spark-env.sh
  ./docmdonall.sh "cd /homed/spark/ && mkdir -p /exe/log && mkdir eventLog"
  ```
4. 在/homed/spark/conf目录下面，创建slaves文件，把各个slave的域名或者ip写入进去，一行一个，如：  
		slave1  
		slave2  
		...  
		slaveN  
5. 进入 /homed/spark/sbin，启动脚本start-all.sh
6. 如果无异常，那么在浏览器中访问 master:9090，会有一个监控页面，证明spark启动成功



