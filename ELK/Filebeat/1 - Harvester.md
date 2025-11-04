**NDJSON parsers (and other input parsers like `multiline`) are applied in the harvester, NOT in the processor chain.**
### What happens in the Harvester:
1. **Raw Byte Reading**: The harvester reads raw bytes from the file
2. **Line Breaking**: Converts the byte stream into logical lines (handles `\n`, `\r\n`, etc.)
3. **Parser Application**: Applies configured parsers in sequence:
    - **NDJSON**: Parses each line as JSON, validates structure, extracts fields
    - **Multiline**: Aggregates multiple physical lines into single logical events
    - **Container**: Parses CRI/log format
4. **Event Creation**: Creates the initial beat.Event with parsed data
**Input parsers run at the "edge" (harvester)** where data is ingested, making them ideal for:
- Structural transformations (JSON, multiline)
- Basic validation
- Initial field extraction