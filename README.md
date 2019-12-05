# Estudo da Ferramenta Hadoop :elephant:

> Hadoop é uma plataforma de computação distribuída voltada para clusters e processamento de grandes volumes de dados, com atenção a tolerância a falhas. Foi inspirada no MapReduce e no GoogleFS.

> Marcos Antônio de Souza @mrcs13  
> Thaynara Mábille Marques Ribeiro

![Logo Hadoop](images/logo-hadoop.png)

### Instalação

Faça o download da versão 2.9.2 do Hadoop\
[Link para download](https://archive.apache.org/dist/hadoop/common/hadoop-2.9.2/hadoop-2.9.2.tar.gz)

```console
hadoop@ubuntu:~$ mkdir softwares
hadoop@ubuntu:~$ mv ~/Downloads/hadoop-2.9.2.tar.gz ~/softwares
hadoop@ubuntu:~$ cd softwares
hadoop@ubuntu:~/softwares$ tar -xzf hadoop-2.9.2.tar.gz
hadoop@ubuntu:~/softwares$ cd ..
hadoop@ubuntu:~$ ln -s softwares/hadoop-2.9.2 hadoop292
hadoop@ubuntu:~$ ssh-keygen -t rsa
hadoop@ubuntu:~$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
hadoop@ubuntu:~$ ssh hadoop@localhost
hadoop@ubuntu:~$ logout
hadoop@ubuntu:~$ whereis java
hadoop@ubuntu:~$ ls -l /usr/bin/java
hadoop@ubuntu:~$ ls -alh /etc/alternatives/java
hadoop@ubuntu:~$ gedit ~/.bashrc
```
Adicione ao final do arquivo

```bash
##JAVA_HOME
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$PATH:export PATH=$PATH:/usr/lib/jvm/java-8-openjdk-amd64/bin

##HADOOP_VARIABLES
export HADOOP_HOME=/home/hadoop/hadoop292

export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin

export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMOM_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME

export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export HADOOP_COMMOM_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
export HADOOP_STREAMING=$HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-2.9.2.jar
export HADOOP_LOG_DIR=$HADOOP_HOME/logs
```
Salve o arquivo e retorne ao terminal
```console
hadoop@ubuntu:~$ source ~/.bashrc
hadoop@ubuntu:~$ echo $HADOOP_HOME
hadoop@ubuntu:~$ mkdir /home/hadoop/hadoop292/data
hadoop@ubuntu:~$ mkdir -p /home/hadoop/hadoop292/hdfs_store/hdfs/datanode
hadoop@ubuntu:~$ mkdir -p /home/hadoop/hadoop292/hdfs_store/hdfs/namenode
hadoop@ubuntu:~$ cd hadoop292/etc/hadoop
hadoop@ubuntu:~/hadoop292/etc/hadoop$ gedit core-site.xml
```
Altere o arquivo as seguintes linhas
```XML
<configuration>
    <property>
        <name>fs.default.name</name>
        <value>hdfs://127.0.0.1:9000</value>
    </property>

    <property>
        <name>hadoop.tmp.dir</name>
        <value>/home/hadoop/hadoop292/data</value>
    </property>
</configuration>
```
Salve o arquivo e retorne ao terminal
```console
hadoop@ubuntu:~/hadoop292/etc/hadoop$ gedit hdfs-site.xml
```
Altere o arquivo as seguintes linhas
```XML
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>

    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:/home/hadoop/hadoop292/hdfs_store/hdfs/namenode</value>
    </property>

    <property>
        <name>dfs.datanode.name.dir</name>
        <value>file:/home/hadoop/hadoop292/hdfs_store/hdfs/data node</value>
    </property>
</configuration>
```
Salve o arquivo e retorne ao terminal
```console
hadoop@ubuntu:~/hadoop292/etc/hadoop$ gedit yarn-site.xml
```
Altere o arquivo as seguintes linhas
```XML
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>

    <property>
        <name>yarn.resourcemanager.address</name>
        <value>127.0.0.1:8032</value>
    </property>

    <property>
        <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
        <value>org.apache.hadoop.mapred.ShuffleHandler</value>
    </property>

    <property>
        <name>yarn.resourcemanager.resource-tracker.address</name>
        <value>127.0.0.1:8025</value>
    </property>

    <property>
        <name>yarn.resourcemanager.scheduler.address</name>
        <value>127.0.0.1:8030</value>
    </property>

    <property>
        <name>yarn.resourcemanager.address</name>
        <value>127.0.0.1:8050</value>
    </property>
</configuration>
```
Salve o arquivo e retorne ao terminal
```console
hadoop@ubuntu:~/hadoop292/etc/hadoop$ cp mapred-site.xml.template mapred-site.xml
hadoop@ubuntu:~/hadoop292/etc/hadoop$ gedit mapred-site.xml
```
Altere o arquivo as seguintes linhas
```XML
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>

    <property>
        <name>mapreduce.jobhistory.address</name>
        <value>127.0.0.1:10020</value>
        <description>Default port is 10020</description>
    </property>

    <property>
        <name>mapreduce.jobhistory.webapp.address</name>
        <value>127.0.0.1:19888</value>
        <description>Default port is 19888</description>
    </property>
</configuration>
```
Salve o arquivo e retorne ao terminal
```console
hadoop@ubuntu:~/hadoop292/etc/hadoop$ gedit hadoop-env.sh
```
Altere o arquivo a seguinte linha
```XML
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/
```
Salve o arquivo e retorne ao terminal
```console
hadoop@ubuntu:~/hadoop292/etc/hadoop$ hdfs namenode -format
hadoop@ubuntu:~/hadoop292/etc/hadoop$ cd
```
### Execução
```console
hadoop@ubuntu:~$ start-dfs.sh &
hadoop@ubuntu:~$ start-yarn.sh &
hadoop@ubuntu:~$ jps
hadoop@ubuntu:~$ hdfs dfs -mkdir -p /user/hadoop
hadoop@ubuntu:~$ hdfs dfs -mkdir -p /user/hadoop/input
hadoop@ubuntu:~$ hdfs dfs -mkdir -p /user/hadoop/output
hadoop@ubuntu:~$ hdfs dfs -ls /user/hadoop
hadoop@ubuntu:~$ cd /hadoop292/input
hadoop@ubuntu:~/hadoop292/input$ hdfs dfs -put lorem-input.txt /user/hadoop/input/
hadoop@ubuntu:~/hadoop292/etc/hadoop$ hdfs dfs -ls /user/hadoop/input
```
-> Abra o navegador

NameNode [localhost:50070](http://localhost:50070)\
NodeManager [localhost:8088](http://localhost:8088)

Volte para o terminal 
```console
hadoop@ubuntu:~/hadoop292/etc/hadoop$ cd
hadoop@ubuntu:~$ hadoop jar hadoop292/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.2.jar
hadoop@ubuntu:~$ hadoop jar hadoop292/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.2.jar wordcount /user/hadoop/input/ /user/hadoop/output/wc
hadoop@ubuntu:~$ hdfs dfs -ls /user/hadoop/output
hadoop@ubuntu:~$ hdfs dfs -cat /user/hadoop/output/wc/part*
hadoop@ubuntu:~$ hdfs dfs -rm -r /user/hadoop/user/hadoop/output/wc
hadoop@ubuntu:~$ stop-dfs.sh &
hadoop@ubuntu:~$ stop-yarn.sh &
```