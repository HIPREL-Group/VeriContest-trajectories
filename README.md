# VeriContest trajectories

Trajectories of the entries on the [VeriContest](https://github.com/HIPREL-Group/VeriContest)
leaderboard. Each entry's metadata, results and verdicts are in `submissions/<entry-id>/` of the
VeriContest repository; this repository holds its trajectories.

## Layout

```text
<release>/<entry-id>.jsonl.gz          one file per entry
<release>/<entry-id>-000.jsonl.gz      shards, when an entry's file would exceed 50 MB
<release>/<entry-id>-001.jsonl.gz
```

Each file is gzip-compressed JSON Lines: one JSON object per problem, in the order of the
release's `leaderboard/problem_ids.txt`. Shards split that sequence into consecutive parts.

## Records

Every record has `problem_id` (for example `leetcode/lc1`), `task`, `input_mode` and `model`.

- **Model track:** `request` (the prompt template, the problem files it is filled with, the
  exact prompt sent to the model with its SHA-256, and the generation parameters) and
  `response` (the raw model response).
- **Agent track:** the run's steps in order: every model call with its prompt and response,
  every tool, compiler and Verus call with its output, every repair step, and the final
  candidate selection.

## Reading

```bash
zcat 2026-10.1/<entry-id>.jsonl.gz | head -n 1
```

```python
import gzip, json

with gzip.open("2026-10.1/<entry-id>.jsonl.gz", "rt") as f:
    for line in f:
        record = json.loads(line)
```
