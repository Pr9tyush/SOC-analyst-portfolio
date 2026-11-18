# Traffic Analysis: The Lucky Turkey (NetSupport RAT)

**Date:** 2026-11-10
**Analyst:** Pr9tyush 
**Verdict:** True Positive (Malware Infection - RAT)

## Executive Summary
On 2026-11-10 at 23:51:49, the Windows client `10.11.10.147` was infected with NetSupport RAT. The malware established Command and Control (C2) communication with the attacker's IP `93.152.209.36` over HTTP to exfiltrate system data and maintain persistence.

## Victim Details
- **IP Address:** 10.11.10.147
- **MAC Address:** 00:12:f0:e5:17:43
- **Hostname:** DESKTOP-50MVI3L
- **User Account:** edward.winslow

## Indicators of Compromise (IOCs)
- **C2 IP:** 93.152.209.36
- **C2 URL:** `http://93.152.209.36/fakeurl.htm`
- **Traffic Pattern:** Repeated HTTP POST requests to the C2 server.
- **Payload:** The traffic contained encoded binary data (`CMD=ENCD`), indicating encrypted command execution.

## Detailed Analysis
The investigation began by analyzing network endpoints to identify the host generating the most traffic. The victim machine (`10.11.10.147`) was observed making repeated HTTP POST requests to a suspicious external IP (`93.152.209.36`) at 23:51:49. 

By inspecting the HTTP streams, it was determined that the host was communicating with a NetSupport RAT C2 server. The communication included encoded data payloads (`CMD=ENCD`). Kerberos authentication logs confirmed the human user `edward.winslow` was active on the machine prior to the infection.

## Conclusion & Recommendations
The host is actively compromised by NetSupport RAT, granting the attacker remote control capabilities. 
**Recommended Actions:**
1. Immediately isolate the host `10.11.10.147` from the network.
2. Block the C2 IP `93.152.209.36` at the firewall.
3. Reset credentials for the user `edward.winslow` from a clean device.
4. Run a full antivirus/EDR scan on the infected machine.
