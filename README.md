# friendly-octo-wagen
Building a Multi-Agent Reinforcement Learning (MARL) system with Governance, Risk, and Compliance (GRC) embedded

# Enterprise MARL System with GRC Telemetry

Building a Multi-Agent Reinforcement Learning (MARL) project moving beyond academic research into a professional-grade system with Governance, Risk, and Compliance (GRC) embedded from day one. 

This repository contains the foundation architecture for a decentralized agent environment (simulating mailbox actions like `GET`, `OPEN`, and `BIN`), backed by a robust PyTorch neural network stack and a privacy-first SQLite telemetry data lake.

## 📖 Overview

[cite_start]The core concept separates the broadcast global state ($S$), local agent observations ($o_t$), joint actions ($a$), and Decision Interpreter (DI) credit assignment ($r$), while persisting all telemetry for strict GRC auditing[cite: 2]. The system decouples the complex Reinforcement Learning math from the data storage, ensuring a clean, compliant telemetry lake ready for graphing and analysis.

### Key Capabilities
* **Decoupled Intelligence:** MARL mathematics (PyTorch) and GRC telemetry (SQLite) operate entirely independently.
* **Privacy-First Data Generation:** The environment generator enforces SHA-256 hashing on all PII (User IDs, Item IDs) before data ever touches the agents or the database.
* **Decision Interpreter Matrix:** A central rules engine handles Late Fusion, evaluates the joint action vector, assigns the global reward, and routes specific penalties (e.g., conflict penalties or security breach penalties) to prevent reward hacking.
* **Immutable Audit Trail:** Telemetry is stored across normalized tables (`environment_states`, `agent_observations`, `joint_actions`, `di_credit_assignment`) to track exactly *why* a policy updated its weights.

## 🛠️ Tech Stack
* **Machine Learning:** PyTorch (`torch`, `torch.nn`, `torch.optim`)
* **Telemetry & Storage:** SQLite (`sqlite3`)
* **Data Processing & Visualization:** Pandas, Matplotlib
* **Environment:** Python 3 (Designed for Google Colab)

## 🗄️ Database Schema (GRC Telemetry Lake)

The SQLite backend is structured to normalize data into distinct tables to track the full lifecycle:

1. `environment_states`: Captures the broadcast State ($S$) and the Collapsed State without PII. Includes `data_retention_expiry` for automated cleanup.
2. `agent_observations`: Logs what each decentralized agent actually "saw" after filtering the global state.
3. `joint_actions`: Logs the Late Fusion phase where individual actions are vectorized, alongside the raw numerical scalar global reward.
4. `di_credit_assignment`: Logs how the global reward was split so the correct agent learns, including a `policy_updated` audit trail.

## 🚀 Quick Start (Demo Mode)

The current codebase is packaged as a Jupyter Notebook (`28022026_MARL_System_with_GRC_Telemetry.ipynb`).

1. **Initialize the Database:** Run Cell 1 to generate the `marl_grc_telemetry.db` schema.
2. **Setup the Environment:** Run Cell 2 to instantiate the `SecureEnvironmentGenerator`.
3. **Initialize Agents:** Run Cell 3 to compile the generic PyTorch Neural Networks (MLPs) for the `GET`, `OPEN`, and `BIN` agents.
4. **Train the System:** Run Cell 5 to execute the `execute_training_loop()`. This will simulate the environment, extract modalities, process the forward pass, evaluate the Decision Interpreter, apply backpropagation, and log all state changes to SQLite.
5. **Visualize:** Run Cell 6 to query the database and plot the MARL System Convergence (Global Reward Optimization) over time.

## 🗺️ Roadmap to Production: Concrete GRC Improvements

[cite_start]While the concept is sound for a demo [cite: 2][cite_start], the current implementation is being aggressively upgraded to achieve a true "GRC-grade" operational standard[cite: 91]. The following Epics are currently in the backlog:

* [cite_start]**Strong Pseudonymization:** Replacing low-entropy plain SHA-256 hashes (which are vulnerable to dictionary attacks [cite: 6, 9][cite_start]) with KMS-backed HMAC[cite: 101]. [cite_start]We will never store raw identifiers[cite: 101].
* [cite_start]**Retention Enforcement & Deletion Evidence:** Transitioning from passive retention dates to automated purge processes [cite: 103] [cite_start]with cascading/transactional deletes [cite: 103] [cite_start]and a dedicated `deletion_events` tombstone table[cite: 103].
* [cite_start]**Tamper-Evidence & Data Integrity:** Enforcing `PRAGMA foreign_keys=ON` [cite: 73] [cite_start]and implementing an append-only event log pattern [cite: 105] [cite_start]where each row includes a `prev_event_hash` and `event_hash` [cite: 106] to detect unauthorized database manipulation.
* [cite_start]**ML Safety & Reproducibility:** Refactoring notebook execution order into a deterministic script[cite: 59]. [cite_start]Implementing reward clipping/normalization [cite: 81] [cite_start]and strictly logging `policy_updated` truthfully (only when an optimizer step occurred)[cite: 82].
* [cite_start]**Safe Action Semantics:** Modifying the action space so the `OPEN` agent utilizes safer actions like `OPEN_SAFE_PREVIEW`, `OPEN_SANDBOX`, or `QUARANTINE` [cite: 89] [cite_start]to prevent adversaries from bypassing static logic and triggering active payloads[cite: 48, 87].
* [cite_start]**Governance Metadata:** Adding strict data classification [cite: 99] [cite_start]and versioning telemetry (`policy_version`, `feature_schema_version`, `rule_version`)[cite: 95].

## 🔒 Security & Access
[cite_start]If the SQLite file is exported from the sandbox and contains real telemetry, it must be encrypted at rest (e.g., SQLCipher), file permissions must be locked down, and duties must be separated between read-only analysts and write-service accounts[cite: 109, 111, 112, 113].

---
