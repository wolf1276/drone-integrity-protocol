# Drone Integrity Protocol

A full-stack MERN application for drone integrity verification using Stellar blockchain.

## Tech Stack

- **Frontend:** React
- **Backend:** Node.js + Express
- **Database:** MongoDB (via Docker)
- **Blockchain:** Stellar SDK
- **Containerization:** Docker + Docker Compose

## Project Structure

```
drone-integrity-protocol/
├── docker-compose.yml
├── README.md
├── .gitignore
├── backend/
│   ├── Dockerfile
│   ├── package.json
│   ├── .env
│   └── src/
│       ├── server.js
│       ├── routes/
│       ├── controllers/
│       ├── services/
│       │   └── stellarService.js
│       ├── utils/
│       │   └── hash.js
│       └── config/
│           └── network.js
├── frontend/
│   ├── Dockerfile
│   ├── package.json
│   └── src/
├── contracts/
├── firmware/
│   └── firmware.bin
└── docs/
    └── belt-progress.md
```

## Getting Started

### Prerequisites

- Docker & Docker Compose

### Run the Project

```bash
docker-compose up --build
```

- **Frontend:** http://localhost:3000
- **Backend API:** http://localhost:5000
- **MongoDB:** localhost:27017

### Stop the Project

```bash
docker-compose down
```

## License

MIT
