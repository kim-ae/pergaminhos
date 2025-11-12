## Elasticsearch
### Check failed pipeline runs above 10%
#ES #elasticsearch #eck #metrics #stats #ingest-pipeline
```
cat nodes_stats.json | jq -c '.nodes[]|.name as $node| .ingest.pipelines|to_entries[]| .key as $pipeline| .value.processors[] | to_entries[]|.key as $process| { pipeline:$pipeline, process:$process, node:$node, total:.value.stats.count, failed:.value.stats.failed, failed_percent:(try (100*.value.stats.failed/.value.stats.count|round) catch 0)}| select(.total>0 and .failed_percent>10)'
```

### Time per event on each pipeline
#ES #elasticsearch #eck #metrics #stats #ingest-pipeline
```
cat nodes_stats.json | jq -rc '.nodes[]|.name as $n| .ingest.pipelines|to_entries[]| .key as $pi|.value| { name:$n, pipeline:$pi, total_count:.count, total_millis:.time_in_millis, total_failed:.failed }| select(.total_count>0)|.+{time_per:(.total_millis/.total_count|round)}|select(.time_per>0)'
```

### Top 10 long running tasks
#es #elasticsearch #eck #metrics #tasks
```
cat tasks.json| jq -r '.nodes[].tasks[]|select(.action|contains("[s]")|not)|[.running_time_in_nanos, .running_time, .cancellable, .node[:5], .action]|@tsv' | sort -nr | head -10   
```