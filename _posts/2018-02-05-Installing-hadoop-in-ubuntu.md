---
layout: post
title: Installing Hadoop on Ubuntu 16.0.4
gh-repo: rizwan-abro/Rizwan Abro
gh-badge: [star, fork, follow]
---



## Step 1 — Installing Java

```
$ sudo apt-get update && sudo apt-getupgrade
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository ppa:webupd8team/java
$ sudo apt-get update
$ sudo apt-get install oracle-java8-installer
```

Once the installation is complete, let's check the version.

```
$ java -version
```



*Output *

`openjdk version "1.8.0_91"OpenJDK Runtime Environment (build 1.8.0_91-8u91-b14-3ubuntu1~16.04.1-b14)OpenJDK 64-Bit Server VM (build 25.91-b14, mixed mode)`





## Disable IPV6

Open the file `/etc/sysctl.conf`:

```
rizwan % editor /etc/sysctl.conf
```

And append to the end:

```
# Disable IPv6
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
```

Now reboot the system

```
rizwan % reboot
```

Now add a group 

```
rizwan % addgroup hadoopgroup
rizwan % adduser -ingroup hadoopgroup hadoopuser
```

Install SSH

```
rizwan % sudo apt install ssh
rizwan % sudo apt install ssh
rizwan % sudo systemctl enable ssh
rizwan % sudo systemctl start ssh
```



Switch to hadoopuser

```
rizwan % su - hadoopuser
```

Add the public key 

```
hadoopuser@rizwan ~ $ ssh-keygen -t rsa -P ""
hadoopuser@rizwan ~ $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
hadoopuser@rizwan ~ $ chmod 600 ~/.ssh/authorized_keys
hadoopuser@rizwan ~ $ ssh-copy-id -i ~/.ssh/id_rsa.pub localhost
hadoopuser@rizwan ~ $ ssh localhost
```

 **Install Hadoop**

```
rizwan % wget it.apache.contactlab.it/hadoop/common/hadoop-2.7.5/hadoop-2.7.5.tar.gz
rizwan % tar xzf hadoop-2.7.5.tar.gz
rizwan % rm -rf hadoop-2.7.5.tar.gz
```

we'll move the extracted files into `/usr/local`, the appropriate place for locally installed software. Change the version number, if needed, to match the version you downloaded.

```
rizwan % sudo mv hadoop-2.7.5 /usr/local
rizwan % sudo ln -sf /usr/local/hadoop-2.7.5/ /usr/local/hadoop
rizwan % sudo chown -R hadoopuser:hadoopgroup /usr/local/hadoop-2.7.5/
```

Edit bashrc file for setting the environment variables of hadoop:

```
hadoopuser@rizwan ~ $ editor ~/.bashrc
```



**Append at the end:**

`#Hadoop Config:`

`export HADOOP_PREFIX=/usr/local/hadoop`

`export HADOOP_HOME=/usr/local/hadoop`

`export HADOOP_MAPRED_HOME=${HADOOP_HOME}`

`export HADOOP_COMMON_HOME=${HADOOP_HOME}`

`export HADOOP_HDFS_HOME=${HADOOP_HOME}`

`export YARN_HOME=${HADOOP_HOME}`

`export HADOOP_CONF_DIR=${HADOOP_HOME}/etc/hadoop`

`# Native path`

`export HADOOP_COMMON_LIB_NATIVE_DIR=${HADOOP_PREFIX}/lib/native`

`export HADOOP_OPTS="-Djava.library.path=$HADOOP_PREFIX/lib/native"`

`# Java path`

`export JAVA_HOME="/usr"`

`# OS path`

`export PATH=$PATH:​$HADOOP_HOME/bin:$JAVA_PATH/bin:​$HADOOP_HOME/sbin`



**Save Changes**

Next, source ~/.bashrc to apply changes.

hadoopuser@rizwan ~ $ source ~/.bashrc



Now we need to edit **/usr/local/hadoop/etc/hadoop/hadoop-env.sh**

Hadoop requires that you set the path to Java, either as an environment variable or in the Hadoop configuration file.

`rizwan % editor /usr/local/hadoop/etc/hadoop/hadoop-env.sh`

And add this at the end:

`export JAVA_HOME="/usr"`



### Configure Hadoop

Hadoop configuration is quite hard, because it has a lot of config files. We need to navigate to /usr/local/hadoop/etc/hadoop and edit these files:

·                   **core-site.xml**

·                   **hdfs-site.xml**

·                   **mapred-site.xml** (needs to be copied from mapred-site.xml.template)

·                   **yarn-site.xml**

They all are XML files with a top-level <configuration> node. For clarity we report the configuration node only.

**core-site.xml**

**<configuration>**

**<property>**

  **<name>**fs.default.name**</name>**

​    **<value>**hdfs://localhost:9000**</value>**

**</property>**

**</configuration>**

**hdfs-site.xml**

**<configuration>**

**<property>**

 **<name>**dfs.replication**</name>**

 **<value>**1**</value>**

**</property>**

 

**<property>**

  **<name>**dfs.name.dir**</name>**

​    **<value>**file:/usr/local/hadoop/hadoopdata/hdfs/namenode**</value>**

**</property>**

 

**<property>**

  **<name>**dfs.data.dir**</name>**

​    **<value>**file:/usr/local/hadoop/hadoopdata/hdfs/datanode**</value>**

**</property>**

**</configuration>**

{: .box-note}
**Note:** Mapred-site.xml doesn't exists so you need to create this file with xml header.

**mapred-site.xml**

**<configuration>**

 **<property>**

  **<name>**mapreduce.framework.name**</name>**

   **<value>**yarn**</value>**

 **</property>**

**</configuration>**

**yarn-site.xml**

**<configuration>**

 **<property>**

  **<name>**yarn.nodemanager.aux-services**</name>**

​    **<value>**mapreduce_shuffle**</value>**

 **</property>**

**</configuration>**



**Format namenode**

Next, we need to format the namenode filesystem with thefollowing command:

hadoopuser@rizwan ~ $ hdfs namenode -format

`Search the output: if you can read a string like this:`

INFO common.Storage: Storage directory /usr/local/hadoop/hadoopdata/hdfs/namenodehas been successfully formatted.

It's done.

Start and stop services

Now, the last thing to do is starting Hadoop services:

hadoopuser@rizwan ~ $ start-dfs.sh

hadoopuser@rizwan ~ $ start-yarn.sh

To check the status of the services use the jps command:

hadoopuser@rizwan ~ $ jps

`You will see the following output`

**26899** Jps

**26216** SecondaryNameNode

**25912** NameNode

**26041** DataNode

**26378** ResourceManager

**26494** NodeManager



To stop services, these are the commands:

hadoopuser@rizwan ~ $ stop-dfs.sh

hadoopuser@rizwan ~ $ stop-yarn.sh



## **Congratulations, you made it!**