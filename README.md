# Estudo da Ferramenta Hadoop :elephant:

> Hadoop é uma plataforma de computação distribuída voltada para clusters e processamento de grandes volumes de dados, com atenção a tolerância a falhas. Foi inspirada no MapReduce e no GoogleFS.

![Logo Hadoop](/images/logo-hadoop.png)

### Instalação

Faça o download da versão 2.9.2 do Hadoop
[Link para download](https://archive.apache.org/dist/hadoop/common/hadoop-2.9.2/hadoop-2.9.2.tar.gz)

```console
hadoop@mrcs-Lenovo:~$ mkdir softwares
hadoop@mrcs-Lenovo:~$ mv ~/Downloads/hadoop-2.9.2.tar.gz ~/softwares
hadoop@mrcs-Lenovo:~$ cd softwares
hadoop@mrcs-Lenovo:~/softwares$ tar -xzf hadoop-2.9.2.tar.gz
hadoop@mrcs-Lenovo:~/softwares$ cd ..
hadoop@mrcs-Lenovo:~$ ln -s softwares/hadoop-2.9.2 hadoop292
hadoop@mrcs-Lenovo:~$ ssh-keygen -t rsa
hadoop@mrcs-Lenovo:~$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
hadoop@mrcs-Lenovo:~$ ssh hadoop@localhost
hadoop@mrcs-Lenovo:~$ logout
hadoop@mrcs-Lenovo:~$ whereis java
hadoop@mrcs-Lenovo:~$ ls -l /usr/bin/java
hadoop@mrcs-Lenovo:~$ ls -alh /etc/alternatives/java
hadoop@mrcs-Lenovo:~$ gedit ~/.bashrc
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
```shell
hadoop@mrcs-Lenovo:~$ source ~/.bashrc
hadoop@mrcs-Lenovo:~$ echo $HADOOP_HOME
hadoop@mrcs-Lenovo:~$ mkdir /home/hadoop/hadoop292/data
hadoop@mrcs-Lenovo:~$ mkdir -p /home/hadoop/hadoop292/hdfs_store/hdfs/datanode
hadoop@mrcs-Lenovo:~$ mkdir -p /home/hadoop/hadoop292/hdfs_store/hdfs/namenode
hadoop@mrcs-Lenovo:~$ cd hadoop292/etc/hadoop
hadoop@mrcs-Lenovo:~/hadoop292/etc/hadoop$ gedit core-site.xml
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
```bash
hadoop@mrcs-Lenovo:~/hadoop292/etc/hadoop$ gedit hdfs-site.xml
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
```sh
hadoop@mrcs-Lenovo:~/hadoop292/etc/hadoop$ gedit yarn-site.xml
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
```zsh
hadoop@mrcs-Lenovo:~/hadoop292/etc/hadoop$ cp mapred-site.xml.template mapred-site.xml
hadoop@mrcs-Lenovo:~/hadoop292/etc/hadoop$ gedit mapred-site.xml
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
```PowerShell
hadoop@mrcs-Lenovo:~/hadoop292/etc/hadoop$ gedit hadoop-env.sh
```
Altere o arquivo a seguinte linha
```XML
export JAVA_HOME=/usr/lib/jvm/java-8-oracle/            #java-8-openjdk-amd64
```
Salve o arquivo e retorne ao terminal
```ps
hadoop@mrcs-Lenovo:~/hadoop292/etc/hadoop$ hdfs namenode -format
hadoop@mrcs-Lenovo:~/hadoop292/etc/hadoop$ cd
hadoop@mrcs-Lenovo:~$ start-dfs.sh &
hadoop@mrcs-Lenovo:~$ jps
hadoop@mrcs-Lenovo:~$ start-yarn.sh &
hadoop@mrcs-Lenovo:~$ jps
hadoop@mrcs-Lenovo:~$ hdfs dfs -mkdir -p /user/hadoop
hadoop@mrcs-Lenovo:~$ hdfs dfs -mkdir -p /user/hadoop/input
hadoop@mrcs-Lenovo:~$ hdfs dfs -mkdir -p /user/hadoop/output
hadoop@mrcs-Lenovo:~$ hdfs dfs -ls /user/hadoop
hadoop@mrcs-Lenovo:~$ cd /hadoop292/input
hadoop@mrcs-Lenovo:~/hadoop292/input$ hdfs dfs -put lorem-input.txt /user/hadoop/input/
hadoop@mrcs-Lenovo:~/hadoop292/etc/hadoop$ hdfs dfs -ls /user/hadoop/input
```
-> Abra o navegador

Acesse o link [localhost:50070](http://localhost:50070)

Acesse o link [localhost:8088](http://localhost:8088)

Volte para o terminal 
```bat
hadoop@mrcs-Lenovo:~/hadoop292/etc/hadoop$ cd
hadoop@mrcs-Lenovo:~$ hadoop jar hadoop292/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.2.jar
hadoop@mrcs-Lenovo:~$ hadoop jar hadoop292/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.2.jar wordcount /user/hadoop/input/ /user/hadoop/output/wc
```