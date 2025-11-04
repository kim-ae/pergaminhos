As soon as a log line is read by a harvester, it becomes a "beat.Event" and enters the processor chain. This is where you can manipulate and enrich the data _in-memory, before it is queued_.
- **Purpose:** Decorate, filter, and reduce the volume of your data.
- **Common Processors:**
	- `add_fields`, `add_tags`: Add metadata (e.g., `env: prod`, `team: platform`).
	- `drop_fields`: Remove unnecessary fields to save bandwidth.
    - `dissect`, `grok`: Parse unstructured log lines into structured key-value pairs.
    - `decode_json_fields`: Parse JSON strings into proper JSON objects.
    - `drop_event`: Discard events that match certain conditions (e.g., health checks).
**Processors run in the "core" (pipeline)** and are better for:
- Enrichment (adding host metadata, tags)
- Complex filtering logic
- Cross-event correlation
- Output-specific formatting
## Hints
* Filter early

## Processor throughput Impact
Processor Type        | Typical Impact | High-Volume Impact
----------------------|----------------|-------------------
add_fields/drop_fields| 0-2%           | 1-5%
decode_json_fields    | 2-10%          | 5-20%
dissect               | 5-15%          | 10-30%
grok                  | 15-40%         | 25-60%
script (javascript)   | 20-50%         | 35-75%
dns (network calls)   | 10-30%         | Highly variabl
