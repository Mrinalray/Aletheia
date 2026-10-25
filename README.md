
<div align="center">

<br/>

# ALETHEIA

### `TRUTH IS IN THE PIXELS`

**AI Media Forensics · Deepfake Detection · Blockchain Integrity**

Aletheia is a multimodal media-analysis platform designed to help users assess whether images, videos, and audio may be AI-generated, with an optional blockchain integrity layer that makes it possible to verify whether a registered file has changed.

<br/>

[![Live Demo](https://img.shields.io/badge/🌐_Live_Demo-Visit_Aletheia-0891b2?style=for-the-badge)](https://aletheiaa.netlify.app/)
[![GitHub](https://img.shields.io/badge/GitHub-Source_Code-181717?style=for-the-badge&logo=github)](https://github.com/Mrinalray/Aletheia_blockchain)

<br/>

![Aletheia Homepage](https://raw.githubusercontent.com/Mrinalray/aletheia/main/assets/mainpage.png)

**Detect · Analyze · Verify**

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Why Aletheia?](#-why-aletheia)
- [Core Features](#-core-features)
- [How It Works](#-how-it-works)
- [Blockchain Integrity Layer](#-blockchain-integrity-layer)
- [AI Analysis Pipeline](#-ai-analysis-pipeline)
- [Technology Stack](#-technology-stack)
- [Screenshots](#-screenshots)
- [Getting Started](#-getting-started)
- [Environment Variables](#-environment-variables)
- [Project Structure](#-project-structure)
- [Security and Responsible Use](#-security-and-responsible-use)
- [Limitations](#-limitations)
- [Roadmap](#-roadmap)
- [Contributors](#-contributors)
- [Acknowledgements](#-acknowledgements)

---

## 🧠 Overview

The name **Aletheia** comes from the ancient Greek concept of *truth* or *disclosure*. As synthetic images, manipulated videos, and cloned voices become increasingly convincing, it is becoming harder to assess the authenticity of digital media.

**Aletheia combines AI-assisted media forensics with blockchain-based file-integrity verification.** It is designed to provide an analysis result with supporting forensic details and, when requested, a tamper-evident on-chain reference for a file.

The platform brings together:

- 🔬 **Sightengine** for AI-generated media analysis.
- 🤖 **Google Gemini** for multimodal reasoning and additional forensic observations.
- ⛓️ **Blockchain** for recording and verifying a file's cryptographic fingerprint.
- 🔐 **SHA-256** for generating a fingerprint from the exact file being registered.
- 🖥️ **A web interface** for uploading media, viewing results, and checking integrity records.

> **Important distinction:** AI analysis estimates whether media may be synthetic. Blockchain does not detect deepfakes; it helps verify whether a file matches the fingerprint registered on-chain.

## 🎯 Why Aletheia?

- Help users investigate potentially AI-generated or manipulated media.
- Combine multiple AI analysis sources rather than relying on a single signal.
- Make analysis results easier to understand through a verdict and supporting details.
- Provide a way to verify the integrity of a previously registered file.
- Connect AI-assisted media forensics with a transparent, independently verifiable blockchain record.

## ✨ Core Features

| Feature | Description |
|---|---|
| 🖼️ Image analysis | Submit an image for AI-generated media assessment. |
| 🎬 Video analysis | Analyze supported video inputs using the application's media-analysis pipeline. |
| 🎧 Audio analysis | Submit supported audio inputs for synthetic-audio assessment. |
| 🔗 URL analysis | Analyze supported media URLs where accessible by the backend and provider APIs. |
| 🤖 Multi-model analysis | Combine Sightengine results with Gemini-generated forensic observations. |
| 📊 Verdict and confidence | Present an AI/real/uncertain-style result and associated analysis details, depending on the available model output. |
| 🧪 Forensic observations | Surface visual or media characteristics identified during analysis. |
| 🔐 SHA-256 fingerprint | Generate a cryptographic fingerprint for a file. |
| ⛓️ Blockchain registration | Register a file fingerprint through the configured authenticity-registry smart contract. |
| ✅ Integrity verification | Recalculate a file's fingerprint and compare it with the registered record. |
| 🕓 Analysis history | Revisit prior analysis results where history is enabled in the application. |
| 🧭 Accessible workflow | A clear web interface for submitting media and reviewing results. |

## 🔄 How It Works

1. **Submit media**  
   Upload a supported image, video, or audio file, or provide a supported URL.

2. **Backend processing**  
   The frontend sends the request to the Node.js backend, which validates and processes the input.

3. **AI-assisted analysis**  
   The backend calls the configured providers, including Sightengine and Google Gemini, and collects their results.

4. **Result synthesis**  
   The application combines the available provider outputs according to its configured result-merging logic and presents a verdict with supporting details.

5. **Optional integrity registration**  
   Generate a SHA-256 fingerprint of the exact file and submit it to the configured blockchain registry when registration is requested.

6. **Later verification**  
   Hash the file again and compare the result with the registered fingerprint. A matching fingerprint indicates that the checked file's bytes match the registered file's bytes; a mismatch indicates that the files differ.

```text
Media file / supported URL
          │
          ▼
   Frontend interface
          │
          ▼
     Node.js backend
          │
          ├────────► Sightengine analysis
          │
          └────────► Google Gemini analysis
                          │
                          ▼
                  Combined AI result
                          │
                ┌─────────┴─────────┐
                ▼                   ▼
          Show verdict       Optional integrity flow
                                    │
                                    ▼
                              SHA-256 hash
                                    │
                                    ▼
                         Authenticity Registry
                                    │
                                    ▼
                         Verify hash on-chain
```

## ⛓️ Blockchain Integrity Layer

Aletheia's blockchain extension adds a verifiable integrity record to the AI media-analysis workflow.

### What it does

- **Creates a SHA-256 fingerprint:** a fixed-length digest derived from the exact file bytes.
- **Registers the fingerprint:** sends the fingerprint to the configured authenticity-registry smart contract.
- **Provides a persistent reference:** the transaction and registry record can be used to look up the registered fingerprint.
- **Checks file integrity:** a later fingerprint can be compared with the registered one to determine whether the file has changed.
- **Supports independent verification:** a verifier can inspect the blockchain transaction or registry record using the configured network and contract details.

### What it does not do

- It does **not** determine whether a file was generated by AI.
- It does **not** prove that the original upload was authentic or truthful.
- It does **not** guarantee that a registered file's content is accurate.
- It does **not** mean that the entire media file is stored on-chain. The integrity workflow records a fingerprint/reference, not necessarily the media itself.

### Typical verification flow

1. Select the file to register.
2. Generate its SHA-256 fingerprint.
3. Submit the fingerprint to the configured registry contract.
4. Keep the transaction hash and registration details.
5. To verify later, hash the candidate file again.
6. Compare the new fingerprint with the on-chain record.

**Network:** Sepolia testnet is used in the current project configuration, according to the project setup. Testnet records are for development and demonstration, not production guarantees.

**Smart contract:** `AletheiaAuthenticityRegistry`  
**Contract address:** Add the deployed Sepolia contract address here before publishing it in project documentation.  
**Explorer:** Add the matching Sepolia Etherscan contract or transaction URL when available.

> Never publish private keys, seed phrases, provider secrets, or wallet credentials in this repository. Keep secrets in backend environment variables.

## 🤖 AI Analysis Pipeline

Aletheia uses a multi-provider analysis approach:

### Sightengine

Used for AI-generated media analysis through the configured API/model. The exact available signals depend on the provider response and supported media type.

### Google Gemini

Used for additional multimodal reasoning and forensic observations, where supported by the selected model and input.

### Result synthesis

The backend's configured result-merging logic synthesizes the available provider results into the response shown by the frontend. Provider output can vary, and the result should be treated as an assessment rather than absolute proof.

> **Accuracy note:** No AI detector is perfectly reliable. Results may be affected by compression, resizing, editing, screenshots, unseen generators, and changes in detection models. Do not use a single result as the sole basis for consequential decisions.

## 🛠️ Technology Stack

| Layer | Technologies |
|---|---|
| Frontend | HTML5, CSS3, JavaScript |
| Backend | Node.js, Express.js |
| File handling and HTTP | Multer, Axios, FormData |
| AI services | Sightengine, Google Gemini API |
| Blockchain | Solidity smart contract, Ethereum-compatible Sepolia testnet |
| File integrity | SHA-256 |
| Configuration and middleware | dotenv, CORS, express-rate-limit |
| Deployment | Netlify (frontend), Render (backend, if configured) |

*The exact dependencies and deployment settings should be confirmed against the current repository before release.*

## 📸 Screenshots

### Main Dashboard

![Aletheia main page](https://raw.githubusercontent.com/Mrinalray/aletheia/main/assets/mainpage.png)

### Upload Interface

![Aletheia upload interface](https://raw.githubusercontent.com/Mrinalray/aletheia/main/assets/upload-section.png)

### Analysis Result

![Aletheia result panel](https://raw.githubusercontent.com/Mrinalray/aletheia/main/assets/result.png)

> If a screenshot does not display, confirm that the corresponding image exists in the repository's `assets/` directory and update the path if the new repository uses a different folder or filename.

## 🚀 Getting Started

Follow these steps to run the project locally. The commands below assume the backend uses Node.js and Express and that the frontend is served as static files. Adjust paths if the current repository layout differs.

### Prerequisites

- Node.js 18 or later
- npm 9 or later
- Sightengine API credentials
- Google Gemini API key
- A Sepolia-compatible wallet and deployed registry contract, if you want to use blockchain features
- Git

### 1. Clone the repository

```bash
git clone https://github.com/Mrinalray/Aletheia_blockchain.git
cd Aletheia_blockchain
```

### 2. Install dependencies

If the repository has a `package.json` in the backend/root directory:

```bash
npm install
```

If dependencies are not listed in `package.json`, install the packages required by the current backend implementation:

```bash
npm install express cors multer axios dotenv @google/generative-ai form-data express-rate-limit
```

Install any additional blockchain dependencies specified by the smart-contract or frontend package files. For example, a project using Hardhat or ethers.js will need its configured development dependencies.

### 3. Configure environment variables

Create a local `.env` file using the repository's example, if available:

```bash
cp .env.example .env
```

If `.env.example` is not present, create `.env` manually and add the variables required by your implementation. See [Environment Variables](#-environment-variables).

### 4. Start the backend

Use the script defined in `package.json`, if one exists:

```bash
npm start
```

Alternatively, if the backend entry point is `server.js`:

```bash
node server.js
```

### 5. Serve the frontend

For a static frontend, you can use:

```bash
npx serve .
```

Open the local URL printed by the server. Ensure the frontend's backend/API URL points to your local backend rather than the production deployment.

### 6. Configure blockchain integration

If using the blockchain feature locally:

1. Configure the intended Sepolia RPC endpoint and contract address.
2. Configure the signer/wallet through the project's supported secure method.
3. Ensure the contract has been deployed to the selected network.
4. Use testnet funds only when a transaction requires gas.
5. Confirm that the transaction and verification paths target the same network and contract.

The exact commands for compiling or deploying the contract depend on the smart-contract tooling included in the repository.

## 🔑 Environment Variables

Use only the variables required by your implementation. A possible backend configuration is shown below; rename or add variables to match the actual code.

```env
# AI provider credentials
SIGHTENGINE_API_USER=your_sightengine_user
SIGHTENGINE_API_SECRET=your_sightengine_secret
GEMINI_API_KEY=your_gemini_api_key

# Backend
PORT=3001

# Blockchain — example names; match these to your code
SEPOLIA_RPC_URL=your_sepolia_rpc_url
CONTRACT_ADDRESS=your_deployed_contract_address
```

**Security checklist**

- Keep `.env` out of Git.
- Commit `.env.example` with placeholder values only.
- Never put API keys or private keys in frontend JavaScript.
- Do not expose a wallet private key to the browser or commit it to the repository.
- Rotate any credential that has accidentally been published.

## 🗂️ Project Structure

The structure below is illustrative. Keep it aligned with the actual folders and filenames in your repository.

```text
Aletheia_blockchain/
├── assets/
│   ├── mainpage.png
│   ├── upload-section.png
│   └── result.png
├── contracts/
│   └── AletheiaAuthenticityRegistry.sol
├── index.html
├── style.css
├── app.js
├── server.js
├── package.json
├── .env.example
├── .gitignore
└── README.md
```

Your repository may use a separate frontend/backend layout or a different smart-contract directory. Update this tree to reflect the actual codebase.

## 🔐 Security and Responsible Use

- **Protect secrets:** API credentials, RPC keys, and private keys must not be committed.
- **Validate uploads:** enforce file type, file size, and request limits on the backend.
- **Treat media carefully:** uploads may contain personal or sensitive information; avoid retaining or sharing files unnecessarily.
- **Explain uncertainty:** display model outputs as estimates and avoid presenting them as conclusive proof.
- **Verify the right network:** always confirm the chain ID and contract address before interacting with a contract.
- **Handle transactions safely:** explain when a blockchain transaction will be submitted and whether it may incur gas fees.
- **Avoid sensitive on-chain data:** public blockchain records are generally persistent. Register fingerprints or references, not private media or personal information.

## ⚠️ Limitations

- AI detection may produce false positives and false negatives.
- Supported formats, file-size limits, and URL access depend on backend implementation and provider APIs.
- Some URLs may be inaccessible because of authentication, network restrictions, or provider limitations.
- Blockchain verification confirms a fingerprint match, not the truthfulness, provenance, or safety of the content.
- A matching hash proves byte-level consistency with the registered file; it does not establish who created the file unless identity is separately authenticated.
- Testnet deployments are intended for testing and may not provide production-grade availability.

## 🧭 Roadmap

Potential future improvements:

- [ ] Add a dedicated blockchain verification page with transaction details.
- [ ] Display network, contract address, transaction hash, and verification status in the UI.
- [ ] Add downloadable forensic reports with model/provider details and timestamps.
- [ ] Improve input validation, rate limiting, and error handling.
- [ ] Expand test coverage for AI analysis and smart-contract workflows.
- [ ] Add clearer explanations of confidence, uncertainty, and detector limitations.
- [ ] Document contract deployment and verification steps for reproducible setup.

## 👥 Contributors

Built with 💙 by **TeamX**.

| Contributor | GitHub |
|---|---|
| Mrinal Roy | [@Mrinalray](https://github.com/Mrinalray) |
| Swastika Shaw | [@Swastika01-suu](https://github.com/Swastika01-suu) |
| Rahul Sah | [@real-Rahul1](https://github.com/real-Rahul1) |

## 🙏 Acknowledgements

- [Google Gemini](https://ai.google.dev/)
- [Sightengine](https://sightengine.com/)
- [Ethereum](https://ethereum.org/)
- [Sepolia testnet](https://sepolia.dev/)
- The open-source community

---

<div align="center">

**Aletheia — Because truth matters.**

`DETECT · ANALYZE · VERIFY`

[Live Demo](https://aletheiaa.netlify.app/) · [GitHub Repository](https://github.com/Mrinalray/Aletheia_blockchain)

</div>
