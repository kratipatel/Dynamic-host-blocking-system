# Dynamic-host-blocking-system-
# Dynamic Host Blocking System using SDN

##  Problem Statement

This project implements a Software Defined Networking (SDN) solution to dynamically block a specific host in a network using a centralized controller. The system monitors traffic and installs flow rules to prevent communication from a malicious or unauthorized host.

---

##  SDN Architecture

The SDN architecture separates the control plane and data plane:

* **Controller:** Ryu Controller (decision-making)
* **Switch:** Open vSwitch (packet forwarding)
* **Hosts:** h1, h2, h3
* **Protocol:** OpenFlow 1.3

### Working:

1. Packet arrives at switch
2. If no matching rule → sent to controller (packet_in)
3. Controller decides to allow or block
4. Flow rule installed in switch

---

##  Mininet Topology

Single switch topology with three hosts:

```
h1 ----\
        s1 ---- Controller
h2 ----/
h3 ----/
```

Command used:

```bash
sudo mn --topo single,3 --controller=remote,ip=127.0.0.1,port=6633 --switch ovsk,protocols=OpenFlow13
```

---

##  Setup & Execution Steps

### 1. Install Dependencies

```bash
sudo apt update
sudo apt install mininet python3-ryu -y
```

### 2. Run Ryu Controller

```bash
ryu-manager dynamicblock.py
```

### 3. Run Mininet

```bash
sudo mn -c
sudo mn --topo single,3 --controller=remote,ip=127.0.0.1,port=6633 --switch ovsk,protocols=OpenFlow13
```

---

##  Test Scenarios

###  Allowed Communication

```
h1 ping h3
```

Result: Successful communication

###  Blocked Host

```
h2 ping h1
```

Result: Packet loss (blocked)

---

##  Flow Rule Design

### Allow Rule:

* Match: All packets
* Action: Forward (FLOOD)
* Priority: Low

### Block Rule:

* Match: ipv4_src = 10.0.0.2
* Action: DROP
* Priority: High

---

##  Performance Evaluation

### Latency:

```
h1 ping h3
```

### Throughput:

```
iperf h1 h3
```

### Flow Table:

```
sudo ovs-ofctl dump-flows s1
```

---

##  Project Structure

```
├── dynamicblock.py
├── README.md
├── screenshots/
```


---

##  Conclusion

This project demonstrates how SDN enables dynamic control over network traffic. Using a centralized Ryu controller, specific hosts can be blocked efficiently by installing OpenFlow rules, showcasing flexibility and programmability in modern networks.
