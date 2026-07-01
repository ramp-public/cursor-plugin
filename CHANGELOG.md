# Changelog

All notable changes to this plugin are documented here. This project adheres to
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Changed

- Switched the canonical skill source to
  [ramp-public/ramp-cli](https://github.com/ramp-public/ramp-cli/tree/main/src/ramp_cli/skills)
  (the maintained, tested set) — `skills/` now mirrors its 14 CLI-based skills instead of
  the 3 from `mcp-skills`.
- `ramp-safety` rule and `/ramp-approvals` command updated for the CLI surface (correct
  money conventions; no hardcoded MCP tool names).
- README documents the Ramp CLI prerequisite (`ramp auth login`) alongside the MCP server.
- The sync workflow now self-heals drift: it re-mirrors canonical and opens a PR whenever
  the committed skills differ (covers upstream changes and local hand-edits) — no separate
  drift-check needed.

## [0.1.0] - 2026-06-17

### Added

- Initial release of the Ramp plugin for Cursor.
- `ramp` MCP server wired to production (`https://mcp.ramp.com/mcp`).
- Skills vendored from [ramp-public/mcp-skills](https://github.com/ramp-public/mcp-skills):
  `approval-dashboard`, `transaction-cleanup`, `vendor-analysis`.
- `ramp-safety` always-on rule with money-handling and write-confirmation guardrails.
- `/ramp-approvals` command for clearing the pending approval queue.
- GitHub Action that syncs `skills/` from upstream daily and opens a PR on change.
