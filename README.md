###### Desc

Installs and configures spark (master, thrift, historyserver and slave)

###### Deps

This role is essentially plugin for [ansible-role-hadoop](https://github.com/rlex/ansible-role-hadoop) and should be run after it

###### Params for install

For using in host/group vars, allows you to select which parts of spark to install

```
spark_master: true
spark_thriftserver: true
spark_historyserver: true
spark_slave: true
```

Also don't forget to set spark_master_host to ip pointing to node with spark_master

###### Ports

* 8080 - master web
* 18080 - historyserver

###### Pre-setup

You will need to manually create required HDFS folders for spark:

```
hadoop@hdp-1:~$ hdfs dfs -mkdir -p /spark/logs
hadoop@hdp-1:~$ hdfs dfs -mkdir -p /spark/apphistory
```

###### Cluster test

For spark on yarn:
./bin/spark-submit --class org.apache.spark.examples.SparkPi --master yarn --deploy-mode client examples/jars/spark-examples_2.11-2.2.0.jar 1000
For standalone:
./bin/spark-submit --class org.apache.spark.examples.SparkPi --master spark://YOUR_SPARK_MASTER:7077 --deploy-mode client examples/jars/spark-examples_2.11-2.2.0.jar 1000
On lowspec mode it's worth to use --executor-memory 512M --driver-memory 512M

##### TBD

* Fix native libs
* Fix logging to journald
* Fix PID files location (maybe push them somewhere to /opt/hadoop/pids?)
* Spark ignores normal exit signals and needs to be killed by systemd. Not sure what i can do about this.