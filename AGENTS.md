## Agent skills

### Issue tracker

Issues are tracked in this repo's GitHub Issues (`The-Resonance-Team/odoo`) via the `gh` CLI. See `docs/agents/issue-tracker.md`.

### Triage labels

The five canonical triage labels are used as-is (`needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, `wontfix`). See `docs/agents/triage-labels.md`.

### Domain docs

Single-context layout: `CONTEXT.md` and `docs/adr/` at the repo root. See `docs/agents/domain.md`.

## Toolchain

This is a Bun-native repo (`packageManager: bun@1.4.6`): use `bun install`, `bun fmt`, `bun lint`, and `bun check`. Turborepo runs the workspace tasks.

## Packages

Packages are deep modules: see [packages/README.md](./packages/README.md) before adding or importing one. If a boundary case has no clean solution, ask the user instead of bypassing the rule.
