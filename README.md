# 🚁 Drone Integrity & Mission Authentication Protocol

> **A decentralized, blockchain-backed integrity verification layer for autonomous drone fleets.**

The **Drone Integrity & Mission Authentication Protocol** is a full-stack, decentralized MERN application designed to ensure that autonomous drone fleets remain uncompromised. By anchoring firmware hashes and mission commands to the **Stellar Blockchain**, this protocol creates an immutable audit trail that prevents tampering, unauthorized firmware modifications, and rogue mission injections.

---

## 📑 Table of Contents

- [Project Overview](#-project-overview)
- [Why This Project Matters](#-why-this-project-matters)
- [System Architecture Overview](#%EF%B8%8F-system-architecture-overview)
- [Technology Stack](#-technology-stack)
- [Folder Structure](#-folder-structure)
- [How Integrity Verification Works](#-how-integrity-verification-works)
- [Belt System Roadmap](#-belt-system-roadmap)
- [Docker Setup Instructions](#-docker-setup-instructions)
- [Environment Variables Setup](#-environment-variables-setup)
- [Development Setup (Without Docker)](#-development-setup-without-docker)
- [Future Roadmap](#-future-roadmap)
- [Security Considerations](#-security-considerations)
- [Contribution Guidelines](#-contribution-guidelines)
- [License](#-license)
- [Disclaimer](#-disclaimer)

---

## 🌍 Project Overview

As autonomous drones become increasingly critical in sectors such as logistics, agriculture, and emergency response, their susceptibility to cyber attacks grows. A compromised drone can have its firmware altered or be sent unauthorized mission coordinates ("rogue missions").

This project solves this critical vulnerability by introducing a **Zero-Trust Verification Layer**. Before a drone is allowed to execute a flight plan, its firmware signature and its mission commands are hashed and verified against records immutably stored on the Stellar blockchain. If the hashes do not match the blockchain's source of truth, the drone is grounded.

---

## 🛡️ Why This Project Matters

This project focuses entirely on **cybersecurity, data integrity, and fleet management safety**. It is built to:

1. **Prevent Firmware Hijacking:** Ensure that the software running on the drone is the exact, authorized version released by the manufacturer or fleet manager.
2. **Prevent Rogue Mission Injection:** Verify that the GPS coordinates and flight paths sent to the drone originated from an authorized command center.
3. **Provide Immutable Auditability:** Maintain a cryptographically secure, permanent record of all operations for compliance and post-incident forensics.

> **Note:** This is a defensive cybersecurity protocol. It does not involve, endorse, or relate to the weaponization of drones or military technology.

---

## 🏗️ System Architecture Overview

The system is composed of five distinct layers:

1. **Drone Layer (IoT):** The physical or simulated drone hardware. It computes a hash of its own firmware upon boot and sends it to the API for verification.
2. **Frontend Layer (React):** A dashboard for fleet managers to register new firmware versions, authorize mission waypoints, and monitor the integrity status of active drones.
3. **Backend API (Node.js/Express):** The central verification engine. It receives data from the drones, hashes mission parameters via cryptographic standard libraries, and communicates with the blockchain.
4. **Blockchain Layer (Stellar):** The decentralized, immutable ledger. Firmware hashes and mission command hashes are recorded here as transactions or state data (future Smart Contracts).
5. **Database Layer (MongoDB):** A fast, structured database to store off-chain metadata (e.g., drone names, human-readable mission logs, UI configurations) without congesting the blockchain.

---

## 💻 Technology Stack

This application is built using a modern, scalable, and containerized architecture:

- **Frontend:** React.js
- **Backend:** Node.js, Express.js
- **Database:** MongoDB (via Docker)
- **Blockchain:** Stellar SDK (Horizon API & Soroban-ready scaffolding)
- **Cryptography:** CryptoJS (SHA-256)
- **Containerization:** Docker & Docker Compose

---

## 📁 Folder Structure

```text
drone-integrity-protocol/
├── docker-compose.yml       # Orchestrates MongoDB, Backend, and Frontend containers
├── README.md                # Project documentation
├── .gitignore               # Ignored files and directories
├── backend/                 # Node.js + Express API
│   ├── Dockerfile
│   ├── package.json
│   ├── .env                 # Backend environment variables
│   └── src/
│       ├── server.js        # Express entry point
│       ├── routes/          # API endpoint definitions
│       ├── controllers/     # Request handling logic
│       ├── services/
│       │   └── stellarService.js # Stellar blockchain interaction layer
│       ├── utils/
│       │   └── hash.js      # Cryptographic hashing utilities
│       └── config/
│           └── network.js   # Blockchain & DB network configuration
├── frontend/                # React Dashboard Application
│   ├── Dockerfile
│   ├── package.json
│   └── src/                 # UI Components and Logic
├── contracts/               # (Future) Soroban Smart Contracts
├── firmware/                # Simulated drone firmware binaries (.bin)
└── docs/                    # Additional documentation and roadmap
    └── belt-progress.md
```

---

## 🔄 How Integrity Verification Works

### 1. Firmware Registration (Authorized Party)

- The fleet manager uploads a new firmware binary through the React dashboard.
- The Backend API hashes the executable using SHA-256.
- The hash is submitted as a transaction to the Stellar blockchain, acting as the ultimate "Source of Truth."

### 2. Drone Boot Verification (Pre-Flight)

- Upon powering on, the drone calculates the SHA-256 hash of its local running firmware.
- The drone sends this hash to the Backend API.
- The API queries the Stellar blockchain. If the drone's hash matches the authorized hash on the blockchain, the drone is marked as **Verified**. If not, it is flagged as **Compromised**.

### 3. Mission Authentication

- The fleet manager creates a mission (e.g., a set of GPS waypoints).
- The mission data is hashed and recorded on Stellar.
- Before the drone executes the mission, it verifies that the incoming mission hash matches the authorized hash on the blockchain.

---

## 🥋 Belt System Roadmap

This project is structured around an incremental learning and feature roadmap:

- **[White Belt] Scaffolding:** Initial MERN + Docker setup, Stellar SDK integration, and basic testnet connection. _(Completed)_
- **[Yellow Belt] Core Verification:** Implementation of SHA-256 hashing, API endpoints for drone check-in, and Stellar transaction submission for firmware hashes.
- **[Orange Belt] Mission Control:** Addition of the React Dashboard to visualize drone status, manage firmware versions, and create verified missions.
- **[Green Belt] Database Integration:** Connecting MongoDB to store off-chain metadata, drone histories, and UI preferences.
- **[Blue Belt] Smart Contracts:** Migrating basic transaction logging to Soroban Smart Contracts for advanced state management.
- **[Brown/Black Belt] Advanced Security:** Implementing multisig mission authorization, ZK-proofs for privacy-preserving fleet data, and swarm-level consensus verification.

---

## 🐳 Docker Setup Instructions

The easiest way to run the entire stack (Database, Backend, and Frontend) is using Docker Compose.

1. **Ensure Docker is running** on your machine.
2. Clone the repository and navigate to the root directory:
   ```bash
   cd drone-integrity-protocol
   ```
3. Build and start the containers:
   ```bash
   docker-compose up --build
   ```

**Services will be available at:**

- **Frontend Dashboard:** `http://localhost:3000`
- **Backend API:** `http://localhost:5001` _(Mapped to 5001 to prevent conflicts)_
- **MongoDB:** `localhost:27017`

To stop the services, run:

```bash
docker-compose down
```

---

## ⚙️ Environment Variables Setup

For the backend to function correctly, ensure the `backend/.env` file is configured. An example configuration:

```env
# Backend Server Port (Internal Docker Port)
PORT=5000

# MongoDB Connection String (Resolves via Docker internal network)
MONGO_URI=mongodb://mongo:27017/drone-integrity

# Stellar Network Configuration (TESTNET for development)
STELLAR_NETWORK=testnet

# Stellar Keypair (DO NOT commit real secret keys in production)
# PUBLIC_KEY=G...
# SECRET_KEY=S...
```

---

## 💻 Development Setup (Without Docker)

If you prefer to run the services locally without Docker for debugging:

1. **Start a local MongoDB instance** on port 27017.
2. **Start the Backend:**
   ```bash
   cd backend
   npm install
   npm run dev
   ```
   _(Ensure you update the `MONGO_URI` in `.env` to `mongodb://localhost:27017/drone-integrity`)_
3. **Start the Frontend:**
   ```bash
   cd frontend
   npm install
   npm start
   ```

---

## 🚀 Future Roadmap

- **Soroban Smart Contracts:** Transition from basic Stellar transactions to robust smart contracts for complex authorization logic.
- **Multisig Authority:** Require multiple authorized personnel to sign off on high-risk missions (e.g., flight over populated areas).
- **Swarm Validation:** Allow drones in a localized "swarm" to cryptographically verify each other's integrity before initiating cooperative maneuvers.
- **Zero-Knowledge (ZK) Proofs:** Allow drones to prove they possess valid firmware or mission clearance without revealing the exact version numbers to potential interceptors.

---

## 🔒 Security Considerations

- **Private Key Management:** In production, Stellar secret keys must never be stored in plain text or `.env` files. They should be managed via hardware security modules (HSMs) or enterprise secret managers (e.g., AWS Secrets Manager, HashiCorp Vault).
- **Hashing Standards:** The protocol relies on standard, uncompromised cryptographic hashes (SHA-256).
- **Blockchain Immutability:** By anchoring data to Stellar, the history of firmware updates and missions becomes impossible to retroactively alter, guaranteeing a reliable audit trail.

---

## 🤝 Contribution Guidelines

Contributions are welcome! Please follow these steps:

1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/amazing-feature`).
3. Commit your changes (`git commit -m 'Add amazing feature'`).
4. Push to the branch (`git push origin feature/amazing-feature`).
5. Open a Pull Request.

Ensure all code follows the existing style guidelines and includes appropriate testing where applicable.

---

## 📄 License

This project is licensed under the MIT License. See the `LICENSE` file for details.

---

## ⚠️ Disclaimer

**This software is a cybersecurity integrity protocol.** It is designed exclusively to protect drone systems from unauthorized data tampering, hacking, and software manipulation. This project does not contain, support, or endorse any features related to the weaponization of drones, military surveillance, or autonomous combat systems. Its sole purpose is data assurance, auditability, and civil aviation safety.
