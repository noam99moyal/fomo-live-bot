# Fomo Live Bot

Hourly, paper-first copy trading for [FOMO](https://fomo.family).

It copies traders you **already follow** on FOMO, with locked size and exits. Starts in **paper**. Real orders only after you type `go live` and the FOMO account is logged in and funded.

> **Paper first.** Every stamp and report leads with `mode: paper` or `mode: live` so you never confuse the two.

---

## How to download / install

1. Open **Grok Bot**.
2. Import the public template **Fomo Live Bot** (from the template / share page once it’s published).
3. Open the new agent chat and complete **first-run** (locks below).
4. Sign into FOMO in the agent’s browser when asked (Apple or Google). The bot never finishes login or 2FA for you.
5. Keep money in the FOMO account **before** you ever say `go live`. Empty balance → live is refused with a clear reminder.

This repo is the public docs + source of truth for how the bot behaves. The runnable bot is the Grok Bot template.

---

## How it works

### Signal source

- Signed-in **fomo.family** browser session on the agent’s computer.
- Allowlist = your FOMO **Following** list (trim/rename before lock).
- One allowlist per bot. New follows after lock do **not** auto-join.
- Want two books? Run two Fomo Live Bot agents.

### Cadence

- Hourly routine (`fomo-copy-hourly`) at the top of each hour.
- Reads **new** buys/sells since the last successful run for allowlisted traders only.
- Quiet on a clean run with nothing new.
- **Never fails silent:** session errors, empty live balance, blocked caps, and read failures are reported in chat.

### Size (pick one, then lock)

| Mode | Behavior |
|------|----------|
| **Flat $** | Same dollar size on every copy buy |
| **Mirror % + $ cap** | Percent of their trade size, never above your cap |

Every **new** allowlist buy on the same token can add another fill (same trader again or another follow), unless a risk cap blocks it.

### Exits

- **Stop-loss %** — optional (recommended). From your entry.
- **Primary exit** — exactly one:
  - **Mirror their sells** → then choose **Partial** (same fraction) or **Full on any sell** (first sell closes 100% of yours)
  - **Take-profit** — fixed % from your entry; optional **trail from peak** after (default: skip)
  - **Time stop** — exit after a fixed hold time
- First match wins; the report names which rule fired.

**Trail (optional after TP):** peak = highest price after your entry while still in. Trail % = pullback from that peak. A hard TP that hits first means trail never runs on that trade.

### Risk caps (all optional)

- Max `$` per token (total open in one coin)
- Max concurrent positions
- Max `$` / day (new buy notional, your local calendar day)

### Modes

| Command | Effect |
|---------|--------|
| *(default)* | `mode: paper` |
| `go live` | Real FOMO orders if logged in **and** funded |
| `back to paper` | Return to paper stamps |

Never auto-flips. Never deposits or withdraws for you.

---

## First-run checklist

1. Allowlist — confirm/trim FOMO follows  
2. Size — flat `$` or mirror `%` + cap  
3. Stop-loss — set or skip  
4. Primary exit — mirror / take-profit / time stop (+ mirror style or TP% / optional trail)  
5. Risk caps — set or skip all  
6. Lock summary → enable hourly routine → session check  

---

## Sample report lines

```text
mode: paper
FILL buy TOKEN $25 @ … reason: copy @trader

mode: live
REFUSED go live — FOMO balance empty; fund the account first

mode: paper
SKIP buy TOKEN — max $ per token
```

---

## What this bot will not do

- Invent traders outside your locked allowlist  
- Change size/exits trade-by-trade  
- Complete FOMO login / 2FA / deposits  
- Stay quiet when something failed  

---

## Repo

Public docs for **Fomo Live Bot**. Issues and README updates welcome once the template is live.
