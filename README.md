## 1. 🚨 Problem

AI agents are becoming increasingly capable of understanding markets and performing financial tasks.

But giving an AI agent the ability to interact with trading infrastructure introduces an important question:

> **How do we make an AI agent useful without allowing it to take uncontrolled risks?**

A conventional AI trading assistant may:

* Analyze market conditions
* Generate trade ideas
* Calculate indicators
* Recommend entries and exits
* Potentially interact with trading infrastructure

However, analysis alone does not guarantee responsible execution.

A trade can look attractive while still violating a user's:

* Maximum loss
* Position-size limit
* Risk-per-trade limit
* Volatility tolerance
* Trading permissions

**AgentGuard addresses this gap with a dedicated AI risk firewall.**

---

# 2. 💡 Solution

AgentGuard introduces a **Risk Firewall** between the user's request and the trading action.

### Basic workflow

```text
User Trade Request
        ↓
   AgentGuard AI
        ↓
 Market Analysis
        ↓
   Risk Engine
        ↓
 ┌──────┼────────┐
 ↓      ↓        ↓
BLOCK  MODIFY   APPROVE
                 ↓
          Authorized Action
                 ↓
             Audit Log
```

Instead of blindly following:

> "Buy $500 BTC."

AgentGuard evaluates the request against the user's configured risk parameters.

### Example

```text
User:
"Open a $500 BTC position."

AgentGuard:

Requested Position: $500
Maximum Allowed Loss: $10
Estimated Risk: $18.40

Risk Status: ❌ BLOCKED

Reason:
The proposed position exceeds the
user-defined maximum loss.
```

The user can then request:

> "Reduce the position to fit my risk limit."

AgentGuard recalculates the trade.

```text
Recommended Position: $270
Estimated Maximum Loss: $9.70

Risk Status: ✅ APPROVED
```

The objective is not to predict the market perfectly.

The objective is to ensure that **AI-driven actions remain within user-defined boundaries.**

---

# 3. 🏗️ Architecture

```text
                    ┌───────────────┐
                    │     USER      │
                    └───────┬───────┘
                            │
                            ▼
                 ┌────────────────────┐
                 │    AgentGuard AI    │
                 │   Decision Layer   │
                 └─────────┬──────────┘
                           │
             ┌─────────────┼─────────────┐
             ▼             ▼             ▼
       ┌──────────┐  ┌───────────┐  ┌───────────┐
       │  Market  │  │    Risk   │  │ Portfolio │
       │   Data   │  │   Engine  │  │   Data    │
       └────┬─────┘  └─────┬─────┘  └─────┬─────┘
            │              │              │
            └──────────────┼──────────────┘
                           ▼
                  ┌─────────────────┐
                  │  RISK FIREWALL  │
                  └────────┬────────┘
                           │
                 ┌─────────┴─────────┐
                 ▼                   ▼
             ❌ BLOCK             ✅ APPROVE
                                       │
                                       ▼
                             ┌──────────────────┐
                             │  Binance Agent   │
                             │       OS         │
                             └────────┬─────────┘
                                      │
                                      ▼
                              Authorized Action
                                      │
                                      ▼
                                Audit Log
```

### Core components

**AI Agent**

Understands the user's natural-language request and coordinates the analysis.

**Market Data Layer**

Retrieves relevant market information for the requested asset.

**Risk Engine**

Calculates:

* Position size
* Estimated loss
* Risk percentage
* Volatility exposure
* User-defined limits

**Risk Firewall**

Makes the final safety decision:

```text
APPROVE
MODIFY
BLOCK
```

**Binance Agent OS**

Provides the authorized connection to Binance capabilities.

**Audit Layer**

Records the decision, risk checks, and action result for transparency.

---

# 4. 🤖 Agent OS Integration

AgentGuard is designed around **Binance Agent OS** as the execution and agent-infrastructure layer.

The project uses Agent OS capabilities to allow the AI agent to interact with Binance within user-defined permissions.

### Integration concept

```text
AgentGuard
     │
     ▼
Binance Agent OS
     │
     ├── Market Data
     ├── Account Information
     └── Authorized Trading Actions
```

AgentGuard does **not** treat the AI model as an unrestricted actor.

Instead:

```text
AI Decision
     ↓
Risk Firewall
     ↓
Permission Check
     ↓
Authorized Agent OS Action
```

This creates an additional safety boundary around agentic financial actions.

### Why Agent OS?

Agent OS provides the infrastructure required for AI agents to interact with Binance while allowing users to control the capabilities and permissions available to the agent.

AgentGuard builds an application-level risk-management layer on top of that infrastructure.

---

# 5. ✨ Features

## 🧠 Natural-Language Trading Requests

Users can communicate with AgentGuard naturally.

Example:

```text
"Analyze BTC and prepare a $500 long
with a maximum acceptable loss of $10."
```

---

## 🛡️ Risk Firewall

Every proposed trade passes through configurable risk checks.

```text
Position Size
      +
Maximum Loss
      +
Risk Percentage
      +
Market Volatility
      +
User Permissions
      ↓
Risk Decision
```

---

## 📊 Market Analysis

AgentGuard can evaluate relevant market information before making a decision.

Possible analysis includes:

* Price
* Trend
* Momentum
* Volatility
* Support/resistance
* Technical indicators

---

## 📐 Position Sizing

Instead of simply answering:

> "BUY"

AgentGuard can calculate a position size based on the user's risk constraints.

---

## 🚦 Three-Level Decision System

### 🔴 BLOCK

Trade violates the configured risk policy.

### 🟡 MODIFY

Trade may be acceptable after reducing size or changing parameters.

### 🟢 APPROVE

Trade satisfies the configured rules.

---

## 🔐 Permission-Aware Execution

AgentGuard is designed to operate only within the capabilities authorized by the user.

---

## 📝 Decision Audit

Each decision can include:

```text
Request
Market Data
Risk Parameters
Risk Calculation
Decision
Reason
Action
Timestamp
```

This makes the agent's behavior easier to inspect and understand.

---

# 6. ⚙️ Setup

## Requirements

* Python 3.10+
* Node.js 18+
* A Binance account
* Binance Agent OS access
* An AI-compatible development environment
* Git
* Environment variables for configuration

---

## Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/agentguard.git

cd agentguard
```

---

## Create a virtual environment

```bash
python -m venv .venv
```

Activate it:

### Linux / macOS

```bash
source .venv/bin/activate
```

### Windows

```bash
.venv\Scripts\activate
```

---

## Install dependencies

```bash
pip install -r requirements.txt
```

If the project includes a frontend:

```bash
cd ui
npm install
```

---

## Configure environment variables

Create:

```text
.env
```

Example:

```env
AI_API_KEY=your_api_key
BINANCE_API_KEY=your_api_key
BINANCE_API_SECRET=your_api_secret

MAX_RISK_PER_TRADE=0.02
MAX_POSITION_SIZE=500
MAX_LOSS_USDT=10
```

> **Never commit real API keys or secrets to GitHub.**

Use:

```text
.env.example
```

for public configuration documentation.

---

## Run AgentGuard

```bash
python agent/main.py
```

For the dashboard:

```bash
npm run dev
```

> Commands may change depending on the final implementation.

---

# 7. 🔒 Security & Risk Controls

Security is a core part of AgentGuard rather than an afterthought.

### Risk limits

Users can configure:

```text
Maximum Position Size
Maximum Loss
Maximum Risk Per Trade
Maximum Exposure
Allowed Trading Pairs
```

---

### No unrestricted execution

The AI agent should never receive unlimited authority simply because it can reason about a trade.

The intended execution flow is:

```text
User Request
     ↓
AI Analysis
     ↓
Risk Validation
     ↓
Permission Validation
     ↓
Authorized Action
```

---

### Withdrawal protection

AgentGuard should not require withdrawal permissions for its trading workflow.

---

### Emergency Stop

The user should be able to immediately disable the agent's authorized access when necessary.

---

### API Secret Protection

Never store secrets directly in source code.

Use environment variables or an appropriate secret-management system.

---

### Human Override

The user remains in control.

AgentGuard should provide the ability to:

* Reject an action
* Modify parameters
* Disable the agent
* Revoke permissions

---

# 8. 📸 Screenshots

> Screenshots below will be replaced with actual screenshots from the working demo.

### Dashboard

![AgentGuard Dashboard](docs/images/dashboard.png)

### Risk Analysis

![Risk Analysis](docs/images/risk-analysis.png)

### Blocked Trade

![Blocked Trade](docs/images/trade-blocked.png)

### Approved Trade

![Approved Trade](docs/images/trade-approved.png)

### Agent OS Integration

![Agent OS Integration](docs/images/agent-os.png)

### Audit Log

![Audit Log](docs/images/audit-log.png)

---

# 9. 🚀 Future Improvements

AgentGuard is designed as a foundation for safer agentic finance.

Future versions could include:

### Multi-Asset Risk Management

Support BTC, ETH and additional assets with portfolio-level exposure calculations.

### Portfolio Risk Engine

Evaluate the entire portfolio rather than individual trades.

```text
Portfolio
   ↓
Correlation
   ↓
Exposure
   ↓
Volatility
   ↓
Portfolio Risk Score
```

### Adaptive Risk Limits

Allow risk parameters to adjust according to market volatility while remaining within user-defined maximum boundaries.

### Explainable Decisions

Provide a clear explanation for every:

```text
APPROVE
MODIFY
BLOCK
```

decision.

### Simulation Mode

Allow users to test strategies without executing real trades.

### Backtesting

Evaluate the risk framework against historical market conditions.

### Advanced Agent Permissions

Introduce granular permissions for:

* Read-only market data
* Portfolio analysis
* Trading
* Specific assets
* Maximum transaction size

### Real-Time Alerts

Notify users when:

* Risk limits are approached
* Market volatility increases
* An action is blocked
* Agent permissions change

### Multi-Agent Risk Architecture

A future version could separate responsibilities:

```text
Research Agent
      ↓
Trading Agent
      ↓
Risk Agent
      ↓
Execution Agent
```

The Risk Agent becomes an independent verification layer.

---

# 🎥 Demo

## 90-Second Demo Flow

### 1. User request

```text
"Prepare a $500 BTC long.
Maximum acceptable loss: $10."
```

### 2. Agent analyzes

```text
BTC Market Analysis
Trend: Bullish
Volatility: High
```

### 3. Risk Firewall

```text
Requested Position: $500
Estimated Risk: $18.40
Maximum Risk: $10

❌ BLOCKED
```

### 4. User adjusts

```text
"Reduce the position to fit my risk limit."
```

### 5. Agent recalculates

```text
Recommended Position: $270
Estimated Risk: $9.70

✅ APPROVED
```

### 6. Agent OS

The demo then shows the authorized Binance Agent OS interaction.

### 7. Audit

```text
Decision: APPROVED
Position: $270
Risk: $9.70
Reason: Risk policy satisfied
Status: Completed
```

---

# 🎯 Hackathon Focus

AgentGuard is built around one simple idea:

> **Give AI agents useful financial capabilities without removing the user's control over risk.**

The project demonstrates how an AI agent can:

**Understand → Analyze → Calculate Risk → Decide → Act → Explain**

rather than simply:

**Understand → Act**

---

# 🏁 Conclusion

AgentGuard explores a safer architecture for agentic trading.

The goal isn't to build an AI that trades more aggressively.

The goal is to build an AI that can **reason about risk before taking action**.

> ### **AI shouldn't just know how to trade.**
>
> ### **It should know when NOT to trade.**

---

## Built for the Binance Agent OS Mini Hackathon

**Project:** AgentGuard
**Category:** AI Agent / Agentic Finance
**Infrastructure:** Binance Agent OS
**Focus:** AI + Risk Management + Controlled Execution