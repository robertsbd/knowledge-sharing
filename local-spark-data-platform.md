# Set up a local spark data platform

Creating a spark data platform on a local machine can be useful for learning about spark and delta lakes when you may not have access to a cloud platform with technologies such as Databricks or Synapse/Fabric.

These are steps that you can use to set up your own Spark Data Platform.

This page documents how to set-up a Spark data platform on a system running Arch Linux. For other Linux distros adjust to use the appropriate package manager. Likewise I have done this previously on MacOS, the steps are the same but I used the brew package manager rather than pacman.

## Step 1 Update all your packages and install any dependencies.

``` shell
sudo pacman -Syu tar wget
```

## Step 2 Install Java

Spark runs on Java and you will need the JDK, the open JDK works well, I am using version 17.

``` shell
sudo pacman -S jre17-openjdk
```

Once installed validate:

``` shell
java --version
```

Which should output something like this:

``` shell
openjdk 17.0.11 2024-04-16 
OpenJDK Runtime Environment (build 17.0.11+9) 
OpenJDK 64-Bit Server VM (build 17.0.11+9, mixed mode, sharing)
```

## Step 3 Install Scala

If scala is not installed on your system then download and install it. You will find instructions at https://www.scala-lang.org/download/

To do so I copied the command provided at the scala website and ran it as below:

``` shell
curl -fL https://github.com/coursier/coursier/releases/latest/download/cs-x86_64-pc-linux.gz | gzip -d > cs && chmod +x cs && ./cs setup
```

Make sure that scala is included in your systems path. On my system Scala has been installed at: 

``` shell
~/.local/share/coursier/bin
```

During the scala install, there were messages indicating that the following were added to my path:

``` shell
Should we add ~/.local/share/coursier/bin to your PATH via ~/.profile, ~/.bash_profile
```

To ensure these locations were now in my active path I ran:

``` shell
source ~/.profile
```

I then validated this with: 

``` shell
echo $PATH
```

I also checked that scala had been installed

``` shell
scala -version
```

To which I received the following output

``` shell
Scala code runner version 3.4.2 -- Copyright 2002-2024, LAMP/EPFL
```

I also received an error saying that JAVA_HOME was not set. TO resolve this simply run scala by typing 

``` shell
scala 
```

and it will downloaded the required version of Java if there was a problem earlier

## Step 3 Install Apache Spark

Go here and select the version you want, pick the latest.

https://spark.apache.org/downloads.html

In this article I am downloading spark-3.5.4-bin-hadoop3.tgz and then extracting the tar archive as follows

``` shell
wget https://dlcdn.apache.org/spark/spark-3.5.4/spark-3.5.4-bin-hadoop3.tgz
tar xvf spark-3.5.4-bin-hadoop3.tgz
```

Move the extracted folder to the right location on your system. I am moving mine to:

``` shell
sudo mv spark-3.5.4-bin-hadoop3 /usr/local/spark
```

If you installed this in the same location as above add /usr/local/spark/bin to your path.

## Step 4 Verify your spark installation

Run your spark shell with the below command "spark-shell" and you should see the below (note the below was from an install with an older version of Spark then mentioned above):

``` shell
spark-shell

Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 3.5.1
      /_/
         
Using Scala version 2.12.18 (OpenJDK 64-Bit Server VM, Java 17.0.11)
Type in expressions to have them evaluated.
Type :help for more information.

scala> 
```

## Step 5 Verify Pyspark shell

To use the pyspark shell enter the command "pyspark" below and you should see

``` shell
pyspark

Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /__ / .__/\_,_/_/ /_/\_\   version 3.5.1
      /_/

Using Python version 3.12.3 (main, Apr 23 2024 09:16:07)
Spark context Web UI available at http://192.168.1.32:4040
Spark context available as 'sc' (master = local[*], app id = local-1717346117959).
SparkSession available as 'spark'.
>>> 
```

## Step 6 Using Delta with PySpark

To run a delta lake and to work with delta files you will find instructions here

https://delta.io/learn/getting-started/

Ensure you have the correct python libraries installed if you are using Python, we will do so below creating them in a virtual environment.

### Create virtual environment

``` shell
python -m venv venv
```

### Install delta-spark library

``` shell
pip install delta-spark
```

You are now ready to create some Python scripts, and create a delta-lake on your own system.

Here you can create a medallion architecture storing your delta files in different folders bronze, silver, gold if you like.

Other things to do is to start to play around with the Hive metastore.

As you play with these you will come to the realisation that Databricks, Synapse and Fabric are all very impressive, but are essentially a UI wrapper on Apache Spark and the Delta file format which is where the real magic lies.
