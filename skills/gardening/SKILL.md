---
name: gardening
description: Interact with the gardening agent. Voice note processing, event/observation logging, crop management, garden zones, and status overview. Entry point for all land memory operations.
metadata: {"openclaw": {"requires": {"bins": ["ga"]}}}
---

# Gardening Agent Skill

This skill wraps the `ga` CLI from the gardening-agent project. All gardening data operations go through this interface.

## Entry Point

Run `ga` with no arguments for the help overview. All commands emit JSON.

## Commands

### Pipeline

```bash
ga process <path.m4a>           # Transcribe + extract in one step
ga process --all-pending         # Process all unprocessed recordings
ga transcribe <path.m4a>         # Transcribe audio only
ga extract <path.json>           # Extract events/observations from transcript
ga extract --all-pending         # Extract all unprocessed transcripts
```

### Data Entry

```bash
ga add-crop --crop <name> [--variety <v>] [--lifecycle annual|perennial|biennial] [--aliases "a,b"]
ga log-event --event-type <type> --description <text> [--date YYYY-MM-DD] [--crop <c>] [--garden <g>]
ga log-obs --obs-type <type> --description <text> [--date YYYY-MM-DD] [--crop <c>] [--garden <g>] [--hypothesis <h>]
ga add-zone --garden <name> --zone-id <id> --zone-type <type> [--tunnel] [--trellis] [--soil-type <s>]
```

### Status and Flags

```bash
ga status                        # Pipeline state + open flags overview
ga flags                         # List open flags
ga flags resolve <flagged_at> "<note>"   # Resolve a flag (promotes held items)
ga flags dismiss <flagged_at> "<note>"   # Dismiss a flag (discards)
```

## When to Use

- User mentions garden, crops, planting, harvest, soil, or land observations
- Voice note arrives that relates to gardening activity
- User asks about garden status, pending recordings, or flags
- User wants to log an event or observation
- User asks for seasonal planning or crop history

## Data Model

Two primary types:
- **Events**: actions taken (seeded, transplanted, harvested, fertilized). Build timeline memory.
- **Observations**: things noticed (pest presence, soil moisture, plant vigor). Build insight memory.

Both carry confidence scores and record IDs for idempotent writes.

## Notes

- The gardening-agent project lives at a separate root. This skill is the bridge.
- `ga status` is the starting point for any session involving garden data.
- All output is JSON, optimized for agent consumption.
