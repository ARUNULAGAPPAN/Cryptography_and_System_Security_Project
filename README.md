# Cryptography_and_System_Security_Project

# Hybrid Post-Quantum Secure F1AP Framework for O-RAN (5G)

## Overview

This project implements a hybrid Post-Quantum Cryptographic (PQC) framework to secure the F1 Application Protocol (F1AP) interface in disaggregated 5G O-RAN architectures.

The system integrates ML-KEM (Kyber) and ML-DSA (Dilithium) with classical cryptography (X25519) to build a quantum-resilient IPsec tunnel between the Central Unit (CU) and Distributed Unit (DU).

Designed to mitigate Harvest-Now-Decrypt-Later (HNDL) threats in next-generation telecom networks.

---

## Key Features

- Hybrid Key Exchange  
  - X25519 + ML-KEM-768  

- Quantum-Safe Authentication  
  - ML-DSA (Dilithium) certificates  

- Fully Containerized Architecture  
  - Docker-based CU–DU deployment  

- Secure F1AP Signaling  
  - IPsec (IKEv2 + ESP AES-256-GCM)  

- Performance Evaluation  
  - Latency, throughput, packet expansion  

- Reproducible Testbed  
  - OpenAirInterface + strongSwan + liboqs  

---

## System Architecture
<img width="500" height="424" alt="image" src="https://github.com/user-attachments/assets/4a442ea4-3b86-437c-a645-171a15101581" />



- Communication occurs over a Docker bridge network  
- F1AP messages are encapsulated in SCTP over IPsec  
- Hybrid cryptography ensures backward compatibility and post-quantum security  

---

## Tech Stack

| Layer            | Technology               |
|------------------|------------------------|
| RAN Stack        | OpenAirInterface (OAI) |
| VPN / Security   | strongSwan (IKEv2, ESP) |
| PQC Library      | liboqs                |
| Crypto Backend   | OpenSSL 3 + OQS Provider |
| Containerization | Docker, Docker Compose |
| Protocol         | SCTP (F1AP)           |

---

## Getting Started

### Prerequisites

- Docker and Docker Compose  
- Linux (Ubuntu 22.04 recommended)  
- Minimum 8GB RAM  

---

### Run the Project

```bash
git clone https://github.com/your-repo/pqc-f1ap.git
cd pqc-f1ap
docker-compose up -d
Establish Secure Tunnel
docker exec -d oai-cu-pqc /usr/libexec/ipsec/charon
docker exec -d oai-du-pqc /usr/libexec/ipsec/charon

docker exec -it oai-cu-pqc swanctl --load-all
docker exec -it oai-du-pqc swanctl --load-all

docker exec -it oai-cu-pqc swanctl --initiate --child f1-c
Verify Connection
docker exec -it oai-cu-pqc swanctl --list-sas

Expected output:

IKE_SA f1-pqc
CHILD_SA f1-c
Test Communication
docker exec -it oai-cu-pqc ping 192.168.10.20
Results
Metric	Classical	PQC Hybrid
Handshake Latency	42 ms	54 ms
Overhead	—	+21%
Security	Quantum-vulnerable	Quantum-safe
Minimal latency increase
No packet loss
Stable SCTP throughput
Packet Analysis (Optional)
sudo tcpdump -i br-<bridge-id> -w capture.pcap
wireshark capture.pcap

Apply filter:

icmp
Security Highlights
Resistant to Shor’s Algorithm
Protects against HNDL attacks
Secure against:
Man-in-the-Middle (MITM) attacks
Replay attacks
Passive eavesdropping
Project Structure
.
├── docker-compose.yml
├── Dockerfile.pqc
├── configs/
│   ├── swanctl.conf
│   ├── ipsec.conf
│   └── certificates/
├── scripts/
│   └── setup.sh
├── results/
│   └── performance-analysis
└── README.md
Research Contribution

This project demonstrates:

Practical deployment of PQC in 5G O-RAN
Feasibility of hybrid cryptographic migration
Real-world mitigation of quantum-era threats
Future Work
Extend PQC to F1-U (user plane)
FPGA acceleration for NTT operations
Multi-DU scalable O-RAN deployment
Integration with 6G architectures
Contributors
Arun Ulagappan S
Team Members
License

This project is intended for academic and research purposes.

Acknowledgment

Inspired by NIST PQC standards and O-RAN Alliance architecture.

Final Note

This repository bridges the gap between theoretical PQC research and practical telecom deployment, enabling quantum-resilient 5G infrastructure.
