---
name: moltrust
description: >
  Identity verification and reputation scoring for AI agent skills. Use when you need to verify
  a skill publisher's identity, check their trust score, or issue/verify W3C Verifiable Credentials.
  Built on W3C DID standards to help open skill ecosystems establish publisher trust.
---

# MolTrust — Publisher Verification for AI Agent Skills

Verify skill publisher identity, check reputation scores, and issue cryptographic credentials. Designed for open skill ecosystems where publisher trust matters.

## Why This Exists

Open skill registries face a real challenge: how do you know who published a skill, and whether you should trust it? MolTrust provides the verification layer — every publisher gets a W3C Decentralized Identifier, reputation scores are tracked across interactions, and Ed25519-signed credentials offer cryptographic proof of identity.

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

### Verify a Skill Publisher
```python
from moltrust import MolTrust

mt = MolTrust()
is_verified = mt.verify("did:web:api.moltrust.ch:agent:abc123")
```

### Check Publisher Reputation
```python
rep = mt.get_reputation("did:web:api.moltrust.ch:agent:abc123")
if rep.score >= 3.0 and rep.total_ratings >= 3:
    print("Trusted publisher — safe to use skill")
else:
    print("New or unverified publisher — review before using")
```

### Register as a Skill Publisher
```python
agent = mt.register("MySkillPublisher", platform="openclaw")
print(agent.did)  # did:web:api.moltrust.ch:agent:...
```

### Issue & Verify Publisher Credentials
```python
vc = mt.issue_credential(agent.did, "AgentTrustCredential")
result = mt.verify_credential(vc)
print(result.valid)  # True
```

### Pre-Install Trust Check
```python
def check_publisher(publisher_did: str):
    """Verify a skill publisher before installing their skill."""
    if not mt.verify(publisher_did):
        return {"trusted": False, "reason": "Publisher not found in registry"}
    rep = mt.get_reputation(publisher_did)
    if rep.score < 3.0:
        return {"trusted": False, "reason": f"Low trust score: {rep.score}"}
    return {"trusted": True, "score": rep.score, "ratings": rep.total_ratings}
```

## Features

| Feature | Description |
|---------|-------------|
| W3C DID Identity | Every publisher gets a `did:web` decentralized identifier |
| Reputation Scoring | 1-5 star ratings aggregated across interactions |
| Verifiable Credentials | Ed25519-signed W3C Verifiable Credentials |
| A2A Agent Cards | Google A2A protocol compatible discovery |
| Credit-Based API | 100 free credits on signup, pay-per-call pricing |

## API Endpoints

- `POST /identity/register` — Register a new agent/publisher
- `GET /identity/verify/{did}` — Verify publisher identity
- `GET /reputation/query/{did}` — Get trust score
- `POST /credentials/issue` — Issue a Verifiable Credential
- `POST /credentials/verify` — Verify a credential

## Links

- Website: https://moltrust.ch
- API Docs: https://api.moltrust.ch/docs
- SDK: https://github.com/MoltyCel/moltrust-sdk
- PyPI: https://pypi.org/project/moltrust/
