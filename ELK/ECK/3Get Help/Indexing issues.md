Tags: #es #index #shards #es-api #ingest-pipeline #indexing #metrics #es-tasks #curl #logs 
## Data takes some time to be ingested
Normally this means that ES is taking some time to ingest the data or the data shipper is taking too much time to send the data. In this document we will check the first case, while the second case will be explored on filebeat subject.
Normally this issue will happen whenever there is some high throughput data on a specific index, or might be a more general issue (for many indices).
In case you have some index that receives a high data throughput it is there is some alternatives:
### Increase CPU for all the machines
**When:** if the degradation of throughput is for a lot of indices and it is possible to see that all nodes are using a lot of cpu (near hardware limit, or available limit).
**Main Metrics:** CPU Usage, Indices indexing time
**Solution:** 
* Increase number of nodes;
* Increase CPU available.
### Increase primary shards
**When:**  There is at least one index  index that is taking to long to indexing data. 
**Main Metrics:** Index indexing time, CPU Usage, ingest pipeline time_in_millis/ total_count
**Solution:** 
* Multiple primary shards will distribute the data for a index across many nodes, this will make the data ingestion more parallel task.
### Increase coordinator nodes
**When:** The requests have huge latencies on client side even thought the client is sending multiple requests in parallel and there is no saturation on cpu for the nodes. It is possible to see a lot of timeouts on client side.
**Main Metrics:**  Quantity of indexing  per node, CPU Usage (low), Tasks
**Solution:** 
* Add more nodes to receive requests in the LB. If using k8s with service LB this service should use the new nodes.
### Add ingest pipeline dedicated nodes
**When:** It is possible to see a high CPU usage, but the indexing rate is low. it is possible to see that ingest pipeline time/event is high and some threads from ingest task are using a lot of CPU (hot spotting)
**Main Metrics:** CPU Usage, ingest pipeline time/events, hot spotting tasks, tasks time running
**Solution:** 
* Crete nodes dedicated to ingest pipeline;
* Remove the role from data nodes.
### Metrics
**CPU Usage**: This metric is straight forward, it will indicate how much of cpu time the node is using.
**How:** 
* This can be done on your monitoring stack, in case you are using k8s the metric would be *container_cpu_usage_seconds_total* .
* Directly in kibana. Stack Monitoring > Nodes > \<specific node>  > Advanced > CPU Utilization
**Hot Spotting:** This will return the threads that are using a lot of CPU, in order to identify what you need you will need to check for specific words, like bulk, ingest, index, and so on, to try to find relevant tasks that might indicate some kind of wrong usage of cpu resource. You can also check for functions, it will provide the stack trace of the code running in the task, in ES Docs you can find some functions used per process (indexing, ingest pipeline, ...)
**How:** ES exposes also a API that shares Hot Spotting, that will be the threads that uses CPU the most:
 ```
   GET /_nodes/hot_threads
 ```
 Output example:
 ```
 ::: {elasticsearch-es-master-2}{vUVX96JhQDG7r1YD7dCDdg}{tQAnuXfqRDaHHRI-Hvk0GQ}{elasticsearch-es-master-2}{192.168.9.142}{192.168.9.142:9300}{mr}{9.1.0}{8000099-9033000}{ml.config_version=12.0.0, k8s_node_name=aks-observabil-19098908-vmss00001p, transform.config_version=10.0.0, xpack.installed=true}

Hot threads at 2025-11-11T14:26:27.211Z, interval=500ms, busiestThreads=3, ignoreIdleThreads=true:

  

::: {elasticsearch-es-master-0}{tJsThGTuRByg-qakyh9rAw}{OdnoP_rnR4m751g5fG-VWw}{elasticsearch-es-master-0}{192.168.11.55}{192.168.11.55:9300}{mr}{9.1.0}{8000099-9033000}{ml.config_version=12.0.0, k8s_node_name=aks-observabil-19098908-vmss000009, xpack.installed=true, transform.config_version=10.0.0}

Hot threads at 2025-11-11T14:26:27.221Z, interval=500ms, busiestThreads=3, ignoreIdleThreads=true:

0.0% [cpu=0.0%, idle=100.0%] (500ms out of 500ms) cpu usage by thread 'elasticsearch[elasticsearch-es-master-0][transport_worker][T#5]'

2/10 snapshots sharing following 4 elements

io.netty.common@4.1.118.Final/io.netty.util.concurrent.SingleThreadEventExecutor$4.run(SingleThreadEventExecutor.java:998)

io.netty.common@4.1.118.Final/io.netty.util.internal.ThreadExecutorMap$2.run(ThreadExecutorMap.java:74)

java.base@24/java.lang.Thread.runWith(Thread.java:1460)

java.base@24/java.lang.Thread.run(Thread.java:1447)

  

::: {elasticsearch-es-master-1}{4YDXTazDQZmH58h2JuXcBA}{gVEMjkVsSWCrzJJ8QBg2MQ}{elasticsearch-es-master-1}{192.168.15.96}{192.168.15.96:9300}{mr}{9.1.0}{8000099-9033000}{ml.config_version=12.0.0, k8s_node_name=aks-observabil-19098908-vmss000012, xpack.installed=true, transform.config_version=10.0.0}

Hot threads at 2025-11-11T14:26:27.222Z, interval=500ms, busiestThreads=3, ignoreIdleThreads=true:

100.0% [cpu=0.0%, other=100.0%] (500ms out of 500ms) cpu usage by thread 'elasticsearch[readiness-service]'

10/10 snapshots sharing following 7 elements

java.base@24/sun.nio.ch.Net.accept(Native Method)

java.base@24/sun.nio.ch.ServerSocketChannelImpl.implAccept(ServerSocketChannelImpl.java:424)

java.base@24/sun.nio.ch.ServerSocketChannelImpl.accept(ServerSocketChannelImpl.java:391)

app/org.elasticsearch.server@9.1.0/org.elasticsearch.readiness.ReadinessService.lambda$startListener$0(ReadinessService.java:176)

app/org.elasticsearch.server@9.1.0/org.elasticsearch.readiness.ReadinessService$$Lambda/0x00000000675be218.run(Unknown Source)

java.base@24/java.lang.Thread.runWith(Thread.java:1460)

java.base@24/java.lang.Thread.run(Thread.java:1447)
 ```

**Quantity of indexing  per node or index:**  This provides the number of indexing data per second. This measurement is a counter that can be extracted from stat endpoint or can be seen on kibana monitoring UI.
**How:** 
* Kibana UI for Node metric: Stack Monitoring > Nodes > \<specific node> > Advanced  > Request Rate
* Kibana UI for Index Metric: Stack Monitoring > Indices > \<Specific Index> > Overview > Request Rate
* Using Node Stats API for Node metric: 
  ```
     GET /_nodes/stats?filter_path=nodes.*.name,nodes.**.indexing
   ```
   * Using Index Stats API for Index Metric:
  ```
    GET /_stats/indexing?filter_path=indices.*.primaries.indexing.index_total
    ``` 
 
 **Ingest Pipeline Metrics:** The main metrics that we are interested is the duration in milliseconds and the total number of events processed by specific ingest pipeline. That way we can see how much time in average it takes to process a event.
 **How:** Using node stats API it will return all the statistics about a specific pipeline, that way we can read mainly `time_in_millis` and `count` from that we can have avg time/event, if this number is too high we can evaluate that maybe it needs more cpu to process the pipeline hence we would need some dedicated nodes to do that instead of sharing cpu with reading and indexing.
 ```
 GET /_nodes/stats/ingest?filter_path=nodes.**.pipelines.<pipeline-name>.*
 ```
 Hint on how to get avg time/event:
 ```
 cat nodes_stats.json | jq -rc '.nodes[]|.name as $n| .ingest.pipelines|to_entries[]| .key as $pi|.value| { name:$n, pipeline:$pi, total_count:.count, total_millis:.time_in_millis, total_failed:.failed }| select(.total_count>0)|.+{time_per:(.total_millis/.total_count|round)}|select(.time_per>0)'
 ```
 
 **Tasks:** Here we can check information of tasks currently running, we can check if there is any tasks that is running for long period of time, beasides tasks that begin in the ES start and finish when it terminates all the other tasks should not be long time tasks, mainly the write ones.
 **How:** We can check tasks using the task API:
 ```
 GET /_tasks?human&detailed
 ```
 Hint: One can use the following script to get the time per action
```
cat tasks_20251110151222.json| jq -r '.nodes[].tasks[]|select(.action|contains("[s]")|not)|[.running_time_in_nanos, .running_time, .cancellable, .node[:5], .action, .description]|@tsv' | sort -nr | head -10 
```