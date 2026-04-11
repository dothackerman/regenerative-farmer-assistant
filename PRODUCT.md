# Regenerative Farmer Assistant

A pre-configured OpenClaw deployment for regenerative farmers. The user connects Telegram and starts working. Everything else is locked.

## Product Architecture

Two layers:

**Product layer (locked, version-controlled):**
- AGENTS.md, SOUL.md
- Exec approval rules, DM/group policy, tool access
- Memory and local embedding search config
- Gardening agent skill (wraps `ga` CLI from gardening-agent project)

**User layer (filled on setup):**
- Telegram token
- API keys
- Location

The user cannot override product-layer config. This is the core architectural requirement that OpenClaw currently does not support (no config layering system exists yet).

## Deployment

- Alpha testers run directly on their laptop
- Update mechanism required regardless of target
- Scale to VPS or laptop+VPS in parallel based on user need
- Target does not constrain architecture decisions

## Components

| Component | Location | Role |
|---|---|---|
| OpenClaw | System dependency | Agent runtime, Telegram, session management |
| Gardening agent | `~/Projects/private/gardening-agent/` | Core intelligence: voice notes, transcription, structured extraction |
| This project | Product packaging | Locked config, skill plugin, deployment, onboarding |

The gardening agent connects to this product via an OpenClaw skill plugin (`skills/gardening/SKILL.md`).

## What Needs to Happen

1. **OpenClaw config layering**: product-layer configs that the user cannot override. This is a feature request to OpenClaw itself.
2. **Gardening agent refinement**: the `ga` CLI needs production hardening (separate sessions, tracked in gardening-agent repo)
3. **Skill plugin**: wraps `ga` CLI for the OpenClaw agent loop (scaffolded in `skills/gardening/`)
4. **Deployment scripts**: setup script that installs gardening-agent, configures OpenClaw, connects Telegram
5. **Alpha test with first user**: scoped functionality, feedback loop

## Alpha Testing

First user is a direct regenerative farmer, onboarded 2026-04-11. Full-day session (9:30-16:00) confirmed the product idea is strong. Practical issues surfaced around OpenClaw fine-grained security config during onboarding.

Scope for alpha needs to be defined in a separate session.
