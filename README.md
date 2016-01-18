# batch-processing-comparison
This project includes all the Karamel definition files which are required to do the batch processing comparison between Apache Spark vs Apache Flink in public cloud. This project used TeraSort for benchmarking the systems and TeraGen has been used to generate the data. You can [feed each Karamel definition file](https://www.youtube.com/watch?v=tCIA8_2dR14) to Karamel to execute its task.

###Step 1 : Deploy the cluster in cloud
--------------
Definition file : [cluster-deployment.yml](https://github.com/karamel-lab/batch-processing-comparison/blob/master/cluster-deployment.yml)

Above definition file will deploy the following clusters.

| Cluster    | Version           |
| :-------------: |:-------------:| 
| Spark     | 1.3.1 | 
| Flink      | 0.9.1      |  
| Hadoop | 2.4.0      | 


####Configurations that you might want to change

* EC2 machine type and region:
```
ec2:
  region: us-west-2
  type: m3.xlarge
```

You can override the machine type, region for different node groups.
```
datanodes:
    size: 2
    ec2:
      region: us-west-2
      type: i2.4xlarge
      price: 0.5
```
* Size and bid for spot instances

Using the above configuration, size of a group can be changed with the parameter ```size:``` and ```price: 0.5``` specify using [spot instances](https://aws.amazon.com/ec2/spot/) with a bid of $0.5 per hour. But if you do not need to take the risk of using spot instances, you can remove the line ```price: 0.5``` and Karamel will automatically pick up on-demand instances.

* Memory allocations:

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



###Step 2 : Run TeraGen experiment to generate data
--------------
Definition file: [teragen.yml](https://github.com/karamel-lab/batch-processing-comparison/blob/master/teragen.yml)

####Configuration changes

* IP to deploy the experiment (required *)
You should add the public IP address of your master node in [configuration for](https://github.com/karamel-lab/batch-processing-comparison/blob/master/terasort-spark.yml#L20) ```ips:```
```
ips:
      - 54.203.56.51
```

* Number of records to be generated
Each record generated by TeraGen is of size 100bytes and the number of records to be generated can be configured with the following [configuration] (https://github.com/karamel-lab/batch-processing-comparison/blob/master/teragen.yml#L13)
```
attrs:
  teragen:
    records: '2000000000'
```
``` records: '2000000000'``` generated 200GB of data.



###Step 3 : Run TeraSort experiment
--------------
TeraSort benchmark for Spark and Flink shoule be run seperately. The public IP address of the master node shoud be configured in ```ips:``` section similarly as for TeraGen experiment

####Spark

Definition file: [terasort-spark.yml](https://github.com/karamel-lab/batch-processing-comparison/blob/master/terasort-spark.yml)


####Configuration changes required *

You should add the public IP address of your master node in configuration for ```ips:```
```
ips:
      - 54.203.56.51
```

####Flink

Definition file: [terasort-flink.yml](https://github.com/karamel-lab/batch-processing-comparison/blob/master/terasort-flink.yml)

####Configuration changes required *

You should add the public IP address of your master node in configuration for ```ips:```
```
ips:
      - 54.203.56.51
```


You can use the [collectl-monitoring tool](https://github.com/shelan/collectl-monitoring) to monitor the system level performance of these big data engines while executing the TeraSort experiment.

Collectl-monitor should be started before you are running the experiment and should be stopped once the experiment is over. You can visit [here](https://github.com/shelan/collectl-monitoring#example-scenario) for an example scenario described for a quick look.
