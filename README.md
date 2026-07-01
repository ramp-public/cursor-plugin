<p align="center">
  <img src="assets/logo.svg" alt="Ramp" width="96" height="96" />
</p>

<h1 align="center">Ramp for Cursor</h1>

<p align="center">Search, access, and act on your Ramp financial data — right inside Cursor.</p>

Connect your Ramp account to Cursor to manage company finances through natural
conversation, with permission-based access for finance teams and employees. The plugin
bundles Ramp's official agent skills and wires up the Ramp MCP server, so every action
respects the signed-in user's role and lands in the Ramp audit log.

## What you can do

- **Analyze company spend** — Pull transactions and bills, break spend down by vendor,
  category, team, or date range, and flag vendor-name variants and refunds.
- **Clear approvals** — Review and approve/reject pending transactions, reimbursements, and
  requests (POs and fund requests).
- **Clean up transactions** — Add memos and coding, complete missing details, and resolve
  receipt/memo compliance gaps.
- **Run AP and procurement** — Search and draft bills, look up payments, and move
  procurement requests forward.
- **Handle employee tasks** — Manage cards (activate/lock), submit reimbursements, upload
  receipts and vendor documents, and book travel.
- **Make agent purchases** — Generate Ramp Agent Card credentials with built-in spend
  controls.

## Setup

These skills drive the **Ramp CLI**, so install and authenticate it first:

```bash
# install (or: brew install ramp-public/ramp/ramp-cli)
curl -fsSL https://agents.ramp.com/install.sh | sh

# authenticate via browser OAuth
ramp auth login
```

The plugin also ships the **Ramp MCP server** (`mcp.json` → `https://mcp.ramp.com/mcp`) for
tool-based, natural-language access. Connect it under **Cursor Settings → Tools & MCP**
(Cursor runs Ramp's OAuth flow), then restart Cursor. To explore with sample data and no
Ramp account, point the server at the demo URL instead:

```json
{
  "mcpServers": {
    "ramp": { "url": "https://demo-mcp.ramp.com/mcp" }
  }
}
```

Use the exact URL with no trailing slash.

## Sample prompts

- "Analyze our company spend over the last 90 days — break it down by vendor and category,
  and call out the biggest increases vs. the prior 90 days."
- "What needs my approval?" — or run `/ramp-approvals`
- "Which of my transactions are missing receipts or memos? Fix the coding where you can."
- "Find the payment for invoice #4401 and tell me its status."

## What's included

The `skills/` directory is vendored 1:1 from Ramp's canonical, maintained skill set in
[ramp-public/ramp-cli](https://github.com/ramp-public/ramp-cli/tree/main/src/ramp_cli/skills)
and kept current automatically (see [Skill sync](#skill-sync)).

| | |
|---|---|
| **Skills (14)** | `spend-analysis`, `approval-dashboard`, `transaction-cleanup`, `receipt-compliance`, `manage-bills`, `payment-lookup`, `card-management`, `submit-reimbursement`, `manage-procurement`, `vendor-document-upload`, `book-flight`, `agentic-purchase`, `apply-to-ramp`, `browser-automation` |
| **MCP server** | `ramp` — connects Cursor to Ramp over MCP (`https://mcp.ramp.com/mcp`) |
| **Rule** | `ramp-safety` — always-on guardrails for money handling, write confirmation, and pagination |
| **Command** | `/ramp-approvals` — surface and clear your pending approval queue |

## Skill sync

`skills/` mirrors
[ramp-public/ramp-cli :: src/ramp_cli/skills](https://github.com/ramp-public/ramp-cli/tree/main/src/ramp_cli/skills)
exactly (only `SKILL.md` files). One GitHub Action
([.github/workflows/sync-skills.yml](.github/workflows/sync-skills.yml)) keeps it current:
it runs daily (and on demand via **Run workflow**), re-mirrors the canonical skills, and
opens a PR whenever the result differs from what's committed.

That single job handles drift in both directions — upstream changes *and* any local
hand-edits get reconciled back to canonical on the next run, so the mirror is
self-healing. Don't hand-edit vendored `SKILL.md` files; custom guidance lives in the
`ramp-safety` rule and the `/ramp-approvals` command instead.

> The sync PR step uses the default `GITHUB_TOKEN`. Ensure the org/repo setting
> **Allow GitHub Actions to create and approve pull requests** is enabled.

## Safety

Ramp tools act on a live financial account and most writes cannot be undone. The bundled
`ramp-safety` rule enforces:

- Show item details before approving/rejecting — never blind-approve.
- Confirm with the user before any write, especially bulk operations.
- Rejections require a reason.
- Convert amounts correctly (transactions are formatted strings; bills and reimbursements
  are numeric dollars).
- Paginate every queue to completion.

## License

[MIT](LICENSE)
