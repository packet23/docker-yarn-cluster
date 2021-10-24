# docker-yarn-cluster

A Hadoop Yarn cluster running in a docker-compose deployment.

## How to build

```bash
docker-compose -f build.yaml build
```

## How to run

```bash
docker-compose up
```

## WebUIs

When the cluster is up and running, the following Web UIs can be accessed:

* [Resource Manager](http://localhost:8042/node)
* [Name Node](http://localhost:9870/dfshealth.html#tab-overview)
* [Job History Server](http://localhost:19888/jobhistory)
* [Data Node](http://localhost:9864/datanode.html)
* [Node Manager](http://localhost:8042/node)

## Using the cluster

A head node is running and can be accessed using SSH (username: user, password: hadoop):

```bash
ssh ssh://user@localhost:2222/
```

## Smoke test

Start the cluster and log on to head node (see above). You can run Teragen/Terasort/Teravalidate to see if the cluster is able to
run Hadoop MapReduce jobs and read/write to HDFS. To generate a 9.3 GiB dataset you would use:

```bash
hadoop jar /opt/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-*.jar teragen 100000000 /user/user/teragen
```

Teragen generates test data to be sorted by Terasort. To sort the generated dataset, you would use

```bash
hadoop jar /opt/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-*.jar terasort /user/user/teragen /user/user/terasort
```

Terasort sorts the generated dataset and outputs the same dataset globally sorted. Teravalidate verifies that the dataset is
globally sorted. You can run it like this:

```bash
hadoop jar /opt/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-*.jar teravalidate /user/user/terasort /user/user/teravalidate
```

:warning: *WARNING* The data is stored on HDFS and could be spilled to disk. When running on macOS, you should ensure that Docker's /Disk image size/
is set to a capacity that can hold the dataset at least 3 times.
