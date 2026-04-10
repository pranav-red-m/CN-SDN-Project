# 🚀 SDN Learning Switch using Mininet & POX

## 📌 Overview

This project implements a **Software Defined Networking (SDN) Learning Switch** using **Mininet** and the **POX controller**. The controller dynamically learns MAC addresses and installs flow rules to efficiently forward packets, mimicking the behavior of a traditional Ethernet switch.

---

## 🎯 Objectives

* Demonstrate **controller–switch interaction**
* Implement **match–action flow rules**
* Handle **PacketIn events**
* Observe network behavior using:

  * `ping` (latency)
  * `iperf` (throughput)

---

## 🛠️ Prerequisites

Make sure you have the following installed:

* Ubuntu / WSL (recommended)
* Python **3.6 – 3.9** (POX compatible)
* Mininet
* Open vSwitch
* POX Controller

---

## 📂 Project Structure

```
pox/
├── forwarding/
│   └── switch_custom_USE_THIS.py   # Your controller logic
├── pox.py
```

---

## ⚙️ Setup Instructions

### 1️⃣ Clone POX (if not already)

```bash
git clone https://github.com/noxrepo/pox.git
cd pox
```

---

### 2️⃣ Add Your Controller File

Place your file inside:

```bash
pox/pox/forwarding/switch_custom_USE_THIS.py
```

---

### 3️⃣ Start POX Controller

Open **Terminal 1**:

```bash
cd ~/pox
./pox.py log.level --DEBUG openflow.of_01 forwarding.switch_custom_USE_THIS
```

Expected:

* Controller starts successfully
* Switch connection logs appear

---

### 4️⃣ Start Mininet

Open **Terminal 2**:

```bash
sudo mn -c
sudo mn --topo single,3 --mac --switch ovsk --controller remote
```

This creates:

* 3 hosts → `h1, h2, h3`
* 1 switch → `s1`

---

## 🧪 Testing & Validation

### ✅ Test 1: Connectivity (Learning Behavior)

```bash
h1 ping h2
```

**Expected Behavior:**

* Initial packets may be slower (flooding)
* Later packets are faster (flow rules installed)

---

### ✅ Test 2: Throughput (iperf)

Start server:

```bash
h2 iperf -s &
```

Run client:

```bash
h1 iperf -c h2
```

**Expected:**

* Stable throughput
* Efficient forwarding

---

### 🔍 View Flow Table

```bash
sudo ovs-ofctl dump-flows s1
```

**Shows:**

* Flow rules installed by controller
* Match–action entries

---

## 🔄 Test Scenarios

### 🔹 Scenario 1: Learning Switch Behavior

* Unknown destination → Flooding
* Learned destination → Direct forwarding

---

### 🔹 Scenario 2: Controller Failure

Stop controller (`Ctrl + C`) and run:

```bash
h1 ping h2
```

**Expected:**

* No new flows installed
* Network performance degrades

---

## 📊 Observations

| Metric     | Tool      | Observation              |
| ---------- | --------- | ------------------------ |
| Latency    | ping      | Decreases after learning |
| Throughput | iperf     | Stable after flow setup  |
| Flow Rules | ovs-ofctl | Dynamically installed    |

---

## 📸 Proof of Execution

Include screenshots of:

* Ping results
* iperf output
* Flow table (`dump-flows`)
* Controller logs

---

## 🧠 Key Concepts Demonstrated

* SDN Architecture (Control vs Data Plane)
* OpenFlow Protocol
* MAC Learning
* Flow Rule Installation
* PacketIn Event Handling

---

## ▶️ Quick Run Summary

```bash
# Run controller
./pox.py log.level --DEBUG openflow.of_01 forwarding.switch_custom_USE_THIS

# Run Mininet
sudo mn --topo single,3 --mac --switch ovsk --controller remote

# Test
h1 ping h2
h2 iperf -s &
h1 iperf -c h2
```

---

## 📌 Conclusion

This project successfully demonstrates how an SDN controller can dynamically manage network behavior by installing flow rules, improving efficiency compared to traditional switching.

---

## 📚 References

* POX Documentation
* Mininet Documentation
* OpenFlow Specification

---
