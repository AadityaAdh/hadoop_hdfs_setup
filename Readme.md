# HADOOP HDFS SETUP

## step 1-install sudo 

    apt update && apt install sudo


## user

    adduser hd
    usermod -aG sudo hd
    su - hd


## install required by

    sudo apt update && apt install -y iputils-ping net-tools vim openssh-server openssh-client default-jdk


## setting up ssh

in every machine

    sudo service ssh start

then in master

    ssh-keygen -b 4096

then for every worker node ,execute the following command from master

    ssh-copy-id hd@worker_ip




## download hadoop by

    wget https://downloads.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz

## untar by

    tar -xzf hadoop-3.3.6.tar.gz


## configurations file

Core-site.xml
    <configuration>
        <property>
            <name>fs.defaultFS</name>
            <value>hdfs://master_machine_ip:9000</value>
        </property>
    </configuration>


Hdfs-site.xml
    <configuration>

        <property>
            <name>dfs.namenode.name.dir</name>
            <value>file:///usr/local/hadoop/hdfs/namenode</value>
        </property>

        <property>
            <name>dfs.datanode.data.dir</name>
            <value>file:///usr/local/hadoop/hdfs/datanode</value>
        </property>

        <property>
            <name>dfs.namenode.rpc-address</name>
            <value>master_machine_ip:9000</value>
        </property>

    </configuration>


Mapred-site.xml

    <configuration>

        <property>
            <name>mapreduce.framework.name</name>
            <value>yarn</value>
        </property>

    </configuration>


Yarn-site.xml

    <configuration>

        <!-- ResourceManager main RPC address -->
        <property>
            <name>yarn.resourcemanager.address</name>
            <value>localhost:8025</value>
        </property>

        <!-- ResourceManager scheduler address -->
        <property>
            <name>yarn.resourcemanager.scheduler.address</name>
            <value>localhost:8030</value>
        </property>

        <!-- ResourceManager resource tracker address -->
        <property>
            <name>yarn.resourcemanager.resource-tracker.address</name>
            <value>localhost:8050</value>
        </property>

        <!-- NodeManager auxiliary services for MapReduce shuffle -->
        <property>
            <name>yarn.nodemanager.aux-services</name>
            <value>mapreduce_shuffle</value>
        </property>

        <!-- Minimum healthy disks required on NodeManager -->
        <property>
            <name>yarn.nodemanager.disk-health-checker.min-healthy-disks</name>
            <value>1</value>
        </property>

    </configuration>

## in hadoop-env.sh
    export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-arm64

## in workers file
    write the ip of every worker machine you have




## copying from master to every worker using scp

    scp core-site.xml mapred-site.xml hdfs-site.xml yarn-site.xml hadoop-env.sh 172.19.0.3:/home/hd/hadoop-3.3.6/etc/hadoop/.




## creating folder in master
    sudo mkdir -p /usr/local/hadoop/hdfs/namenode
    sudo chown -R hd:hd /usr/local/hadoop


## creating foler in workers

    sudo mkdir -p /usr/local/hadoop/hdfs/datanode
    sudo chown -R hd:hd /usr/local/hadoop


## in your home/your_username/ folder you will find .bashrc in this add these lines
    export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-arm64
    export HADOOP_HOME=/home/hd/hadoop-3.3.6
    export PATH=$PATH:$JAVA_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin


# enjoy


















