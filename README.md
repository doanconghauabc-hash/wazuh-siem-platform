# wazuh-siem-platform

**Lightweight multi-vendor SIEM/SOAR platform** with CSV-driven architecture, auto-healing, Redis HA, Kafka auto-config, PTL backpressure, and ChatOps (Slack + Telegram).

<img width="553" height="493" alt="image" src="https://github.com/user-attachments/assets/edf25669-62d5-45ed-80ee-39a019132dda" />

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

<img width="771" height="908" alt="image" src="https://github.com/user-attachments/assets/eeaa6bde-e256-4ddf-b8c5-760f5b2d11db" />

*Kafka auto-healing after failure.*

## ⚙️ Architecture Overview

### **Wazuh → Vector → Kafka → Dispatcher → Redis → Filter → Redis → Sender → Slack/Telegram**

### 🎛️ PTL — Progressive Threshold Limiter (self-developed)
PTL is an adaptive load regulation mechanism that automatically detects performance degradation thresholds and adjusts processing speed in real time — with zero manual intervention.

The system handles sudden traffic spikes, avoids bottlenecks, and prevents data loss.

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
<img width="696" height="693" alt="image" src="https://github.com/user-attachments/assets/7d2b7728-29b3-4e2a-9cd5-0d582035670d" />

---

## 📸 Alert Examples

### How Alerts Are Filtered & Routed

Users can define their own alert conditions using two fields in `devices.csv`: `log_monitor` (what type of log to watch, like INTERFACE, AUTH, CONFIG, or SYSTEM) and `monitor_condition` (the specific value to match, such as a particular interface name).

When a log matches BOTH the `log_monitor` type AND the `monitor_condition` value, an alert is triggered and sent to the Special Channel (SPC). All other logs go to the Normal Channel or are ignored.

**Example:** If you set `log_monitor=INTERFACE` and `monitor_condition=GigabitEthernet1/0/23`, you will only get alerts when that specific interface goes up or down. Any other interface changes will not trigger an alert.
<img width="1157" height="936" alt="image" src="https://github.com/user-attachments/assets/18e70098-b474-4336-9919-9d2f722014bd" />


### Alert Grouping

Before sending, multiple alerts are grouped by vendor to keep your chat clean:
This example shows alerts routed to the **Normal Channel** - events with lower severity (INFO, MEDIUM) that don't require immediate action. Examples include interface down, successful logins, and test logs.

Alerts are still **grouped by vendor** before sending to reduce noise.
<img width="783" height="1007" alt="image" src="https://github.com/user-attachments/assets/61845310-d869-4ee5-8baf-e9c13a16e763" />



### Linux SSH Brute Force & Sudo
![Linux Alerts](https://github.com/user-attachments/assets/fbf1e5ce-9a1d-4a1c-9474-8254df5c9c65)

---

## 📁 CSV Structure

The entire platform is configured via CSV files.

configs/configs_data/
├── alert_rules.csv
├── buttons.csv
├── channels.csv
├── colors.csv
├── devices.csv
├── devices_lookup.csv
├── filter_scale.csv
├── icons.csv
├── kafka.csv
├── kafka_config.csv
├── patterns.csv
├── patterns_network.csv
├── patterns_system.csv
├── redis.csv
├── sender_scale.csv
├── service_ports.csv
├── slack_action_rules.csv
├── slack_rbac.csv
├── vector.csv
├── web_server.csv
└── worker_scale.csv

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


