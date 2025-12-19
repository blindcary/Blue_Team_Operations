# Lab 02: Log Analysis at Scale (Grep & Awk)

## Objective
Transform raw, unstructured audit logs into a clean, readable report for management and technical analysis.

## Tools Used
- `grep` (Pattern matching)
- `awk` (Data extraction and reporting)
- `wc` (Counting and statistics)

## Methodology
1. **Raw Data Extraction:** Used `ausearch` with the `--raw` flag to feed data into a pipeline.
2. **Filtering:** Used `grep` to isolate only "SYSCALL" events (the actual execution of commands).
3. **Slicing:** Used `awk` to print specific columns (Timestamp and Executable Path).

## Findings (To be filled)
- **Extracted Column 2 (Timestamp):** [Insert Example Here]
- **Extracted Column 28 (Executable):** [Insert Example Here]
- **Total Alert Count:** [Insert result of wc -l here]

## Conclusion
By using command-line pipelines, a SOC analyst can filter through thousands of logs in seconds to find specific malicious behaviors.
