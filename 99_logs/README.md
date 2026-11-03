# 99_logs

Logs for **auditing** — so your whole process is traceable and reproducible.

Keep here:
- command logs / stdout + stderr from meaningful runs (with timestamps)
- a record of tool versions (`tool --version >> 99_logs/tool_versions.txt`)
- any run notes

A simple pattern, run from the repo root:

```bash
{ echo "### $(date -Is)  <what this is>"; <your command>; } 2>&1 | tee -a 99_logs/<stage>.log
```

We should be able to open these logs and see exactly what was run, with which versions,
and when.
