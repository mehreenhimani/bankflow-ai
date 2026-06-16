# BankFlow AI

An agentic banking platform that demonstrates production-grade AI product architecture for European SME neobanks. BankFlow AI uses Claude's tool-use capability to create a genuinely autonomous banking agent with guardrails, synthetic data, and LLM-as-judge evaluation.

**Live Demo:** [bankflow-ai.vercel.app](https://bankflow-ai-app-deve-ox70.bolt.host/)

## What Makes This Different

This is not a chat wrapper around an LLM prompt. BankFlow AI implements a full agentic architecture:

- **Tool-Use Agent** - Claude autonomously decides which banking tools to call (account lookup, credit scoring, AML flagging, dispute creation) based on user intent. Multi-round tool orchestration with max 3-iteration loops.
- **Synthetic Dataset** - 5 SME customers, 5 accounts, 14 transactions with realistic AML triggers (Cayman Islands entities, Dubai offshore transfers, Turkey wires), all queryable through the agent's tool interface.
- **Guardrail Pipeline** - PII detection (IBAN, email, phone, credit card regex patterns) on input, hallucination detection (4 heuristic rules) on output, confidence scoring (0-100) on every response.
- **LLM-as-Judge Evaluation** - 10 golden test cases across 7 categories. Each test runs the full agent pipeline, then a separate Claude call judges the response on Accuracy, Compliance, Tone, and Completeness (1-5 each).
- **Audit Trail** - Every agent response includes expandable metadata: tools called, input/output per tool, PII scan result, hallucination check, confidence breakdown.

## Modules

| Module | What It Does | What's Live |
|--------|-------------|-------------|
| **Dashboard** | Platform overview with customer portfolio, AML-flagged transactions, synthetic data stats | Real synthetic dataset with persistent customer/transaction data |
| **Savings Engine** | Compound interest calculator + ALM impact simulator | Real financial formulas, ECB base rate (2.25%), NIM/duration gap/IR sensitivity |
| **ConvoBank** | Agentic chat with tool-use, routing tags, guardrails | Live Claude API calls, real tool orchestration, PII detection, audit trail |
| **Credit Engine** | SME credit scoring with explainable factors | Weighted 5-factor algorithm, EU AI Act Art.13 transparency, PD/limit/rate output |
| **Eval Suite** | LLM-as-judge with golden dataset | Live agent + judge pipeline, 4-dimension scoring, tool-match verification |

## Architecture

```
User Input
    |
    v
[PII Guardrail] --> Block if IBAN/Card detected
    |
    v
[Claude API + Tools] <---> [Tool Executor]
    |                           |
    |                    lookup_customer
    |                    check_account
    |                    get_transactions
    |                    run_credit_assessment
    |                    flag_aml_review
    |                    calculate_savings
    |                    create_dispute
    |                    get_product_info
    |
    v (max 3 tool rounds)
[Final Response]
    |
    v
[Hallucination Check] --> Flag fabricated data
    |
    v
[Confidence Scoring] --> 0-100 based on tool backing + routing + hallucination
    |
    v
[Response + Audit Trail + Routing Tag]
```

## Regulatory Compliance

- **EU AI Act Art.13** - All credit decisions include explainable factor breakdown
- **PSD2** - Dispute handling follows 24h acknowledge / 48h resolve timelines
- **AML/CFT** - Transactions >10K EUR to high-risk jurisdictions auto-flagged
- **DORA** - Audit trail on every agent action for operational resilience
- **PII Protection** - Input scanning for IBAN, credit card, phone, email patterns

## Tech Stack

- [React 18](https://react.dev/) - UI framework
- [Recharts](https://recharts.org/) - Data visualization
- [Lucide React](https://lucide.dev/) - Icon library
- [Claude API](https://docs.anthropic.com/) - Agentic AI with tool-use
- [Vite](https://vitejs.dev/) - Build tool (for bolt.new deployment)

## Getting Started

### Prerequisites

- Node.js 18+
- Anthropic API key ([console.anthropic.com](https://console.anthropic.com))

### Setup

1. Clone the repository:

```bash
git clone https://github.com/mehreenhimani/bankflow-ai.git
cd bankflow-ai
```

2. Install dependencies:

```bash
npm install
```

3. Create environment file:

```bash
cp .env.example .env
```

Edit `.env`:
```
VITE_ANTHROPIC_API_KEY=your-anthropic-api-key
```

4. Start development server:

```bash
npm run dev
```

Open http://localhost:5173 in your browser.

### Build for production

```bash
npm run build
```

## Golden Dataset (Eval Suite)

| ID | Category | Tests |
|----|----------|-------|
| G-01 | Tool Use | Account balance lookup via tool chain |
| G-02 | AML | Flagged transaction detection and escalation |
| G-03 | Credit | AI scoring with explainable factors |
| G-04 | Dispute | PSD2-compliant dispute creation |
| G-05 | Savings | Interest projection calculation |
| G-06 | Hallucination | Internal document fabrication prevention |
| G-07 | Hallucination | Regulatory data fabrication prevention |
| G-08 | Multi-Tool | 4-tool chain orchestration |
| G-09 | Compliance | High-risk cross-border blocking |
| G-10 | PII Guard | IBAN detection in user input |

## Synthetic Dataset

- **5 SME customers** across DE, NL, AT (Technology, F&B, Manufacturing, Services, Healthcare)
- **5 accounts** with realistic IBANs and product holdings
- **14 transactions** including 3 AML-flagged (Cayman Islands, Dubai, Turkey)
- **4 banking products** with current ECB-referenced rates

## Project Structure

```
src/
  bankflow-ai.jsx    # Main application (single-file React app)
  
Architecture:
  Synthetic DB       # Customer, account, transaction, product data
  Financial Engine   # Compound interest, ALM, credit scoring algorithms
  Guardrails         # PII detection, hallucination check, confidence scoring
  Agent Tools        # 8 tool definitions + executor
  Agent Orchestrator # Multi-round tool-use loop with Claude API
  Golden Dataset     # 10 evaluation test cases
  LLM Judge          # Structured 4-dimension scoring
```

## Author

**Mehreen Himani** - Senior AI Product Manager

- 13+ years in regulated financial services (UBS, Standard Chartered, Credit Suisse, Capgemini)
- Specialized in AI/ML product management, AML compliance, post-trade settlements
- CSPO | AI Product Manager certified

Portfolio: [mehreens-ai-story.lovable.app](https://mehreens-ai-story.lovable.app)

## License

MIT
