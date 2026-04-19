# packet-logger-sdn
## Problem Statement
Implement an SDN-based packet logging system using Mininet and Ryu controller that logs all network traffic, demonstrates selective packet blocking, and measures network performance.

## Topology
- 1 Switch (s1)
- 4 Hosts: h1 (10.0.0.1), h2 (10.0.0.2), h3 (10.0.0.3), h4 (10.0.0.4)
- 1 Remote Ryu Controller (127.0.0.1:6633)
- **Blocked pair: h1 <-> h3**

## Setup & Execution

### Requirements
- Mininet 2.3.0
- Ryu 4.34 (Python 3.9 venv)
- Open vSwitch 3.3.4

### Install Ryu
```bash
sudo add-apt-repository ppa:deadsnakes/ppa -y
sudo apt update
sudo apt install python3.9 python3.9-venv python3.9-distutils -y
python3.9 -m venv ~/ryu-env39
source ~/ryu-env39/bin/activate
pip install setuptools==58.0.0
pip install ryu
pip install eventlet==0.30.2
```

### Run the Project

**Terminal 1 - Start Controller:**
```bash
source ~/ryu-env39/bin/activate
ryu-manager packet_logger.py
```

**Terminal 2 - Start Mininet:**
```bash
sudo python3 topology.py
```

## Test Scenarios

### Scenario 1: Allowed vs Blocked
- Run `pingall` in Mininet
- h1 and h3 are blocked — all other pairs communicate freely
- Result: 16% dropped (10/12 received)

### Scenario 2: Performance Measurement
- Latency: `h1 ping -c 4 h2` → avg 1.144ms, 0% packet loss
- Throughput: `h2 iperf -s &` then `h1 iperf -c 10.0.0.2 -t 5` → 38.7 Gbits/sec
- Flow table: `sh ovs-ofctl dump-flows s1`

## Expected Output
- Controller terminal logs every packet with timestamp, MAC, IP, protocol, and action (FORWARDED/BLOCKED)
- All logs saved to `packet_log.csv`

## Proof of Execution
See screenshots in the repository.

## References
- Ryu SDN Framework: https://ryu-sdn.org
- Mininet: http://mininet.org
- OpenFlow 1.3 Specification
