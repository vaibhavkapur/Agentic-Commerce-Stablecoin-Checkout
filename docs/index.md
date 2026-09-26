---
layout: default
title: Home
nav_order: 1
---

# Agentic Commerce + Stablecoin Checkout

A conversational commerce demonstration that turns supported natural-language requests into policy-controlled, simulated USDC payments. Merchant adapters, three policy layers, chain selection, and receipts run behind a chat interface.

[Get Started](getting-started.md) · [API Reference](api-reference.md) · [Repository README](https://github.com/vaibhavkapur/Agentic-Commerce-Stablecoin-Checkout/blob/main/README.md)

## Documentation

- [Getting Started](getting-started.md)
- [Architecture](architecture.md)
- [API Reference](api-reference.md)
- [Configuration](configuration.md)
- [Data Store](data-store.md)
- [Testing](testing.md)
- [Deployment & Roadmap](deployment.md)
- [Intent Parser](intent-parser.md)
- [Policy Engine](policy-engine.md)
- [Chain Selection](chain-selection.md)
- [Merchant Adapters](merchants.md)
- [Payment Execution](payment-execution.md)
- [UI Components](ui-components.md)

## Key Features

| Feature | Description |
|:--------|:------------|
| **Natural-Language Payments** | Parse commands like "Book me a $50 ride to JFK" into structured payment intents |
| **3-Layer Policy Engine** | Static rules, session delegation, and dynamic risk scoring evaluated in sequence |
| **Multi-Chain Routing** | Automatic chain selection across Base, Polygon, Solana, and Ethereum based on fees, latency, and reliability |
| **Merchant Adapters** | Pluggable adapter pattern for RideCo, BrewHaus Coffee, InvoiceCo, and custom merchants |
| **Session Delegation** | Temporary auto-approval windows with merchant scope, spend limits, and expiry |
| **Transaction Simulation** | Pre-flight checks for balance sufficiency, gas estimation, and recipient validation |
| **Audit Trail** | Every policy evaluation, payment execution, and receipt generation is logged |
| **Conversational UI** | Dark-themed chat interface with color-coded message types and approval prompts |

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                     Chat Interface (Next.js)                │
│              User sends natural-language request            │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                  Layer 1: Intent Parser                     │
│        Rule-based NLP → ParsedIntent                        │
│   Intents: ride, coffee, invoice, send, transit top-up      │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│              Layer 2: Commerce Orchestrator                 │
│   Merchant resolution → Quote retrieval → Purchase request  │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│           Layer 3: Policy Engine (3-Tier)                   │
│                                                             │
│  ┌─────────────┐  ┌───────────────┐  ┌──────────────────┐  │
│  │   Static     │  │   Session     │  │   Risk           │  │
│  │   Policy     │→ │   Policy      │→ │   Policy         │  │
│  │             │  │               │  │                  │  │
│  │ • Allowlist  │  │ • Expiry      │  │ • Velocity       │  │
│  │ • Chain      │  │ • Scope       │  │ • Anomaly        │  │
│  │ • Limits     │  │ • Txn limit   │  │ • Round numbers  │  │
│  │ • Balance    │  │ • Spend cap   │  │ • Risk score     │  │
│  └─────────────┘  └───────────────┘  └──────────────────┘  │
│                                                             │
│           Most restrictive decision wins                    │
└──────────────────────────┬──────────────────────────────────┘
                           │
              ┌────────────┼───────────────┐
              │            │               │
           ALLOW    REQUIRE_APPROVAL     DENY
              │            │               │
              ▼            ▼               ▼
         ┌─────────┐  ┌─────────┐     ┌────────┐
         │ Execute │  │ Prompt  │     │ Reject │
         │         │  │ User    │     │        │
         └────┬────┘  └────┬────┘     └────────┘
              │            │
              ▼            ▼
┌─────────────────────────────────────────────────────────────┐
│              Layer 4: Payment Execution                     │
│                                                             │
│   Chain Selection → Simulation → Mock Execution → Receipt   │
│                                                             │
│   ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│   │   Base   │  │ Polygon  │  │  Solana   │  │ Ethereum │   │
│   │ $0.01    │  │ $0.02    │  │  $0.005   │  │ $2.50    │   │
│   │ 2s       │  │ 3s       │  │  1s       │  │ 15s      │   │
│   │ 99%      │  │ 98%      │  │  97%      │  │ 99.9%    │   │
│   └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│               Layer 5: Receipt & Audit                      │
│      Receipt generation → Audit log → Balance update        │
└─────────────────────────────────────────────────────────────┘
```

---

## Tech Stack and Scope

Next.js 14 / React / TypeScript / Tailwind CSS. Intent parsing is rule-based, state is held in memory, and payments on Base, Polygon, Solana, and Ethereum are mocked.

| Component | Technology |
|:----------|:-----------|
| Framework | Next.js 14 (App Router) |
| Language | TypeScript 5 |
| UI | React 18, Tailwind CSS 3.4 |
| Token | USDC (simulated) |
| Chains | Base, Polygon, Solana, Ethereum |
| State | In-memory store (MVP) |

---

## Project Structure

```
src/
├── app/                           # Next.js App Router
│   ├── api/chat/route.ts         # POST /api/chat endpoint
│   ├── layout.tsx                # Root layout
│   ├── page.tsx                  # Home page
│   └── globals.css               # Global styles
│
├── components/                    # React UI components
│   ├── Chat.tsx                  # Main chat interface
│   ├── MessageBubble.tsx         # Message rendering with formatting
│   └── ApprovalPrompt.tsx        # Approve/Deny buttons
│
└── lib/                           # Core business logic
    ├── types.ts                  # All TypeScript interfaces
    ├── store/index.ts            # In-memory data store + seed data
    │
    ├── agent/                     # Agent orchestration
    │   ├── orchestrator.ts       # Central coordinator
    │   └── intent-parser.ts      # Natural language → ParsedIntent
    │
    ├── merchants/                 # Merchant adapters
    │   ├── types.ts              # MerchantAdapter interface
    │   ├── registry.ts           # Adapter lookup + category mapping
    │   ├── ride-adapter.ts       # RideCo (transport)
    │   ├── coffee-adapter.ts     # BrewHaus (food/beverage)
    │   └── invoice-adapter.ts    # InvoiceCo (invoices)
    │
    ├── policy/                    # 3-layer policy engine
    │   ├── engine.ts             # Composite evaluator
    │   ├── static-policy.ts      # Layer 1: Hard rules
    │   ├── session-policy.ts     # Layer 2: Delegated permissions
    │   └── risk-policy.ts        # Layer 3: Risk heuristics
    │
    ├── payment/                   # Payment execution
    │   ├── executor.ts           # Mock payment orchestration
    │   ├── chain-selector.ts     # Weighted chain scoring
    │   └── simulator.ts          # Pre-flight simulation
    │
    └── receipt/                   # Post-payment
        └── generator.ts          # Receipt creation + audit
```

## Related projects

These are independent companion repositories. The links describe related work, not implemented runtime integrations:

- [Agent Authorization Wallet + Merchant Trust Gateway](https://github.com/vaibhavkapur/Agent-Authorization-Wallet-Merchant-Trust-Gateway): purchase authorization, merchant verification, and execution evidence.
- [Agent Services Marketplace](https://github.com/vaibhavkapur/Agent-Services-Marketplace): service discovery, quotes, and agent purchase workflows.
- [Agentic Commerce Protocol Test Lab](https://github.com/vaibhavkapur/Agentic-Commerce-Protocol-Test-Lab): protocol fixtures, scenarios, and conformance checks.
- [Autonomous Price Watch Buyer](https://github.com/vaibhavkapur/Autonomous-Price-Watch-Buyer): price monitoring and bounded purchase decisions.
- [Cross-Merchant Procurement Agent](https://github.com/vaibhavkapur/Cross-Merchant-Procurement-Agent): merchant comparison and procurement planning.
