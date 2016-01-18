# batch-processing-comparison
This project includes all the Karamel definition files which are required to do the batch processing comparison between Apache Spark vs Apache Flink in public cloud. This project used TeraSort for benchmarking the systems and TeraGen has been used to generate the data. You can [feed each Karamel definition file](https://www.youtube.com/watch?v=tCIA8_2dR14) to Karamel to execute its task.

##Step 1 : Deploy the cluster in cloud
--------------
Definition file : cluster-deployment.yml

##Configurations that you might want to change

EC2 machine type and region:
```
ec2:
  region: us-west-2
  type: m3.xlarge
```

Memory allocations:

Spark
```
driver_memory: 8192m
executor_memory: 100g
```

Flink
```
jobmanager:
      heap_mbs: '8192'
    taskmanager:
      heap_mbs: '102400'
```

For a fair comparison, you can allocate the same amount of memory for Spark driver_memory and Flink jobmanager. And similarly for Spark executor_memory and Flink taskmanager.

###Step 2
--------------
Run TeraGen experiment to generate data

###Step 3
--------------
Run TeraSort experiment to execute the benchmarking algorithm

You can use the [collectl-monitoring tool](https://github.com/shelan/collectl-monitoring) to monitor the system level performance of these big data engines while executing the TeraSort experiment.