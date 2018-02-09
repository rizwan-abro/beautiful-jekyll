---
layout: post
title: Integrating R with Hadoop in Ubuntu 16.0.4
gh-repo: rizwan-abro
gh-badge: [star, fork, follow]
---



# Integrating R with Hadoop

## Introducing RHadoop

RHadoop is a collection of three R packages for providing large data operations with an R environment. It was developed by Revolution Analytics, which is the leading commercial provider of software based on R. RHadoop is available with three main R packages: rhdfs, rmr, and rhbase. Each of them offers different Hadoop features.

• **rhdfs** is an R interface for providing the HDFS usability from the R console. As Hadoop MapReduce programs write their output on HDFS, it is very easy to access them by calling the rhdfs methods. The R programmer can easily perform read and write operations on distributed data fles. Basically, rhdfs package calls the HDFS API in backend to operate data sources stored on HDFS.

• **rmr** is an R interface for providing Hadoop MapReduce facility inside the R environment. So, the R programmer needs to just divide their application logic into the map and reduce phases and submit it with the rmr methods. After that, rmr calls the Hadoop streaming MapReduce API with several job parameters as input directory, output directory, mapper, reducer, and so on, to perform the R MapReduce job over Hadoop cluster.

• **rhbase** is an R interface for operating the Hadoop HBase data source stored at the distributed network via a Thrift server. The rhbase package is designed with several methods for initialization and read/write and table
manipulation operations

**Installing the R packages:** We need several R packages to be installed that help it to connect R with Hadoop. The list of the packages is as follows:
° rJava
° RJSONIO
° itertools
° digest
° Rcpp
° httr
° functional
° devtools
° plyr
° reshape2

We can install them by calling the execution of the following R command in
the R console:

```R
install.packages( c('rJava','RJSONIO', 'itertools', 'digest','Rcpp ','httr','functional','devtools', 'plyr','reshape2'))

```


• Setting environment variables: We can set this via the R console using the
following code:

#### Setting up HADOOP_CMD

Paste the code in R 

```R
> Sys.setenv("HADOOP_CMD"="/usr/local/hadoop/bin/hadoop")
```

#### Setting up HADOOP_STREAMING
```R
> Sys.setenv("HADOOP_STREAMING"="usr/local/hadoop/share/hadoop/tools/lib/hadoop-streaming-2.7.5.jar")

```




#### Installing RHadoop Packages [rmr2, rhdfs, rhbase]

1. Download RHadoop packages from GitHub repository of Revolution

   Analytics: https://github.com/RevolutionAnalytics/Rhadoop/wiki/Downloads

**° rmr: [rmr2_3.3.1.tar.gz]**

{: .box-note}
**Note:** This package has dependency on below packages, so you must install them before installing the rmr2 package.

`Rcpp, RJSONIO (>= 0.8-2), digest, functional, reshape2, stringr, plyr, caTools (>= 1.16)`



You can install the rmr2 by running the below command in the terminal

```
$ R CMD INSTALL rmr2_3.3.1.tar.gz
```

**° rhdfs: [rhdfs_1.0.8.tar.gz]**

{: .box-note}
**Note:** Prerequisites 

- This package has a dependency on rJava.


- Access to HDFS via this R package is dependent upon the `HADOOP_CMD` environment variable. `HADOOP_CMD` points to the full path for the `hadoop` binary. If this variable is not properly set, the package will fail when the `init()` function is invoked.
- Thrift (rhdfs & rhbase both has a dependency on thrift server)

`if you haven't installed rJava correctly; the rhdfs won't work`



##### Step#1 Installing rJava manually; skip this step if you have installed already

Download rJava from this [link](https://cran.r-project.org/src/contrib/rJava_0.9-9.tar.gz)

Navigate to the folder where rJava is downloaded

Open the terminal and run the command below

```
$ R CMD INSTALL rJava_0.9-9.tar.gz
```

**Step#2 HADOOP_CMD & HADOOP_STREAMING path must be set before installing rhdfs**

```R
Sys.setenv("HADOOP_CMD"="/usr/local/hadoop/bin/hadoop")
Sys.setenv("HADOOP_STREAMING"="/usr/local/hadoop/contrib/streaming/
hadoop-streaming-1.0.3.jar")
```

**Step#3 Installing Thrift**

Paste the command below in terminal

```
$ sudo apt-get install libboost-dev libboost-test-dev libboost-program-options-dev libevent-dev automake libtool flex bison pkg-config g++ libssl-dev  
```

Download the thrift 0.9.0 

```
$ wget http://archive.apache.org/dist/thrift/0.9.0/thrift-0.9.0.tar.gz
```

Navigate to the downloaded directory and extract thrift

```
$ tar -xvzf thrift-0.9.0.tar.gz
```

Change into the Thrift installation directory(the one extracted) and carry on the following command

```
$ cd thrift-0.9.0
```

Now run this command

```
$ ./configure  
```

Now this command

```
$  make  
```

And finally this one

```
 $ sudo make install  
```

To verify Thrift has installed properly, use the following command

```
  $  thrift -version   
```

Now install rhdfs package by running the below command in the terminal

```
$ R CMD INSTALL rhdfs_1.0.8.tar.gz
```

**° rhbase: [rhbase_1.2.1.tar.gz]** 

Run this command in the terminal

```
$ R CMD INSTALL rhbase-1.2.1.tar.gz
```





Once we complete the installation of RHadoop, we can test the setup by running the
MapReduce job with the rmr2 and rhdfs libraries in the RHadoop sample program
as follows:

##### loading the libraries in R 

{: .box-note}
**Note:** Define the HADOOP_CMD & HADOOP_STREAMING path variable before loading the packages in R

Run the commands below in the R:

`Sys.setenv("HADOOP_CMD"="usr/local/hadoop/bin/hadoop")`

`Sys.setenv("HADOOP_CMD"="usr/local/hadoop/share/hadoop/tools/lib/hadoop-streaming-2.7.5.jar")`

library(rhdfs')

library('rmr2')

##### initializing the RHadoop
hdfs.init() 