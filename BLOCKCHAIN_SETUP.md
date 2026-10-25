# ALETHEIA — Decentralized Media Authenticity Verification Layer
### SIH26204 (AICTE — Student Innovation)

This document covers **only the new blockchain layer**. Everything about
ALETHEIA's existing AI/forensic detection (SightEngine + Gemini, image
upload/URL analysis, the result UI) is unchanged — see the rest of the
repo/`documentation.html` for that.

---

## 1. What this layer does (and doesn't do)

- **AI/Forensics (existing, unchanged):** determines whether media is
  `real`, `ai`, or `uncertain`, with confidence + indicators.
- **Blockchain/DLT (new):** creates a tamper-evident, independently
  verifiable **record** of that already-computed verdict — keyed by the
  media's SHA-256 fingerprint. It never decides real-vs-fake itself, and
  no image/video/audio bytes are ever stored on-chain.

```
Image / Video / Audio / URL
        ↓
Existing ALETHEIA AI/Forensic Analysis   (unchanged)
        ↓
Verdict + Confidence + SHA-256 fingerprint
        ↓
Optional: "Register on Blockchain"        (new, user-initiated)
        ↓
AlethiaAuthenticityRegistry.sol on Sepolia
        ↓
Verification ID (ALT-XXXXXXXX) + QR + Certificate
```

## 2. New files

| File | Purpose |
|---|---|
| `contracts/AlethiaAuthenticityRegistry.sol` | The on-chain registry contract |
| `contracts/AlethiaAuthenticityRegistry.abi.json` | ABI used by `ethers.js` |
| `services/hashService.js` | SHA-256 (required) + optional perceptual hash |
| `services/blockchainService.js` | ethers.js integration + Demo Blockchain Mode fallback |
| `verify.html` | Public "Verify Media" page (Verification ID lookup + file re-check) |
| `.env.example` | Placeholder-only env template |

New routes added to the existing `server.js` (same backend, same `.env`):

```
GET  /api/blockchain/status              → subsystem status (configured / demo mode)
GET  /api/blockchain/status/:hash        → is this fingerprint already registered?
POST /api/blockchain/register            → create a new on-chain record (user-initiated)
POST /api/blockchain/verify              → hash an uploaded file and check/compare it
GET  /api/blockchain/record/:id          → look up a record by Verification ID
```

## 3. Demo Blockchain Mode (default, zero setup)

If `BLOCKCHAIN_RPC_URL`/`ALCHEMY_API_KEY`, `BLOCKCHAIN_PRIVATE_KEY`, or
`BLOCKCHAIN_CONTRACT_ADDRESS` are missing, the backend automatically runs
in **Demo Blockchain Mode**: registrations are kept in an in-memory store
on the server so the full flow (register → verify → certificate → QR)
works end-to-end for demos, without needing real Sepolia ETH.

Every response in this mode includes `"demoMode": true`, and the UI shows
a **DEMO MODE** tag — it is never presented as a real transaction. This
also doubles as the graceful-degradation path if the real testnet is
temporarily unreachable: existing AI detection keeps working regardless.

## 4. Going live on Sepolia testnet (optional, for the real demo)

1. **Get a Sepolia RPC endpoint (Alchemy, free tier):**
   - Create an app at [alchemy.com](https://www.alchemy.com/) on the
     "Ethereum Sepolia" network.
   - Copy the API key into `ALCHEMY_API_KEY` in `.env` (or paste the full
     HTTPS URL into `BLOCKCHAIN_RPC_URL` directly).

2. **Create a dedicated test wallet:**
   - Use MetaMask, create a **new** wallet used only for this project.
   - Export its private key and put it in `BLOCKCHAIN_PRIVATE_KEY`.
   - Never reuse a wallet that holds real funds.

3. **Fund it with free Sepolia test ETH:**
   - Use any public Sepolia faucet (e.g. Alchemy's own faucet, or
     `sepoliafaucet.com`) to send a small amount of test ETH to that
     wallet address. This only pays gas — no real money is involved.

4. **Deploy the contract with Remix:**
   - Open [remix.ethereum.org](https://remix.ethereum.org/).
   - Create a new file and paste in `contracts/AlethiaAuthenticityRegistry.sol`.
   - Compile with Solidity `0.8.19+`.
   - Under "Deploy & Run Transactions", set environment to
     **Injected Provider – MetaMask**, make sure MetaMask is on
     **Sepolia**, and deploy.
   - Copy the deployed contract address into `BLOCKCHAIN_CONTRACT_ADDRESS`.

5. **Set the network and restart the backend:**
   ```
   BLOCKCHAIN_NETWORK=sepolia
   ```
   Restart `node server.js` (or your Render service). The startup log
   will print `Blockchain: ✓ configured (sepolia)` once everything is
   wired up correctly.

## 5. Verifying media

- **From the result page**, right after analysis, ALETHEIA automatically
  checks "Not Registered" vs "Registered" for the exact fingerprint —
  this never implies the media is fake, only that no record exists yet.
- **From `verify.html`**, anyone can:
  - Look up a Verification ID (`ALT-XXXXXXXX`) from a certificate, or
  - Upload a file to see if its exact SHA-256 fingerprint is registered,
    optionally against one specific Verification ID (an integrity check —
    a mismatch is reported as "Integrity Verification Failed", never as
    "deepfake detected").

## 6. Database vs. blockchain

- The existing backend/response already carries the full forensic report
  (indicators, summary, technical details) — that stays off-chain.
- The blockchain only stores: media hash, verdict, confidence, media
  type, model version, an optional hash of the full report, timestamp,
  and the Verification ID. No media files, ever.

## 7. Known pre-existing item (unrelated to this layer)

`server.js` reads `SIGHTENGINE_API_USER` / `SIGHTENGINE_API_SECRET` from
the environment, but `.env` defines `SIGHTENGINE_USER` / `SIGHTENGINE_SECRET`.
This predates the blockchain work and was left untouched per the
"preserve existing behavior" requirement — flagging it here in case you
want to rename the `.env` keys (or the code) to match.
