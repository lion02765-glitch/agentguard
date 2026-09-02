# 🛡️ AgentGuard

### AI-Powered Risk Control for Binance Agent OS

**Binance Agent OS Mini Hackathon — Track A**

AgentGuard is a lightweight application-level safety layer for an AI financial agent. It evaluates a proposed trading action against deterministic risk and policy controls before the action can reach an execution adapter.

> **AI proposes → AgentGuard evaluates → Policy checks → APPROVE / BLOCK → Controlled execution**

## Demo

The included demo runs locally with Python's standard library. It intentionally uses a **mock execution adapter** so no real order can be placed accidentally.

### Demo flow

1. Enter a natural-language request such as:
   `Open a 1,000 USDT BTC/USDT long using 20x leverage`
2. AgentGuard parses the intent.
3. The risk engine calculates a score.
4. The policy engine checks hard limits.
5. The system returns **BLOCKED** or **APPROVED** with explanations.
6. Approved actions can be sent to the mock Agent OS adapter.
7. Every decision is written to `data/audit.jsonl`.

## Architecture

```text
User
  |
  v
AI Agent / Intent Parser
  |
  | Proposed Action
  v
+-----------------------------+
|         AgentGuard          |
|                             |
|  Risk Engine                |
|  Policy Engine              |
|  Decision Engine            |
|  Audit Logger               |
+-------------+---------------+
              |
        +-----+-----+
        |           |
      BLOCK       APPROVE
                    |
                    v
          Agent OS Adapter
                    |
                    v
             Controlled
              execution
```

## Safety model

Default demo policies:

| Policy | Limit |
|---|---:|
| Maximum leverage | 10x |
| Maximum position | 1,000 USDT |
| Maximum account exposure | 20% |
| Maximum estimated loss | 50 USDT |
| Stop-loss required | Yes |
| Emergency stop | Yes |

These are **demo values**, not trading advice.

## Binance Agent OS integration

Binance Agent OS currently connects AI agents to Binance capabilities including trading and market data through its MCP server and other tools. Users control agent permissions, funding and access.

AgentGuard is intentionally an **additional application-level policy gate**. It does not replace Binance's native permissions, sub-account isolation, disconnect controls or emergency stop.

The repository therefore keeps the execution boundary in `agent_os_adapter.py`.

For a hackathon demo, use the included mock adapter. A real Agent OS/MCP client should be connected only after validating the exact current MCP tool schemas and using least-privilege permissions.

## Run

Requires Python 3.10+.

```bash
python server.py
```

Open:

```text
http://127.0.0.1:8000
```

No API key is required for the demo.

## Test

```bash
python -m unittest discover -s tests -v
```

## Example requests

### High-risk request

```text
Open a 1000 USDT BTC/USDT long using 20x leverage with a stop loss at 66000
```

Expected:

```text
BLOCKED
Reason: leverage exceeds the 10x policy.
```

### Compliant request

```text
Open a 500 USDT BTC/USDT long using 5x leverage with a stop loss at 66000
```

Expected:

```text
APPROVED
```

## Project files

```text
agentguard/
├── server.py
├── agent.py
├── risk_engine.py
├── policy_engine.py
├── decision_engine.py
├── audit.py
├── agent_os_adapter.py
├── config.py
├── requirements.txt
├── .env.example
├── tests/
│   └── test_agentguard.py
├── data/
│   └── .gitkeep
├── docs/
│   └── SECURITY.md
└── web/
    └── index.html
```

## Security notes

- Never commit API keys.
- Keep withdrawals disabled.
- Prefer an isolated/sub-account for agent activity.
- Use least-privilege permissions.
- Keep the mock adapter enabled while recording the demo.
- Add human approval for high-impact actions in a production system.
- Treat risk checks as defense-in-depth, not a guarantee of safety.

## Hackathon positioning

AgentGuard's differentiator is the explicit **policy-before-execution boundary**:

> **AI decides. AgentGuard verifies. Policies control. Execution only follows an approved decision.**

## Disclaimer

This is a hackathon demonstration, not financial advice or a production trading system. Digital-asset trading carries substantial risk.