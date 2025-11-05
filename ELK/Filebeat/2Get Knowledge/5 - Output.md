tags: #undesrtanding-concepts #filebeat #beats #libbeats 
This component takes batches from the queue and manages the connection to the actual output(s).
- **Purpose:** Handle the connection logic, retries, and load balancing.
- **Key Behaviors:**
    - **Retry Logic:** If a batch fails to send (e.g., network blip, ES 429 Too Many Requests), the output client will retry according to a backoff strategy. By default it uses exponential backoff.
    - **Load Balancing:** If you have multiple endpoints defined for an output, the client will load balance between them.
    - **Compression:** Often handles on-the-wire compression (e.g., gzip) for the batch before sending it over the network.