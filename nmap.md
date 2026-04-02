# NMAP CHEAT SHEET

```
1. HOST DISCOVERY

Host discovery determines whether a target is alive before scanning ports.

🔹 Basic Ping Scan

	nmap -sn target

Explanation:

	-sn → Disables port scanning, only checks if host is up
	Uses ICMP echo, TCP ACK, and SYN probes by default

📌 Use when:

	You only want to identify live hosts in a network


🔹 TCP SYN Ping Scan

	nmap -sn -PS target

Explanation:

	-PS → Sends TCP SYN packets to common ports (like 80, 443)
	If SYN-ACK received → host is alive

	nmap -sn -PS222 target

	-PS222 → Sends SYN packet specifically to port 222

📌 Key Insight:

	-sn + -PS → Only host discovery
	-PS alone → Also performs port scanning

	nmap -PS222 target


🔹 TCP ACK Ping Scan

	nmap -sn -PA3389 target

Explanation:

	-PA → Sends TCP ACK packets
	Useful when SYN packets are filtered by firewall

📌 Why it works:

	Some firewalls allow ACK packets assuming it's part of an existing connection


🔹 Skip Host Discovery

	nmap -Pn target

Explanation:

	-Pn → Treats host as alive (no ping)
	Directly starts port scanning

📌 Use when:

	Target blocks ICMP or appears down


🔹 Fast Scan (Top Ports)

	nmap -Pn -F target

Explanation:

	-F → Scans top 100 most common ports

📌 Advantage:

	Much faster than full scan


-------------------------------------------------------------------


2. PORT SCANNING TECHNIQUES

🔹 SYN Stealth Scan (Most Important)

	nmap -Pn -sS target

Explanation:

	-sS → Half-open scan (SYN → SYN-ACK → RST)
	Does NOT complete TCP handshake

📌 Why it's called stealth:

	Harder to detect
	Leaves fewer logs


🔹 SYN Stealth + Fast Scan

	nmap -Pn -sS -F target


🔹 TCP Connect Scan

	nmap -Pn -sT target

Explanation:

	-sT → Full TCP 3-way handshake
	OS handles connection (not raw packets)

📌 Pros:

	More accurate

📌 Cons:

	Noisy (easily detected/logged)


🔹 UDP Scan

	nmap -Pn -sU target

Explanation:

	-sU → Scans UDP ports

📌 Important:

	Slower than TCP scans
	Many services rely on UDP:
	DNS (53)
	SNMP (161)
	DHCP


-------------------------------------------------------------------


3. OS & VERSION DETECTION

🔹 Full Detection Scan

	nmap -sS -sV -O -p- target

Explanation:

	-sS → SYN scan
	-sV → Detect service versions
	-O → Detect OS
	-p- → Scan all 65535 ports


🔹 Aggressive OS Guessing

	nmap -sS -sV -O --osscan-guess -p- target

Explanation:

	--osscan-guess → Forces OS guess when uncertain


🔹 Version Intensity

	nmap -sS -sV --version-intensity 8 -O --osscan-guess -p- target

Explanation:

	Range: 0–9
	Higher value → More probes → Better accuracy

📌 Trade-off:

	High intensity = slower scan


🔹 Aggressive Scan (Shortcut)

	nmap -sS -A -p- target

Explanation:

	-A enables:
	OS detection (-O)
	Version detection (-sV)
	Script scanning (-sC)
	Traceroute

📌 Use when:

	You want maximum info quickly


-------------------------------------------------------------------


4. NMAP SCRIPTING ENGINE (NSE)

🔹 Default Scripts

	nmap -sS -sV -O -p- -sC target

Explanation:

	-sC → Runs default safe scripts

📌 Examples:

	Detect vulnerabilities
	Gather service info


🔹 List All Scripts

	ls /usr/share/nmap/scripts


🔹 Script Help

	nmap --script-help=script_name


🔹 Run Specific Script

	nmap -sS -sV --script=mongodb-info -p- -T4 target

Explanation:

	--script=mongodb-info → Runs MongoDB info script
	-T4 → Faster execution (aggressive timing)


🔹 Wildcard Scripts

	nmap -sS -sV --script=ftp-* -p21 -T4 target

Explanation:

	Runs all scripts starting with ftp-


-------------------------------------------------------------------


5. FIREWALL EVASION & IDS BYPASS

🔹 Packet Fragmentation

	nmap -sS -p- -f target

Explanation:

	-f → Splits packets into smaller fragments

📌 Purpose:

	Evades IDS/IPS detection


🔹 Custom MTU

	nmap -sS -p- -f --mtu 8 target

Explanation:

	--mtu 8 → Max packet size = 8 bytes

📌 Smaller packets = harder to detect


-------------------------------------------------------------------


6. SPOOFING & DECOYS

🔹 Single Decoy

	nmap -Pn -sS -sV -p443,3389 -f --data-length 200 -D fake_IP target_IP


🔹 Multiple Decoys

	nmap -Pn -sS -sV -p443,3389 -f --data-length 200 -D IP1,IP2 target_IP

Explanation:

	-D → Adds decoy IPs
	Makes it appear scan is coming from multiple sources


🔹 Data Length Obfuscation

	--data-length 200

Explanation:

	Adds 200 random bytes to each packet

📌 Purpose:

	Evades IDS/IPS signature detection
	Makes scan less recognizable


-------------------------------------------------------------------


7. SCAN OPTIMIZATION

🔹 Host Timeout

	nmap -sS -sV -F --host-timeout 10s target/24

Explanation:

	Stops scanning a host after 10 seconds

📌 Useful in:

	Large networks


🔹 Scan Delay (Stealth Control)

	nmap -sS -sV -F --scan-delay 5s target

Explanation:

	Waits 5 seconds between probes

📌 Key Insight:

	Slower scan = harder to detect
```
