# Honeypot Lab

## Overview
Deployed a Cowrie SSH honeypot on an isolated Ubuntu VM to capture and analyze unauthorized login attempts. Simulated real world attacker behavior using Hydra from Kali Linux, analyzed captured logs with Python, and visualized credential patterns using Matplotlib.

---

## Architecture
[Kali Linux Attacker] -> [Port 22] -> [iptables Redirect] -> [Cowrie on Port 2222]

---

## Tools Used
- Cowrie 3.0.3 (SSH Honeypot)
- Kali Linux
- Ubuntu 22.04
- Hydra
- Python 3
- Matplotlib
- VirtualBox

---

## What I Did

### 1. Honeypot Setup
- Created a dedicated cowrie user for security isolation
- Installed and configured Cowrie SSH honeypot on Ubuntu
- Configured hostname to appear as a legitimate server (webserver01)

### 2. Traffic Redirection
- Used iptables to redirect all port 22 SSH traffic to Cowrie on port 2222
- Command I used: iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-port 2222
- Attackers connect thinking they are hitting real SSH 

### 3. Attack Simulation
- Launched SSH brute-force attacks from Kali Linux using Hydra
- Used rockyou.txt wordlist and unix_users.txt username list
- Generated 54 credential attempts across 2 unique IP addresses

### 4. Log Analysis
- Parsed Cowrie JSON logs using Python
- Extracted login attempts filtering by cowrie.login.failed and cowrie.login.success event IDs
- Identified top credentials attempted by attackers

### 5. Visualization
- Built a bar chart using Matplotlib showing top 10 passwords attempted
- Revealed common default credentials like: password, 12345, 123456789, iloveyou, and princess

---

## Key Findings
- 54 total credential attempts captured
- 52 successful logins accepted by Cowrie using common passwords
- 2 failed attempts with non-default credentials
- 2 unique attacking IPs identified
- Most common passwords: 123456, 12345, 123456789, password
- This confirmed to me that attackers really rely heavily on default and common credentials

---

## Screenshots
See /screenshots folder for:
- Cowrie running and status
- iptables redirect rule
- Hydra attack output
- Python analysis output
- Matplotlib bar chart visualization

---

## What I Learned
- How honeypots capture attacker behavior without exposing real systems
- How iptables redirects network traffic at the kernel level
- How to parse JSON logs programmatically using Python
- How attackers use automated tools with common credential lists
- Why default credentials remain one of the most common attack vectors
