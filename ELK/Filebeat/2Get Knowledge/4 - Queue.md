tags: #undesrtanding-concepts #filebeat #beats #libbeats 
This is the **heart of the pipeline's resilience**. The batch of events is placed into a queue.
- **Purpose:** Absorb backpressure and survive output failures.
- **Types of Queues:**
    - **In-Memory Queue (default):** Batches are held in RAM. Very fast, but data is lost if Filebeat is terminated.
    - **Disk-based Queue (persistent queue):** Batches are written to a local disk spool _before_ being acknowledged by the output. This is crucial for data durability.