# log_server_simulator
# Swiggy Log Server Simulator

## Overview

The **Swiggy Log Server Simulator** is a Python-based TCP server that simulates multiple Swiggy branch servers generating live application logs.

It creates three independent branch servers:

- Swiggy Chennai
- Swiggy Bangalore
- Swiggy Mumbai

Each server continuously streams log entries whenever a client connects. The simulator is useful for testing:

- Log harvesting systems
- Centralized logging pipelines
- Log parsers
- Regex validators
- Distributed log collection projects

---

## Features

- Simulates **3 independent branch servers**
- Streams logs continuously over TCP
- Generates realistic Swiggy order events
- Supports multiple log levels:
  - INFO
  - WARNING
  - ERROR
  - DEBUG
- Random log generation intervals (bursty traffic)
- Intentionally injects corrupted log lines (~5%) for parser validation
- Uses multithreading so all branch servers run simultaneously

---

## Simulated Branch Servers

| Branch | Host | Port |
|---------|------|------|
| Swiggy Chennai | 127.0.0.1 | 9001 |
| Swiggy Bangalore | 127.0.0.1 | 9002 |
| Swiggy Mumbai | 127.0.0.1 | 9003 |

---

## Log Format

Each valid log follows the format:

```
YYYY-MM-DD HH:MM:SS | LEVEL | BRANCH_NAME | MESSAGE
```

Example:

```
2026-07-13 14:22:01 | INFO | swiggy-chennai | Order#3245 placed successfully
```

Example ERROR:

```
2026-07-13 14:22:09 | ERROR | swiggy-bangalore | Payment gateway timeout for Order#4567
```

---

## Corrupted Logs

To test validation logic, approximately **5%** of the generated logs are intentionally malformed.

Example:

```
CORRUPTED_LINE_NO_STRUCTURE_HERE
```

Your log parser should reject these entries.

---

## Requirements

- Python 3.8+
- No external libraries required

Uses only Python standard libraries:

- socket
- threading
- random
- time
- datetime

---

## Project Structure

```
project/
│
├── log_server_simulator.py
└── README.md
```

---

## Running the Simulator

Open a terminal and execute:

```bash
python log_server_simulator.py
```

Expected output:

```
[swiggy-chennai] listening on port 9001...
[swiggy-bangalore] listening on port 9002...
[swiggy-mumbai] listening on port 9003...

All simulated branch servers are up. Press Ctrl+C to stop.
```

Leave this program running.

---

## Connecting as a Client

Any TCP client can connect to a branch server.

Example using Python:

```python
import socket

sock = socket.socket()
sock.connect(("127.0.0.1", 9001))

while True:
    data = sock.recv(1024)
    print(data.decode())
```

---

## Sample Output

```
2026-07-13 14:20:11 | INFO | swiggy-chennai | Order#2781 placed successfully

2026-07-13 14:20:12 | DEBUG | swiggy-chennai | Cache miss while fetching menu for Order#2781

2026-07-13 14:20:13 | WARNING | swiggy-chennai | High order volume detected near Order#2781

2026-07-13 14:20:14 | ERROR | swiggy-chennai | Payment gateway timeout for Order#2781

CORRUPTED_LINE_NO_STRUCTURE_HERE
```

---

## Log Levels

### INFO

Normal application events.

Examples:

- Order placed
- Order delivered
- Restaurant accepted order

---

### WARNING

Potential operational issues.

Examples:

- Delivery delay
- High order volume
- Restaurant response time exceeded

---

### ERROR

Critical failures.

Examples:

- Payment timeout
- Restaurant unavailable
- GPS signal lost

---

### DEBUG

Internal system events.

Examples:

- Cache miss
- Database retry

---

## How It Works

1. Three TCP servers are started on separate ports.
2. Each server waits for a client connection.
3. Once connected, it continuously streams randomly generated log entries.
4. Logs are sent every **50–400 milliseconds**.
5. Random corrupted lines are occasionally injected for testing.
6. Streaming continues until the client disconnects.

---

## Use Cases

This simulator can be used for:

- Distributed systems assignments
- Centralized logging projects
- ELK Stack testing
- Log parser development
- Regex validation
- Socket programming practice
- Real-time log analytics
- Data ingestion pipeline testing

---

## Stopping the Simulator

Press:

```
Ctrl + C
```

to stop all simulated branch servers.

---

## Notes

- Only one client is accepted per branch server at a time.
- All servers run concurrently using daemon threads.
- The simulator is intended for local development and testing.
- It binds to `127.0.0.1`, making it accessible only from the local machine.

---

## Author

Swiggy Log Server Simulator – Educational project for simulating distributed log generation using Python sockets and multithreading.
