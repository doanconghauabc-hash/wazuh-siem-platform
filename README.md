# wazuh-siem-platform

**Lightweight multi-vendor SIEM/SOAR platform** with CSV-driven architecture, auto-healing, Redis HA, Kafka auto-config, PTL backpressure, and ChatOps (Slack + Telegram).

![Sender Auto-Healing](https://github.com/user-attachments/assets/0df951fa-8cee-4df9-a498-335eeba42943)
*Sender Worker auto-healing in 4 seconds.*

---

## ✨ Key Features

### 🔌 Multi‑Vendor Support
Cisco, Juniper, Fortinet, Linux, Windows, VMware, Palo Alto, Arista, HP, Checkpoint, MikroTik — each with its own handler.

### 📁 CSV‑Driven Everything
Kafka, Vector, Redis, worker scaling, routing rules, alert patterns, device mapping, Slack colors/icons, RBAC — all defined in CSV. No hardcoding.

### 🔁 Auto‑Healing
Workers respawn automatically when they die (4‑6 seconds downtime). The system rebuilds its own configuration and services even after complete deletion.

### 💬 ChatOps SOAR
Interactive buttons on Slack and Telegram: **Restart / Stop / Start / Status / Log**. Incident lifecycle tracking: `DOWN → AUTO → UP → closed`.

![Kafka Auto-Healing](https://github.com/user-attachments/assets/82040813-c7ba-4aed-8218-8975f9bbf05a)
*Kafka auto-healing after failure.*

### 🎛️ PTL — Progressive Threshold Limiter (self-developed)
PTL is an adaptive load regulation mechanism that automatically detects performance degradation thresholds and adjusts processing speed in real time — with zero manual intervention.

The system handles sudden traffic spikes, avoids bottlenecks, and prevents data loss.

*(Curious how it works? Feel free to reach out.)*

### 🗄️ Infrastructure as CSV
Kafka broker settings, topics, partitions; Vector pipelines (sources, sinks); Redis primary/standby HA — all auto-generated from CSV. No manual config files.

### 🔄 Redis HA + Kafka Distributed Consumer
- **Redis**: Master-slave with `<2s` auto-failover / failback. Incident lifecycle tracking.
- **Kafka**: Auto-generated broker config, replication, retention, IO threads, consumer groups.

### 🧩 Multi‑Worker Scaling
Independent scaling based on:
- Channel
- Topic
- Vendor
- RAM
- Queue depth

### 🧠 Resource Governor
Auto-scales workers based on available RAM + current queue depth.

### 🧰 Orchestrator Manager
Monitors 8+ supervisors with a 3-layer cross-check mechanism.

### ☁️ Disaster Recovery & Self‑Healing
- Respawn crashed workers (exponential backoff)
- Restore configuration, systemd services, Kafka topics
- Resume processing from last checkpoint
- Tested: delete everything → system rebuilds itself

---

## 📸 Alert Examples

### Cisco Interface UP/DOWN
![Cisco Interface](https://github.com/user-attachments/assets/7bef8df4-9a5c-4148-acd2-54fb9a033c03)

### Linux SSH Brute Force & Sudo
![Linux Alerts](https://github.com/user-attachments/assets/fbf1e5ce-9a1d-4a1c-9474-8254df5c9c65)

---

## 📁 CSV Structure

The entire platform is configured via CSV files — 38 files in total.

*(anh chèn ảnh cây thư mục CSV vào đây)*
configs/configs_data/
├── alert_rules.csv
├── buttons.csv
├── channels.csv
├── colors.csv
├── devices.csv
├── patterns.csv (4,344 rules)
├── patterns_network.csv (683 rules)
├── patterns_system.csv (3,585 rules)
└── ...

---

## 🛠️ Stack

- **OS**: Ubuntu 24.04
- **Log ingestion**: Wazuh + Vector
- **Streaming**: Kafka (distributed consumers)
- **Queueing**: Redis (primary/standby HA)
- **Processing**: Python (Dispatcher, Filter, Handler, Sender)
- **ChatOps**: Slack API + Telegram Bot

---

## 📫 Contact

- **GitHub**: [doanconghauabc-hash/wazuh-siem-platform](https://github.com/doanconghauabc-hash/wazuh-siem-platform)

---

*Curious about PTL internals? Want to collaborate or license the platform? Let's talk.*

Lightweight multi-vendor SIEM/SOAR platform with CSV-driven architecture, auto-healing, Redis HA, Kafka auto-config, PTL backpressure, and ChatOps (Slack + Telegram)
