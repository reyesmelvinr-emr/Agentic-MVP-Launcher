# Event Log as the State Source

The append-only `timeline.jsonl` is the canonical record of authorized project-state transitions, and `PROJECT_STATE.yaml` is a rebuildable projection. The event stream records state transitions only; this favors deterministic recovery over a snapshot-only model without turning external tool effects into replayable actions.