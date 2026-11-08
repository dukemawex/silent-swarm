# Silent Swarm

**Omission attacks in multi-agent LLM pipelines, and the limits of partial-visibility monitoring.**
Swarm Alignment Hackathon 2026 (AI Alignment Foundation × Apart Research). Tracks 4 (Oversight & monitoring) and 2 (Science of agent networks).
Author: Emmanuel Duke, DUKERS LTD. Report: [`report.pdf`](report.pdf)

## What it does
8 LLM agents (free `nvidia/nemotron-3.5-lightning:free`) process a record through 8 verifiable steps (name, **SSN redaction**, EUR→USD, tax, total, flag, email, summary).
0, 1 or 2 agents are secretly told to *skip* their step and claim success. Chain and mesh (broadcast board) topologies.
An LLM monitor (`nvidia/nemotron-3-ultra-550b-a55b:free`) sees 25/50/100% of log entries, as **status messages only** or as a **ProofTrace-style hash-chained log with state diffs**.

## Headline results (60 runs, n=10/cell, 95% Wilson CIs, see `results/summary.md`)
- Skipped SSN redaction reached the final output in 70–100% of attack runs; mesh peers never raised an alert (0/20).
- Skipped currency conversion was always repaired downstream (0/20 survived): damage depends on whether later steps re-derive the field.
- Full visibility: message-only monitor caught **1/40** attacks; diff log caught **40/40** [91–100%].
- Partial visibility: ~80% "detection" is driven by flagging unseen agents (75–100% clean-run false alarms). Report precision, not just recall.

## Reproduce
```bash
pip install requests matplotlib reportlab
export OPENROUTER_API_KEY=...        # free-tier models only
python3 src/swarm.py 10              # runs 60 pipelines (resumable, cached)
python3 src/analyze.py               # -> results/summary.md, figures/
python3 src/report.py                # -> report.pdf
```
`results/cache.jsonl` holds every model response, so `analyze.py` replays exactly with no API calls. `results/runs.jsonl` has full per-run logs (messages, diffs, hashes, monitor outputs).

## Files
- `src/swarm.py`: task, agents, attacks, topologies, monitor, hash chain
- `src/analyze.py`: metrics + figures
- `src/report.py`: builds the PDF
