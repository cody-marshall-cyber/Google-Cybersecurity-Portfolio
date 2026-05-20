# Secure Edge-AI Compute Node (SECN)
### System Architecture & Design Specification
**Author:** Cody MArshall   
**Status:** Lifecycle Phase: Requirements & Design  

---

## 1. Project Abstract & Business Logic

### 1.1 The Problem Statement
Modern enterprises and developers heavily rely on cloud-based Large Language Models (LLMs) for automation, data analysis, and coding assistance. However, utilizing cloud-hosted APIs introduces three critical architectural flaws:
1. **Data Privacy & Compliance Risks**: Sensitive corporate data, proprietary source code, or personally identifiable information (PII) is transmitted to third-party servers, risking exposure or compliance violations (e.g., GDPR, HIPAA).
2. **Operational Cost Scaling**: Subscription models and per-token pricing structures scale unpredictably as corporate API request volumes increase.
3. **Network Dependency**: Cloud dependencies introduce structural latency bottlenecks and mean a total loss of AI capability during a local ISP or wide-area network (WAN) outage.

### 1.2 The Solution
The Secure Edge-AI Compute Node (SECN) is a hardened, self-hosted, power-efficient local appliance designed to handle localized AI inferencing. By deploying lightweight, highly optimized 3B and 8B parameter models entirely at the network edge, the SECN eliminates data leakage vectors, operates with zero external internet dependencies, and incurs zero operational token costs after deployment.

---

## 2. Hardware Topology & System Specs

### 2.1 Physical Compute Tier
To achieve maximum performance within a low-power, compact physical footprint, the hardware stack is explicitly engineered to decouple primary system processing from neural network matrix mathematics.

* **Base Compute Host:** Raspberry Pi 5 (8GB LPDDR4X-4267 RAM). The 8GB configuration provides the maximum available memory pool required to cache OS operations, run containerized microservices, and manage memory-mapped model weights.
* **Neural Acceleration Layer:** Raspberry Pi AI Kit featuring the Hailo-8L M.2 AI Acceleration Module connected via a dedicated PCIe Gen 2.0 HAT+ interface. This provides 13 Trillion Operations Per Second (TOPS) of dedicated INT8 computation, completely offloading model inference math from the host CPU.
* **Thermal Management Layer:** Official Raspberry Pi Active Cooler (aluminum heatsink with a pulse-width modulated blower fan). This layout ensures continuous structural airflow beneath the stacked AI HAT, preventing thermal throttling under sustained context-window processing.
* **Primary Power Delivery:** Official Raspberry Pi 27W USB-C Power Supply (5.1V, 5A). Crucial for delivering stable amperage to the host board and the PCIe NPU interface.

### 2.2 Storage Architecture & OS Vector
Traditional MicroSD storage is explicitly deprecated in this architecture due to severe I/O bottlenecks and high sector-wear failure rates caused by continuous system logging.

* **Unified Boot & Storage Pool:** 2TB Externally Powered Hard Disk Drive (HDD).
* **Power Isolation Advantage:** The storage node utilizes an independent external power supply. This completely isolates the high mechanical spin-up power draw from the Raspberry Pi 5 motherboard, ensuring the Pi's 27W power supply can dedicate 100% of its delivery to the host board and the 13 TOPS PCIe AI Hat.
* **Deployment Profile:** Configured for direct USB 3.0 boot protocols running a headless OS (Ubuntu Server 24.04 LTS or Raspberry Pi OS Lite). While sequential read speeds are lower than solid-state media, the 2TB capacity provides massive, stable volume space for storing multiple quantized model weights (e.g., Llama-3 8B, Mistral 7B) alongside extensive local vector databases.

### 2.3 Physical Layer Infrastructure & Power Protection
To meet enterprise physical security and environmental availability baselines, specialized network media and industrial power conditioning are deployed at the hardware root:

* **Physical Layer Medium:** 50ft Shielded Cat6 STP (Shielded Twisted Pair) Patch Cable. The metallic foil wrapping prevents electromagnetic interference (EMI) and mitigates potential induction-based physical layer eavesdropping vulnerabilities. Grounding is strictly enforced via shielded RJ-45 connectors to a grounded chassis link.
* **Power Distribution & Transient Suppression:** CRST 12-Outlet Industrial Surge Protector. Features a heavy-duty metal housing providing professional-grade Joule suppression ratings. This safeguards the host board, the external storage drive power block, and peripheral diagnostic gear against voltage spikes, lineside noise, and thermal overload.

---

## 3. The Security Boundary & Hardening Specification

To align with enterprise Infosec compliance baselines, the SECN implements strict hardening principles across the network, host, and application layers.

### 3.1 Host Hardening & Access Control
* **Cryptographic Authentication Only:** Password-based SSH authentication is explicitly disabled (`PasswordAuthentication no`). Remote management mandates the use of secure ED25519 SSH keypairs.
* **Non-Standard Port Allocation:** The default SSH listening port is shifted from port 22 to a non-standard high port to mitigate automated brute-force discovery scripts and network reconnaissance.
* **Principle of Least Privilege (PoLP):** Default administrative accounts are removed. All administrative tasks run via scoped `sudo` privileges assigned to a dedicated, non-root system user.

### 3.2 Network Layer Security
* **Host-Based Firewall (UFW):** Default-deny inbound posture. Only explicitly authorized management ports are permitted.
* **API Isolation:** The local AI inference server engine (Ollama/Hailo RT) is strictly bound to the local loopback interface (`127.0.0.1`). The model API is inaccessible to the broader local area network (LAN) by default.
* **Reverse Proxy Integration:** Any cross-node communication requirements are fronted by an Nginx reverse proxy enforcing Transport Layer Security (TLS v1.3) encryption and local IP whitelisting.

### 3.3 Data & Model Integrity
* **Local-Only Ingestion:** Zero-telemetry configurations are enforced at the application level. System logs, prompt histories, and context vector embeddings never leave the local 2TB storage volume.

---

## 4. Software Stack & Containerization Blueprint

To maintain an immutable, easily reproducible environment, the software layer completely decouples the host OS from the AI runtime environments utilizing containerization.

* **Host Operating System**: Headless Ubuntu Server 24.04 LTS (64-bit). Stripped of graphical desktop environments to minimize background resource footprint and maximize available physical LPDDR4X RAM capacity for inference context windows.
* **Containerization Engine**: Docker Engine + Docker Compose. Microservices run within structurally isolated namespaces, eliminating dependency drift and package conflicts between the underlying host OS and application-layer Python runtimes.
* **Inference Orchestrator**: Containerized Ollama Service. Handles model weight lifecycle automation, memory-mapped allocation (mmap), context management, and local REST API exposition.
* **Hardware Accelerator Interface**: Hailo RT (Runtime) Daemon + Host PCIe Driver Stack. Acts as the execution translation bridge. It maps standard neural network computational graphs into hardware-optimized INT8 compilation maps executed directly on the Hailo-8L NPU matrix cores.

---

## 5. Architectural Data Flow Map

The operational lifecycle of a user query follows a strict, hardware-isolated execution pipeline to guarantee absolute data privacy and deterministic processing throughput:

[User Interface / Client]
       │
       ▼ (HTTPS / Local Network Only)
[Nginx Reverse Proxy / Port 443] Enforces TLS 1.3
       │
       ▼ (Internal Docker Network)
[Ollama API Engine] Checks Memory Cache
       │
       ├──► [2TB External HDD] Pulls Quantized Model Weights (INT8)
       │
       ▼ (PCIe Gen 2.0 Interface)
[Hailo-8L NPU / 13 TOPS Engine] Executes Heavy Matrix Math Inferencing
       │
       ▼ (Token Streaming Output)
[Ollama Engine] ──► [Nginx Proxy] ──► [User Terminal Interface]

### 5.1 Step-by-Step Execution Lifecycle
1. **Ingress**: The client sends a prompt over a local network connection. The connection is intercepted by the Nginx reverse proxy to verify access permissions via TLS 1.3 certificates and IP whitelisting.
2. **Model Loading**: Ollama receives the API request and verifies if the targeted model weights are cached in RAM. If not, it streams the model file directly from the externally powered 2TB HDD storage pool.
3. **Acceleration**: Instead of processing the mathematical calculations via the Pi 5's ARM CPU, the model layers are fed across the PCIe ribbon directly into the Hailo-8L NPU.
4. **Egress**: The 13 TOPS matrix engine generates tokens, sends them back to Ollama, and streams the decrypted text back to the client interface over a completely local pipeline.

---

## 6. Network Topology
┌────────────────────────┐
│    2TB External HDD    │ (Model / Vector Vault)
└───────────┬────────────┘
            │
            ▼
┌────────────────────────────────────────────────────────────────────────┐
│ MASTER HUB NODE (Raspberry Pi 5)                                       │
│ • System Router & Central Orchestrator                                 │
│ • Heavy Model Routing (3B / 8B Weights) via Hailo-8L 13 TOPS NPU       │
└──────────────┬──────────────────────────┬──────────────────────────────┘
               │                          │
               ▼ (Isolated Local LAN)     ▼ (Isolated Local LAN)
┌──────────────────────────────┐          ┌──────────────────────────────┐
│ HELPER NODE 01 (Pi 4)        │          │ HELPER NODE 02 (Pi Zero 2W)  │
│ • Small Model Inference      │          │ • Continuous Environmental   │
│ • Local Vector Embedding     │          │   Log Monitoring             │
└──────────────────────────────┘          └──────────────────────────────┘

---

## 7. Stateful Implementation & Memory Management

To maintain long-term conversational memory (state) without exceeding the physical 8GB RAM limits, the system implements a decoupled, local contextual architecture.

### 7.1 The Conversational State Engine
Passing an entire chat history back into an LLM for every new prompt causes exponential memory growth. To prevent this, the architecture uses a two-tier memory layer:

*   **Short-Term Cache (Sliding Window Buffer)**: The system holds only the last 4 to 5 exchanges in active memory. As conversation continues, the oldest messages drop off the active stack, keeping the model's RAM usage completely flat and predictable.
*   **Long-Term Database Storage**: The entire conversation history is written locally to a lightweight SQLite database residing on the 100GB OS partition. This database is purely text-based and consumes negligible processing power.

### 7.2 Context Injection Pipeline
When a user submits a prompt, the system resolves conversational state without overloading the NPU:

1. **State Retrieval**: A Python backend microservice queries the local SQLite database for the user's specific session ID.
2. **Summarization/Trimming**: The backend extracts older historical context and creates a tiny text summary (e.g., "The user is asking about configuring firewalls from a previous topic").
3. **Unified Ingress**: The backend packages the small history summary, the last 4 active chat messages, and the new prompt into a single payload.
4. **Ollama Management**: Ollama executes this payload using `OLLAMA_KEEP_ALIVE=0`. Once the NPU streams the response text back, the complete transaction is saved to the SQLite database, and the model is instantly evicted from active RAM to keep the system clean.

---

## 8. Integrated Security Tools & Reconnaissance Assets

### 8.1 On-Demand Outbound Access Controls
To balance operational updates with strict data sovereignty, outbound traffic is regulated at the host layer via the Uncomplicated Firewall (UFW).
* **Default Posture**: Default-Deny Outbound. Outbound internet routing is entirely restricted by default rules.
* **Administrative Overrides**: Scoped administrative scripts temporarily authorize outbound transport layer sessions exclusively during verified system patching or model repository synchronization loops.

### 8.2 Local Asset Discovery Engine
The SECN incorporates native raw-packet network scanning capabilities to act as an internal security posture assessment tool.
* **Tooling Layer**: Nmap (Network Mapper) utilities execute natively over the physical Cat6 interface.
* **Operational Scope**: Scheduled background discovery scripts execute localized, non-intrusive ping sweeps across the private Class C subnet boundary to catalog active host configurations, map local MAC address registers, and audit connected hardware footprints.

### 8.3 Preservation of Strict Local Boundaries
The implementation of on-demand outbound routing toggles and localized network scanning assets does not alter or weaken the foundational security architecture:
1. All core model inferencing processes remain strictly isolated to the local loopback interface (`127.0.0.1`).
2. The primary 2TB Model Vault partition operates under a zero-telemetry profile, preventing outbound data transmission regardless of the firewall's active state.

---

## 9. Implementation Roadmap: Step-by-Step Deployment

This deployment follows a progressive, modular construction strategy. Each tier must be fully verified and hardened before adding secondary infrastructure.

### Phase 1: Storage Node Conditioning & Headless Provisioning
* **Step 1.1 [Partitioning]**: Connect the 2TB external HDD to an administrative machine. Format and partition the drive using an `ext4` filesystem for native Linux performance.
* **Step 1.2 [OS Flashing]**: Use Raspberry Pi Imager to flash Ubuntu Server 24.04 LTS (64-bit Headless) or Raspberry Pi OS Lite directly onto the 2TB external HDD partition.
* **Step 1.3 [Pre-Configuration]**: Prior to booting, edit the `/boot/firmware/config.txt` file on the HDD to force USB-boot priority and enable the PCIe Gen 2.0 interface (`dtparam=pciex1`).
* **Step 1.4 [Initial Ingress]**: Connect the 2TB HDD to the Raspberry Pi 5 USB 3.0 port (blue port). Boot the system, identify the local IP via your router's DHCP pool, and establish an initial SSH handshake.

### Phase 2: Host Hardening & Container Deployment
* **Step 2.1 [Access Control]**: Generate an ED25519 SSH key pair on your admin machine. Copy the public key to the Pi 5. Modify `/etc/ssh/sshd_config` to set `PasswordAuthentication no` and shift the default port 22 to a non-standard port (e.g., 2222). Restart the SSH service.
* **Step 2.2 [Firewall Enlistment]**: Initialize the Uncomplicated Firewall (UFW). Run commands to block all traffic by default, then explicitly allow your non-standard SSH port. Enable the firewall (`sudo ufw enable`).
* **Step 2.3 [ZRAM Activation]**: Install and configure `zram-tools`. Allocate 2GB of physical RAM to act as an on-the-fly compressed swap space using the `lz4` compression algorithm. This expands the effective RAM ceiling under minor model overflows while preventing mechanical HDD swap thrashing.
* **Step 2.4 [Docker Initialization]**: Install Docker Engine and Docker Compose onto the headless OS. Add your administrative user account to the local `docker` group to eliminate the requirement for root execution.

### Phase 3: Hardware Acceleration & Inference Engine Staging
* **Step 3.1 [Thermal Assembly]**: Power down the Pi 5. Physically mount the official Active Cooler directly onto the board chips. Secure the 16mm hardware standoffs, attach the PCIe ribbon cable, and secure the Raspberry Pi AI Kit HAT+ on top of the stack.
* **Step 3.2 [Driver Installation]**: Power on the system. Run package updates and install the Hailo RT (runtime) driver daemon and firmware packages (`sudo apt install hailo-it-drivers-packages`). Execute a system reboot to register the NPU.
* **Step 3.3 [Ollama Service Stack]**: Deploy a containerized instance of Ollama via Docker Compose, mounting a dedicated directory on your 2TB HDD to serve as the local model volume path. Verify that Ollama can communicate natively with the Hailo-8L NPU drivers.
* **Step 3.4 [Model Pull & Test]**: Issue an API pull request through Ollama to download quantized versions of Llama-3-8B and a 1B parameter model (like TinyLlama). Execute a test prompt directly via the command line to verify token streaming and thermal stability.

### Phase 4: Cluster Expansion & Dynamic Routing
* **Step 4.1 [Network Segmentation]**: Configure a dedicated private subnet/VLAN on your local network switch or router specifically for cluster traffic, stripping out any outbound default gateways to ensure complete offline isolation.
* **Step 4.2 [Helper Node Enrollment]**: Flash headless lightweight OS setups onto your helper nodes (Pi 4 / Pi Zero 2W). Connect them to the isolated VLAN and hardcode static local IP addresses.
* **Step 4.3 [Orchestration Layer Deployment]**: Write a lightweight Python-based routing microservice container and deploy it on the Master Hub. This script intercepts user prompts, runs an initial token length/intent evaluation, and programmatically decides whether to trigger local NPU execution or forward the task payload to a helper node over the private network pool.