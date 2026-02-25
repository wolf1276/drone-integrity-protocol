# 🚁 Drone Integrity & Mission Authentication Protocol

> **Decentralized Integrity Verification Layer for Unmanned Aerial Vehicles (UAVs)**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=flat&logo=docker&logoColor=white)](https://www.docker.com/)
[![React](https://img.shields.io/badge/react-%2320232a.svg?style=flat&logo=react&logoColor=%2361DAFB)](https://reactjs.org/)
[![NodeJS](https://img.shields.io/badge/node.js-6DA55F?style=flat&logo=node.js&logoColor=white)](https://nodejs.org/)
[![Stellar](https://img.shields.io/badge/Stellar-000000?style=flat&logo=Stellar&logoColor=white)](https://www.stellar.org/)

---

## 📑 Table of Contents

- [Project Overview](#-project-overview)
- [Why This Project Matters](#-why-this-project-matters)
- [System Architecture Overview](#-system-architecture-overview)
- [Technology Stack](#-technology-stack)
- [Folder Structure](#-folder-structure)
- [How Integrity Verification Works](#-how-integrity-verification-works)
- [Belt System Roadmap](#-belt-system-roadmap)
- [Docker Setup Instructions](#-docker-setup-instructions)
- [Environment Variables Setup](#-environment-variables-setup)
- [Development Setup](#-development-setup)
- [Future Roadmap](#-future-roadmap)
- [Security Considerations](#-security-considerations)
- [Contribution Guidelines](#-contribution-guidelines)
- [License](#-license)
- [Disclaimer](#-disclaimer)

---

## 🦅 Project Overview

The **Drone Integrity & Mission Authentication Protocol** is a decentralized, blockchain-backed security layer designed to ensure the operational integrity of drone fleets. By leveraging the Stellar blockchain, this system records immutable cryptographic hashes of both drone firmware and mission commands.

**The Problem:** Commercial and enterprise drones are increasingly vulnerable to malicious firmware modifications, rogue mission injections, and unauthorized takeovers. Traditional centralized databases can be compromised, leaving fleets exposed.

**The Solution:** This protocol establishes a zero-trust architecture. Before a drone can execute a flight, its current firmware hash and assigned mission parameters are verified against an immutable record on the blockchain. Any discrepancy instantly flags the drone as compromised, preventing unauthorized operations.

---

## 🛡️ Why This Project Matters

As UAV technology proliferates in critical sectors (delivery, surveillance, agriculture, emergency response), the attack surface expands. This project matters because it shifts drone security from reactive to proactive:

- **Cybersecurity First:** Focuses entirely on hardening the communication and software layers of UAVs.
- **Tamper-Evident:** Firmware modifications (even single-byte changes) are immediately detected.
- **Non-Repudiation:** Mission commands are immutably logged, ensuring cryptographically verifiable audit trails of who authorized what flight.
- **Supply Chain Security:** Protects the software supply chain of drones from the manufacturer to deployment.

---

## 🏗️ System Architecture Overview

Our architecture is separated into distinct, scalable layers:

1. **🌐 Frontend Layer (React):** A responsive Dashboard for fleet operators to register drones, construct missions, and monitor integrity status in real-time.
2. **⚙️ Backend API Layer (Node.js + Express):** The core logic engine that processes incoming drone data, generates cryptographic hashes, and interfaces with the blockchain.
3. **🗄️ Database Layer (MongoDB):** Stores off-chain metadata (drone names, operator profiles, rich mission descriptions) linked to on-chain hashes.
4. **⛓️ Blockchain Layer (Stellar):** The immutable ledger where the ultimate truth resides. Stores SHA-256 hashes of firmware and mission data as custom assets or memo fields.
5. **🚁 Drone Layer (Simulated/Physical):** The edge device that computes its own firmware hash on boot and signs mission requests, communicating with the Backend API.

---

## �️ Technology Stack

This project is built using a modern, production-grade stack:

- **Frontend:** React.js, Tailwind CSS _(for rapid styling)_
- **Backend:** Node.js, Express.js
- **Database:** MongoDB (via Mongoose)
- **Blockchain:** Stellar SDK (Horizon API), Soroban-ready architecture for future smart contracts.
- **Containerization:** Docker & Docker Compose
- **Cryptography:** SHA-256 hashing, Ed25519 keypairs

---

## 📁 Folder Structure

```text
drone-integrity-protocol/
├── backend/                  # Node.js API Server
│   ├── src/
│   │   ├── controllers/      # Route handlers
│   │   ├── models/           # Mongoose schemas
│   │   ├── routes/           # API endpoints
│   │   ├── services/         # Stellar blockchain interaction
│   │   └── utils/            # Hashing and crypto utilities
│   ├── Dockerfile
│   └── package.json
├── frontend/                 # React UI Web App
│   ├── public/
│   ├── src/
│   │   ├── components/       # Reusable UI components
│   │   ├── pages/            # View components (Dashboard)
│   │   └── services/         # API client
│   ├── Dockerfile
│   └── package.json
├── contracts/                # (Future) Soroban Smart Contracts
├── docs/                     # Project documentation
├── firmware/                 # Drone firmware mock files/simulators
├── docker-compose.yml        # Orchestrates the full stack
├── README.md                 # You are here
└── .gitignore
```

---

## � How Integrity Verification Works

### 1. 📝 Firmware Registration (Genesis)

1. An authorized operator compiles the drone firmware.
2. The Backend computes a SHA-256 hash of the `.bin` or `.hex` file.
3. This hash is anchored to the Stellar blockchain via a transaction signed by an admin key.

### 2. ⚡ Drone Boot Verification (Pre-flight)

1. Drone powers on and calculates its own firmware hash.
2. Drone pings the Backend API with its ID and computed hash.
3. Backend queries the Stellar ledger for the registered hash.
4. If hashes match: `Status -> VERIFIED`. If mismatch: `Status -> COMPROMISED` (Flight locked out).

### 3. 🗺️ Mission Authentication

1. Operator creates a flight path (waypoints, altitude, speed).
2. The mission payload is hashed.
3. The mission hash is recorded on the Stellar blockchain.
4. The drone receives the mission, calculates the hash, and verifies it against the blockchain record before executing movement.

---

## 🥋 Belt System Roadmap

This project follows a progressive development roadmap:

- **⚪ White Belt (Foundation):** Basic MERN architecture, Docker setup, dummy hashing.
- **🟡 Yellow Belt (Integration):** Connect backend to Stellar Testnet, register static hashes.
- **🟢 Green Belt (Dynamic Auth):** Real-time firmware verification flows, basic React dashboard.
- **🔵 Blue Belt (Mission Control):** Implement mission payload hashing and verification.
- **🟤 Brown Belt (Security Hardening):** Implement JWT auth, secure key management, comprehensive logging.
- **⚫ Black Belt (Production Ready):** Soroban smart contracts, multisig authorization, zero-knowledge proofs (ZKPs), full physical drone integration.

---

## 🐳 Docker Setup Instructions

The easiest way to run the entire stack is using Docker.

**Prerequisites:** Docker and Docker Compose installed.

```bash
# 1. Clone the repository
git clone https://github.com/wolf1276/Drone-Integrity-Mission-Authentication-Protocol.git
cd drone-integrity-protocol

# 2. Build and start the containers
docker-compose up --build

# 3. Access the services
# Frontend: http://localhost:3000
# Backend API: http://localhost:5000
# Database: mongodb://localhost:27017
```

To stop the services:

```bash
docker-compose down
```

---

## 🔐 Environment Variables Setup

Create a `.env` file in the `backend` directory:

```env
# backend/.env
PORT=5000
MONGODB_URI=mongodb://mongo:27017/drone_integrity
STELLAR_NETWORK=TESTNET
STELLAR_HORIZON_URL=https://horizon-testnet.stellar.org

# WARNING: Use a funded testnet key for development. NEVER commit real private keys.
STELLAR_ADMIN_SECRET=S_YOUR_TESTNET_SECRET_KEY_HERE
STELLAR_ADMIN_PUBLIC=G_YOUR_TESTNET_PUBLIC_KEY_HERE

JWT_SECRET=your_super_secret_jwt_key_here
```

---

## 💻 Development Setup (Without Docker)

If you prefer to run services locally:

**1. Database:** Ensure MongoDB is running locally on port `27017`.

**2. Backend Setup:**

```bash
cd backend
npm install
npm run dev
```

**3. Frontend Setup:**

```bash
cd frontend
npm install
npm start
```

---

## 🚀 Future Roadmap

Our vision extends beyond basic hashing:

- **📜 Soroban Smart Contracts:** Moving logic from the centralized Node.js backend directly to Stellar smart contracts for ultimate decentralization.
- **✍️ Multisig Authority:** Requiring `M-of-N` operator signatures to authorize high-risk missions.
- **🐝 Swarm Validation:** Drones cross-verifying each other's integrity hashes locally over mesh networks.
- **🕵️‍♂️ Zero-Knowledge Proofs (ZKPs):** Proving a drone holds valid firmware without revealing the firmware payload itself.

---

## 🔒 Security Considerations

- **Private Key Management:** The current architecture uses `.env` files for Stellar keys. In production, a secure Key Management System (AWS KMS, HashiCorp Vault) MUST be used.
- **Cryptographic Hashing:** Uses industry-standard `SHA-256`. Ensure physical drone hardware is capable of performing these calculations securely on boot.
- **Blockchain Immutability:** Once a firmware hash is registered on Stellar, it cannot be deleted. Rollbacks require registering a new authoritative hash.
- **Network Vectors:** The communication link between the drone and the API must be secured via mutual TLS (mTLS).

---

## 🤝 Contribution Guidelines

We welcome contributions from the cybersecurity, blockchain, and drone developer communities!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

Please ensure all tests pass and your code adheres to the existing styling.

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.

---

## ⚠️ Disclaimer

**CRITICAL NOTICE:** This project is a **Cybersecurity and Integrity Protocol**. It is explicitly designed for defensive purposes—specifically to secure UAV communications, verify software integrity, and prevent unauthorized tampering or hijacking. **This is NOT a weapon system, nor is it designed to be integrated into or facilitate the operation of autonomous weapon platforms.** The creators disclaim any liability for the misuse of this protocol in offensive operations.
