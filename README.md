# Fomo Live Bot

An automated bot that mirrors trader activity on [FOMO](https://fomo.family), hourly.

Fomo Live tracks the traders you choose to follow and copies their moves to your own account — in paper or live mode — using entry and exit rules you set in advance.

---

## Install

1. Open the Grok Bot template: **[Fomo Live Bot on x.ai](https://x.ai/bot/4fmde5VhCGJGeCiqn96D3)**
2. Open the new agent chat and finish **first-run** (checklist below).

---

## How it works

### FOMO login and funds

The bot reads follows and trades from a signed-in **fomo.family** browser session. You sign in yourself (Apple or Google). The bot does not finish login, 2FA, deposits, or withdrawals.

**Paper** does not need a FOMO balance. **Live** does. Money must already be in the FOMO account before you type `go live`. If the session is missing or the balance is empty, live is refused and you get a clear reminder.

### Paper and live

Starts in paper. Real orders only after you type `go live` and the FOMO account is logged in and funded.

Every stamp and report starts with `mode: paper` or `mode: live`.

| Command | Effect |
|---------|--------|
| *(default)* | `mode: paper` |
| `go live` | Real FOMO orders if logged in **and** funded |
| `back to paper` | Back to paper stamps |

Never flips on its own. Never deposits or withdraws for you.

### Signals

- Uses a signed-in **fomo.family** browser session on the agent's computer.
- Allowlist = your FOMO **Following** list. Trim or rename before you lock.
- One allowlist per bot. New FOMO follows after lock are not copied automatically: on weekdays the bot checks Following vs the allowlist and asks if you want to add anyone new (never auto-adds).
- Each bot is one book (one allowlist + one size/exit setup). Want a second group of traders or different rules? Run a second Fomo Live Bot.

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
