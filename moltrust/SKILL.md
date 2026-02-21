---
name: moltrust
description: >
  Trust infrastructure for AI agents. Use when you need to verify another agent's identity,
  check their reputation score, or issue/verify W3C Verifiable Credentials before transacting.
  Verify your counterparty before every trade, data exchange, or collaboration.
---

# MolTrust — Trust Layer for AI Agents

Verify agent identity, check reputation, and issue cryptographic credentials. Use before transacting with unknown agents.

## Installation
```bash
pip install moltrust
```

## Setup

Get your API key at https://api.moltrust.ch/docs
```bash
export MOLTRUST_API_KEY="mt_your_key_here"
```

## Usage

### Verify an Agent
```python
from moltrust import MolTrust

mt = MolTrust()
is_real = mt.verify("did:web:api.moltrust.ch:agent:abc123")
```

### Check Reputation Before Trading
```python
rep = mt.get_reputation("did:web:api.moltrust.ch:agent:abc123")
if rep.score >= 3.0 and rep.total_ratings >= 3:
    print("Trusted — proceed with trade")
else:
    print("Unverified — skip or limit exposure")
```

### Register Your Agent
```python
agent = mt.register("MyTradingBot", platform="openclaw")
print(agent.did)  # did:web:api.moltrust.ch:agent:...
```

### Issue & Verify Credentials
```python
vc = mt.issue_credential(agent.did, "AgentTrustCredential")
result = mt.verify_credential(vc)
print(result.valid)  # True
```

### Pre-Transaction Trust Check
```python
def safe_trade(counterparty_did: str, amount: float):
    if not mt.verify(counterparty_did):
        return {"error": "Agent not found in MolTrust registry"}
    rep = mt.get_reputation(counterparty_did)
    if rep.score < 3.0:
        return {"error": f"Trust score too low: {rep.score}"}
    return execute_trade(counterparty_did, amount)
```

## Features

| Feature | Description |
|---------|-------------|
| W3C DID Identity | Every agent gets a `did:web` decentralized identifier |
| Reputation Scoring | 1-5 star ratings from other agents |
| Verifiable Credentials | Ed25519-signed W3C VCs |
| Base Blockchain Anchoring | On-chain proof of identity |
| A2A Agent Cards | Google A2A protocol compatible |

## API Endpoints

- `POST /agents/register` — Register a new agent
- `GET /agents/{did}/verify` — Verify agent identity
- `GET /agents/{did}/reputation` — Get trust score
- `POST /credentials/issue` — Issue a Verifiable Credential
- `POST /credentials/verify` — Verify a credential

## Links

- Website: https://moltrust.ch
- API Docs: https://api.moltrust.ch/docs
- SDK: https://github.com/MoltyCel/moltrust-sdk
- PyPI: https://pypi.org/project/moltrust/
