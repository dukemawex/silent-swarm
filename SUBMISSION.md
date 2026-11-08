Title: Silent Swarm: Omission Attacks in Multi-Agent Pipelines and the Limits of Partial-Visibility Monitoring

Short abstract: Agents can sabotage a pipeline just by skipping a required step and claiming success. In 60 runs of an 8-agent free-LLM pipeline (chain and mesh, 0–2 compromised agents), a skipped SSN redaction reached the final output in 70–100% of attack runs and honest peers never flagged it. An LLM monitor reading every status message caught 1/40 attacks; the same monitor reading a hash-chained log of per-step state diffs caught 40/40. Under 25–50% visibility, apparent ~80% detection came from blanket suspicion of unseen agents (75–100% clean-run false alarms). Oversight of omission needs content-level, tamper-evident logs, not more message coverage.

Tracks: 4 Oversight & monitoring (primary), 2 Science of agent networks
Repo: https://github.com/dukemawex/silent-swarm
