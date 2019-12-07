# Netdata-Central-with-Promethues-Grafana
Run Netdata Central and integrated Promethues &amp; Grafana ( Panel Boom Table)

Netdata is a centralized tool and has a huge impact on reducing resource in operating nodes. see

https://docs.netdata.cloud/streaming/#streaming-configuration 

The target Netdata tool has been in mode Master/Slave, because In this mode the slave is just a plain data collector. It spawns all external plugins, but instead of maintaining a local database and accepting dashboard requests, it streams all metrics to the master. The memory footprint is reduced significantly, to between 6 MiB and 40 MiB, depending on the enabled plugins.

**STEP1:**


1) installation Netdata on Ubuntu 16.04 (Master/Slave)

2) set config on master/slave.

see https://docs.netdata.cloud/streaming/#monitoring-ephemeralnodes

3) Steps for reducing the resource usage even more:

https://docs.netdata.cloud/docs/performance/#running-netdata-in-embedded-devices

For all the steps above, we need an Ansible role to config them.

**STEP2:**

Integrated Netdata central with Prometheus and Grafana (Plugin Boom Table) 

1) vim /etc/Prometheus/Prometheus.yml

**Add line below scrape_configs**


    job_name: 'netdata-scrape'

      metrics_path: '/api/v1/allmetrics'

      params:

        format: [prometheus_all_hosts]

      honor_labels: true

      static_configs:

        targets: ['{your.netdata.ip}:19999'] 



2) add data source Prometheus in Grafana:
![pic1](https://github.com/ali-nikou/Netdata-Central-with-Promethues-Grafana/blob/master/pics/Untitled.png)
 
3) Installing on a local Grafana (Panel Boom Table):

Use the grafana-cli tool to install Boom Table from the command line

**$ grafana-cli plugins install yesoreyeram-boomtable-panel**

And complete installation run a command

**$ systemclt restart grafana-server.service**

**Features**

Multi column support for graphite, Influx DB, Prometheus & Azure Monitor

Individual thresholds for cells based on pattern

Multi-level thresholds (N number of thresholds)

Individual aggregation method for cell based on pattern

Time based thresholds

Individual cell values can be transformed to helpful texts, based on pattern.

Transformed texts can also contain actual metrics

Icons in metrics

Units can be set at cell level based on pattern

Row/Column name based on multiple graphite/Influx DB/Prometheus columns

Filter metrics

Debug UI to test patterns

**Supported / Tested Data Sources**

Graphite

Influx DB

Prometheus

Azure Monitor

AWS Cloud Watch

Any data sources that returns data in time series format


4) Config Panel Boom Table:

* Add Query and create Legend {{instance}} | Name

* {{instance}} => env  HOST NAME for (row name)

* | => is delimiter

* Name => is key for (col name)


![pic2](https://github.com/ali-nikou/Netdata-Central-with-Promethues-Grafana/blob/master/pics/Untitled2.png) 

* In tab Visualization create default pattern

 ![pic3](https://github.com/ali-nikou/Netdata-Central-with-Promethues-Grafana/blob/master/pics/Untitled3.png)

* Pattern *

* Delimiter |

* Row Name _0_ (means {{instance}})

* Col name $NAME

* And create panel 

 ![pic4](https://github.com/ali-nikou/Netdata-Central-with-Promethues-Grafana/blob/master/pics/Untitled4.png)



**For example, add pattern CPU **

1. Create Queries

![pic6](https://github.com/ali-nikou/Netdata-Central-with-Promethues-Grafana/blob/master/pics/Untitled5.png)
 
Create pattern

* Pattern = key (cputo)

* Delimiter = |

* Row Name = _0_ = {{instance}}

* Col Name = $NAME


2. create pattern

![pic7](https://github.com/ali-nikou/Netdata-Central-with-Promethues-Grafana/blob/master/pics/Untitled6.png)

3. And the output of work

![pic8](https://github.com/ali-nikou/Netdata-Central-with-Promethues-Grafana/blob/master/pics/Untitled7.png)

**And so on, he added other patterns, like the one below**
 
![pic9](https://github.com/ali-nikou/Netdata-Central-with-Promethues-Grafana/blob/master/pics/Untitled8.png)
