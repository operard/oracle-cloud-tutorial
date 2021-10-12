# How to deploy Spark in Oracle Cloud Kubernetes (OKE) ?

The following walk-through guides you through the steps needed to set up your environment to run Spark in Kubernetes in Oracle Cloud Infrastructure.

## Deploy Kubernetes Cluster

Connect to Oracle Cloud Console to deploy a K8s Cluster.




## Install Kubectl

You must deploy kubectl to connect to OKE Cluster.

You can check the documentation [here](https://kubernetes.io/docs/tasks/tools/)

- The installation of Oracle Linux 7.9 is using a JVM by default. 
- You have access to root either directly or via sudo. By default in OCI, you are connected like "opc" user with sudo privilege.

```console
[opc@xxx ~]$ java -version
java version "1.8.0_281"
Java(TM) SE Runtime Environment (build 1.8.0_281-b09)
Java HotSpot(TM) 64-Bit Server VM (build 25.281-b09, mixed mode)
```

## Java Installation

The install is pretty simple. It consists of setting up Java, installing spark and hadoop components and libraries. 
Lets start with setting up the Spark & Hadoop Environment.

Download the last version of JDK 1.8 because Hadoop 2.X is using this JAVA version.


```console
rpm -ivh /home/opc/jdk-8u271-linux-x64.rpm
```

Check Java Version.

```console
java -version
```

## Spark and Hadoop Setup


The next step is to install Spark and Hadoop Environment. 

First step, choose the version of Spark and Hadoop you want to install. Download the version you want to install


### Download Spark 2.4.5 for Hadoop 2.7

```console
cd /home/opc
wget http://apache.uvigo.es/spark/spark-2.4.5/spark-2.4.5-bin-hadoop2.7.tgz
```

### Download Spark 2.4.7 for Hadoop 2.7
```console
wget http://apache.uvigo.es/spark/spark-2.4.7/spark-2.4.7-bin-hadoop2.7.tgz
```

### Download Spark 3.1.1 for Hadoop 3.2

```console
wget http://apache.uvigo.es/spark/spark-3.1.1/spark-3.1.1-bin-hadoop3.2.tgz
```

### Install the Spark and Hadoop Version

Install the Spark and Hadoop Version choosen in the directory "/opt".

```console
sudo -i
cd /opt
tar -zxvf /home/opc/spark-2.4.5-bin-hadoop2.7.tgz
or 
tar -zxvf /home/opc/spark-2.4.7-bin-hadoop2.7.tgz
or
tar -zxvf /home/opc/spark-3.1.1-bin-hadoop3.2.tgz
```

## Install PYSPARK in PYTHON3 evnironment

```console
/opt/Python-3.7.6/bin/pip3 install 'pyspark=2.4.7'
/opt/Python-3.7.6/bin/pip3 install findspark
```


Next we can create a virtual environment and enable it.


### Modify your environment to use this Spark and Hadoop Version

Add to ".bashrc" for the user "opc" the next lines


```console
# Add by %OP%
export PYTHONHOME=/opt/anaconda3
export PATH=$PYTHONHOME/bin:$PYTHONHOME/condabin:$PATH

# SPARK ENV
#export JAVA_HOME=$(/usr/libexec/java_home)
export SPARK_HOME=/opt/spark-2.4.5-bin-hadoop2.7
export PATH=$SPARK_HOME/bin:$PATH
export PYSPARK_PYTHON=python3

export PYSPARK_DRIVER_PYTHON=jupyter
export PYSPARK_DRIVER_PYTHON_OPTS='notebook'
```


## Test your Spark and Hadoop Environment


If you're running directly on a virtual machine and have a browser installed it should take you directly into the jupyter environment. Connect to your "http://xxx.xxx.xxx.xxx:8001/".
  
And upload the next notebooks:

- [Notebook test findspark]().
- [Notebook test Pyspark]().
