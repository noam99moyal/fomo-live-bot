# Fomo Live Bot

Hourly copy trading for [FOMO](https://fomo.family)

Copies traders you already follow on FOMO. Size and exits are locked before the bot runs. 

Starts in paper. Real orders only after you type `go live` and the FOMO account is logged in and funded.

Every stamp and report starts with `mode: paper` or `mode: live`.
---

## Install

1. Open the template: **[Fomo Live Bot on x.ai](https://x.ai/bot/4fmde5VhCGJGeCiqn96D3)**
2. Click **Add to Grok Bot** (install [Grok Bot](https://docs.x.ai/grok-bot/get-started) first if needed).
3. Open the new agent chat and finish **first-run** (checklist below).
4. Sign into FOMO in the agent's browser when asked (Apple or Google). Fund the FOMO account before you type `go live`.

Direct link: https://x.ai/bot/4fmde5VhCGJGeCiqn96D3

---

## How it works

### Signals

- Uses a signed-in **fomo.family** browser session on the agent's computer.
- Allowlist = your FOMO **Following** list. Trim or rename before you lock.
- One allowlist per bot. New follows after lock do not join automatically.
- Two books? Run two Fomo Live Bot agents.

### Cadence

- Hourly routine (`fomo-copy-hourly`), top of each hour.
- Reads new buys and sells since the last successful run, allowlisted traders only.
- Clean run with nothing new: quiet.
- Failures are never silent: session errors, empty live balance, blocked caps, and read failures show up in chat.

### Size (pick one, then lock)

| Mode | Behavior |
|------|----------|
| **Flat $** | Same dollar size on every copy buy |
| **Mirror % + $ cap** | Percent of their trade size, never above your cap |

A new allowlist buy on the same token can add another fill (same trader again, or another follow), unless a risk cap blocks it.

### Exits

- **Stop-loss %** (optional, recommended). Measured from your entry.
- **Primary exit** (pick exactly one):
  - **Mirror their sells**, then **Partial** (same fraction) or **Full on any sell** (first sell closes 100% of yours)
  - **Take-profit**: fixed % from your entry; optional **trail from peak** after (default: off)
  - **Time stop**: exit after a fixed hold time
- First match wins. The report names which rule fired.

**Trail (optional after TP).** Peak = highest price after your entry while still open. Trail % = pullback from that peak. If a hard TP hits first, trail does not run on that trade.

### Risk caps (all optional)

- Max `$` per token (total open in one coin)
- Max concurrent positions
- Max `$` / day (new buy notional, your local calendar day)

### Modes

| Command | Effect |
|---------|--------|
| *(default)* | `mode: paper` |
| `go live` | Real FOMO orders if logged in **and** funded |
| `back to paper` | Back to paper stamps |

Never flips on its own. Never deposits or withdraws for you.

---

## First-run checklist

1. Allowlist - confirm or trim FOMO follows
2. Size - flat `$` or mirror `%` + cap
3. Stop-loss - set or skip
4. Primary exit - mirror / take-profit / time stop (+ mirror style, or TP% / optional trail)
5. Risk caps - set or skip all
6. Lock summary, enable hourly routine, session check

---

## Sample report lines

```text
mode: paper
FILL buy TOKEN $25 @ … reason: copy @trader

mode: live
REFUSED go live - FOMO balance empty; fund the account first

mode: paper
SKIP buy TOKEN - max $ per token
```

---

## What this bot will not do

- Invent traders outside your locked allowlist
- Change size or exits trade by trade
- Complete FOMO login, 2FA, or deposits
- Stay quiet when something failed

---

## Repo

Public docs for **Fomo Live Bot**: https://github.com/noam99moyal/fomo-live-bot

Runnable template: https://x.ai/bot/4fmde5VhCGJGeCiqn96D3
