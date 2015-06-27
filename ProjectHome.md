Hadoop-Snappy is a project for Hadoop that provide access to the snappy compression.
http://code.google.com/p/snappy/

This project is integrated into Hadoop Common (JUN 2011).

Hadoop-Snappy can be used as an add-on for recent (released) versions of Hadoop that do not provide Snappy Codec support yet.

Hadoop-Snappy is being kept in synch with Hadoop Common.


### Build Hadoop Snappy ###
1. Requirements: gcc c++, autoconf, automake, libtool, Java 6, JAVA\_HOME set, Maven 3

2. Build/install Snappy (http://code.google.com/p/snappy/)

3. Build Hadoop Snappy
```
$ mvn package [-Dsnappy.prefix=SNAPPY_INSTALLATION_DIR]
```

'snappy.prefix' by default is '/usr/local'. If Snappy is installed in other location than user local set 'snappy.prefix' to the right location.

The built tarball is at target/hadoop-snappy-0.0.1-SNAPSHOT.tar.gz. The tarball includes snappy native library


### Install Hadoop Snappy in Hadoop ###

1. Expand hadoop-snappy-0.0.1-SNAPSHOT.tar.gz file

Copy (recursively) the lib directory of the expanded tarball in
the `<HADOOP_HOME>`/lib of all Hadoop nodes
```
$ cp -r hadoop-snappy-0.0.1-SNAPSHOT/lib/* <HADOOP_HOME>/lib
```

IMPORTANT: Hadoo Snappy 0.0.1-SNAPSHOT tarball includes Snappy native library.

2. Add the following key/value pairs into core-site.xml
```
  <property>
    <name>io.compression.codecs</name>
    <value>
      org.apache.hadoop.io.compress.GzipCodec,
      org.apache.hadoop.io.compress.DefaultCodec,
      org.apache.hadoop.io.compress.BZip2Codec,
      org.apache.hadoop.io.compress.SnappyCodec
    </value>
  </property>
```

3. Restart Hadoop.


### MapReduce Job Sample ###
```
...

Configuration conf = new Configuration();

// Compress Map output
conf.set("mapred.compress.map.output","true");
conf.set("mapred.map.output.compression.codec","org.apache.hadoop.io.compress.SnappyCodec");

// Compress MapReduce output
conf.set("mapred.output.compress","true");
conf.set("mapred.output.compression","org.apache.hadoop.io.compress.SnappyCodec");

...
```
