# Agentic Commerce + Stablecoin Checkout

A conversational commerce demonstration that turns supported natural-language requests into policy-controlled, simulated USDC payments. Merchant adapters, three policy layers, chain selection, and receipts run behind a chat interface.

> **[Read the full documentation](docs/index.md)**

Next.js 14 / React / TypeScript / Tailwind CSS. Intent parsing is rule-based, state is held in memory, and payments on Base, Polygon, Solana, and Ethereum are mocked.

## Getting Started

See the [Getting Started guide](docs/getting-started.md) for prerequisites and configuration.

```bash
# Install dependencies
npm install

# Start development server
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) and start chatting.

## Quick Example

Illustrative chat output; amounts, routing, and receipts depend on the seeded state and policy. All transactions are simulated.

```
You:   "Buy me a latte from BrewHaus"
Agent: BrewHaus quotes $4.50 for a Latte
       Policy: auto-approved (under $25 threshold)
       Chain:  Base selected ($0.01 fee, 2s latency)
       Tx:    0xabc...def confirmed
       Receipt generated
```

```
You:   "Book me a $50 ride to JFK"
Agent: RideCo quotes $47.30
       Policy: requires approval (above auto-approve limit)
       → "Approve $47.30 for RideCo ride?" [Approve] [Deny]
You:   [Approve]
Agent: Chain:  Polygon selected
       Tx:    0x123...789 confirmed
```
