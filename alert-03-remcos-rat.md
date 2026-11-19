# Traffic Analysis: Remcos RAT Infection (Post-Infection)

**Date:** 2026-08-06
**Analyst:** Pratyush
**Verdict:** True Positive (Malware Infection - RAT)

## Incident Summary
On 2026-08-06, the Windows host `10.8.6.101` was observed communicating with a Remcos RAT Command and Control (C2) server. The traffic was encrypted and hidden over TCP port 443. Note: The initial infection vector was not captured in the provided PCAP.

## Victim Details
- **IP Address:** 10.8.6.101
- **MAC Address:** 20:e5:2a:b6:93:f1
- **Hostname:** Not found in PCAP (Post-infection capture only)
- **User Account:** Not found in PCAP

## Indicators of Compromise (IOCs)
- **C2 IP Address:** 185.14.92.102
- **C2 Domain:** onboard.radiantfluxstudio.com
- **C2 Port:** 443
- **Protocol Anomaly:** Use of outdated SSL/SSLv2 protocols for C2 communication.
- **Traffic Pattern:** Persistent TLS/SSL connection with application data beacons and large inbound data transfers (524 kB).

## Analysis
The victim machine was identified by analyzing TCP conversations in Wireshark. The host established a persistent connection to the external IP `185.14.92.102` on port 443. 

Further inspection revealed the DNS query for `onboard.radiantfluxstudio.com` resolving to this C2 IP. The connection used legacy SSL/SSLv2 protocols, which modern browsers no longer use, indicating custom or outdated encryption libraries (a known characteristic of Remcos RAT). A large data transfer (524 kB) was observed from the C2 server to the victim, likely delivering additional payloads or commands.

## Conclusion
The host is actively compromised by Remcos RAT, allowing an attacker unauthorized remote access. 
**Recommended Actions:**
1. Isolate host `10.8.6.101` immediately.
2. Block the IP `185.14.92.102` and domain `onboard.radiantfluxstudio.com` at the firewall.
3. Reset user credentials and perform a full EDR scan on the endpoint.
