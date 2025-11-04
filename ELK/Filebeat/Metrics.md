Based on project: 
## Beats
Information that is common to all Beats, e.g. version, goroutines, file handles, CPU, memory

| Metric                             | Type    | Explanation |
| ---------------------------------- | ------- | ----------- |
| cgroup_cpu_id_info                 | gauge   |             |
| cgroup_cpu_stats_periods           | counter |             |
| cgroup_cpu_stats_throttled_ns      | counter |             |
| cgroup_cpu_stats_throttled_periods | counter |             |
| cgroup_memory_id_info              | gauge   |             |
| cgroup_memory_stats_usage_bytes    | gauge   |             |
| cpu_system_ticks                   | gauge   |             |
| cpu_system_time_ms                 | counter |             |
| cpu_total_ticks                    | gauge   |             |
| cpu_total_time_ms                  | counter |             |
| cpu_total_value                    | counter |             |
| cpu_user_ticks                     | gauge   |             |
| cpu_user_time_ms                   | counter |             |
| handles_limit_hard                 | gauge   |             |
| handles_limit_soft                 | gauge   |             |
| handles_open                       | gauge   |             |
| info_empehmeral_id_info            | gauge   |             |
| info_name_info                     | gauge   |             |
| info_uptime_ms                     | gauge   |             |
| info_version_info                  | gauge   |             |
| memstats_gc_next                   | gauge   |             |
| memstats_memory_alloc              | gauge   |             |
| memstats_memory_sys                | counter |             |
| memstats_memory_total              | gauge   |             |
| memstats_rss                       | gauge   |             |
| runtime_goroutines                 | gauge   |             |

## filebeat
Information specific to Filebeat, e.g. harvester, events

| Metric                    | Type    | Explanation                                                                                                                                                                                         |
| ------------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| events_active             | gauge   | Number of events being actively processed by Filebeat (including events Filebeat has already sent to the libbeat publisher pipeline, but not including events the pipeline has sent to the output). |
| events_added              | counter | Event created                                                                                                                                                                                       |
| events_done               | counter | Event successfully delivered AND acknowledged by the destination                                                                                                                                    |
| harvester_closed          | counter | Number of harvesters closed                                                                                                                                                                         |
| harvester_open_files      | gauge   | Current number of open files                                                                                                                                                                        |
| harvester_running         | gauge   | Current number of harvesters running                                                                                                                                                                |
| harvester_skipped         | counter |                                                                                                                                                                                                     |
| harvester_started         | counter | Number of harversters started                                                                                                                                                                       |
| input_log_files_renamed   | counter |                                                                                                                                                                                                     |
| input_log_files_truncated | counter |                                                                                                                                                                                                     |

## libbeat
Information about the publisher pipeline and output, also common to all Beats

| Metric                                | Type    | Explanation                                                                                                                                                                       |
| ------------------------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| config_module_running                 | gauge   | Number of modules in the runner list                                                                                                                                              |
| config_module_starts                  | counter |                                                                                                                                                                                   |
| config_module_stops                   | counter |                                                                                                                                                                                   |
| config_reloads                        | counter | Number of config reloads                                                                                                                                                          |
| config_scans                          | counter | Number of config scans                                                                                                                                                            |
| output_batches_split                  | counter | Number of times a batch was split for being too large                                                                                                                             |
| output_events_acked                   | counter | Number of events accepted by the output's receiver.                                                                                                                               |
| output_events_active                  | gauge   | events sent and waiting for ACK/fail from output                                                                                                                                  |
| output_events_batches                 | counter | Number of calls to the output's Publish function                                                                                                                                  |
| output_events_dead_letter             | counter | Number of failed events ingested to the dead letter index                                                                                                                         |
| output_events_dropped                 | counter | Number of events that were dropped due to a non-retryable error.                                                                                                                  |
| output_events_duplicates              | counter | Number of events rejected by the output's receiver for being duplicates.                                                                                                          |
| output_events_failed                  | counter | Number of events that reported a retryable error from the output's receiver.                                                                                                      |
| output_events_toomany                 | counter | Number of events that failed due to a "429 too many requests" error.                                                                                                              |
| output_events_total                   | counter | Number of events sent to the output's Publish function.                                                                                                                           |
| output_read_bytes                     | counter | total amount of bytes read in the response from output                                                                                                                            |
| output_read_errors                    | counter | total number of errors while waiting for response on output                                                                                                                       |
| ouput_type_info                       | gauge   |                                                                                                                                                                                   |
| output_write_bytes                    | counter |                                                                                                                                                                                   |
| output_write_errors                   | counter |                                                                                                                                                                                   |
| output_write_latency_histogram_count  | counter |                                                                                                                                                                                   |
| output_write_latency_histogram_max    | gauge   |                                                                                                                                                                                   |
| output_write_latency_histogram_mean   | gauge   |                                                                                                                                                                                   |
| output_write_latency_histogram_median | gauge   |                                                                                                                                                                                   |
| output_write_latency_histogram_min    | gauge   |                                                                                                                                                                                   |
| output_write_latency_histogram_p75    | gauge   |                                                                                                                                                                                   |
| output_write_latency_histogram_p95    | gauge   |                                                                                                                                                                                   |
| output_write_latency_histogram_p999   | gauge   |                                                                                                                                                                                   |
| output_write_latency_histogram_stddev | gauge   |                                                                                                                                                                                   |
| pipeline_clients                      | gauge   | clients measures the number of open pipeline clients. On k8s context each pod will use one client, but each pod might have more than one harvester if it has multiple containers. |
| pipeline_events_active                | gauge   | measures events that have been created, but have not yet been failed, filtered, or acked/dropped.                                                                                 |
| pipeline_events_dropped               | counter | counts events that were dropped because errors from the output workers exceeded the configured maximum retry count.                                                               |
| pipeline_events_failed                | counter | counts events that were rejected by the queue, or that were sent via an already-closed pipeline client.                                                                           |
| pipeline_events_filtered              | counter | counts events that were filtered by processors before being sent to the queue.                                                                                                    |
| pipeline_events_published             | counter | counts events that were accepted by the queue.                                                                                                                                    |
| pipeline_events_retry                 | counter | counts events that an output worker sent back to be retried.                                                                                                                      |
| pipeline_events_total                 | counter | counts all created events.                                                                                                                                                        |
| pipeline_queue_acked                  | counter | **DEPRECATED** use removed_events instead                                                                                                                                         |
| pipeline_queue_added_bytes            | counter |                                                                                                                                                                                   |
| pipeline_queue_added_events           | counter | Number of events added to the queue by input workers.                                                                                                                             |
| pipeline_queue_consumed_bytes         | counter |                                                                                                                                                                                   |
| pipeline_queue_consumed_events        | counter | Event was consumed but not ack yet                                                                                                                                                |
| pipeline_queue_filled_bytes           | gauge   |                                                                                                                                                                                   |
| pipeline_queue_filled_events          | gauge   | Event was added but not ack yet                                                                                                                                                   |
| pipeline_queue_filled_pct             | gauge   | How full the queue is relative to its maximum size, as a fraction from 0 to 1.                                                                                                    |
| pipeline_queue_max_bytes              | gauge   |                                                                                                                                                                                   |
| pipeline_queue_max_events             | gauge   | The queue's maximum event count if it has one, otherwise zero. Config: `queue.mem.events`                                                                                         |
| pipeline_queue_removed_bytes          | counter |                                                                                                                                                                                   |
| pipeline_queue_removed_events         | counter | Event was consumed and ack by the output                                                                                                                                          |

## registrar

| Metrics        | Type    | Explaination |
| -------------- | ------- | ------------ |
| states_cleanup | counter |              |
| states_current | gauge   |              |
| states_update  | counter |              |
| writes_fail    | counter |              |
| writes_success | counter |              |
| writes_total   | counter |              |

## system
I don't think this works on k8s.

| Metrics      | Type    | Explanation |
| ------------ | ------- | ----------- |
| cpu_cores    | counter |             |
| load_1       | gauge   |             |
| load_5       | gauge   |             |
| load_15      | gauge   |             |
| load_norm_1  | gauge   |             |
| load_norm_5  | gauge   |             |
| load_norm_15 | gauge   |             |

