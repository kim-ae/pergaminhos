#filebeaet #metrics #logs #queue #performance 
## Saturation queue 1
**The Problem:** Whenever the metric `queue.fillied.pct` is 1 it indicates that the queue is 100% filled. That means that the output is not able to consume all events that filebeat is processing. **Possible Issues:** 
* This can be a result of some issue on output side, for that you should check ELK document called [[Performance Issues]]. 
* Filebeat output threads are underutilized.
**Solution:** To be sure that output threads are been utilized we can adjust 3 paramters: `output.elasticsearch.workers`, `queue.mem.events` and `output.elasticsearch.bulk_max_size`. Those 3 parameters must be change together in order to increase filebeat throughput, the default configurations for those parameters are 1 worker, max of 3200 events in memory and bulk max size of 1600 events. In case we want to try to have 2 parallel threads trying to send data to elasticsearch, if we bump the worker number to 2 we will have possible 3200 events in flight at queue at most, because while the events are not ack by the output it will not be removed from queue, hence we can not have more than 2 workers working in parallel .If we want to be able to have more parallelism we need to have multiples of 1600 as queue limit.
Decide the values by following this formulae: `queue.mem.events = output.elasticsearch.workers x output.elasticsearch.bulk_max_size`
## High Output Latency
**The Problem:** Filabeat output is taking to long to complete the requests. 
**Possible Issues:** 
* This can be result of some output issue, you can check the documentation [[Performance Issues]]. 
* It also can be a result of too big bulks to be processed, not necessarily the latency high will be an issue. Be sure to check the p95 and p99 if indeed it has a high value (20+s).
**Solution:** Decrease the size of `output.elasticsearch.bulk_max_size`.

## High input low output
**The problem:** It is possible to see that haversters are ingesting a lot of events but it is not possible to see that many events going to the queue. 
**Possible Issues:** This might indicate that the processor chain is taking too long.
**Solution:** 
* Check if the processors are doing complex logic that could be shifted to ingest pipeline;
* Try to filter better which events the processors are applied to.