# Traffic Analysis: ACR/Amatera Stealer (ClearFake ClickFix)

**Date:** 2026-11-13
**Analyst:** Pr9tyush
**Verdict:** True Positive (Malware Infection - Infostealer)

## Executive Summary
On 2026-11-13, a Windows host was infected with the ACR/Amatera Stealer via a ClearFake ClickFix campaign. The user visited a compromised legitimate website (`essentialmbandc.com`) which hosted a fake CAPTCHA. This tricked the user into executing a malicious PowerShell command that downloaded and executed the stealer.

## Victim Details
- **IP Address:** 10.11.13.128
- **MAC Address:** 00:16:76:9a:86:b9
- **Hostname:** DESKTOP-2PAS7CF
- **User Account:** Not found in capture (Likely logged in prior to PCAP start)

## Indicators of Compromise (IOCs)
- **Compromised Website:** `essentialmbandc.com`
- **Payload URL (GitLab):** `https://gitlab.com/amber-ffa-f15a6b/c5fa-4bda-b3e2-5f9c4914c525/-/raw/main/9be-4304-aca5-58755806e515`
- **Payload URL (Stager):** `https://d8815f-0edc4e94-b0180ec9a046a1e0b.palegrove.cc/install.ps1`
- **C2 Domains (Exfiltration):**
  - `socket.hollowick.cc`
  - `edge.wickenfell.cc`
  - `we.datanode.in.net`
  - `reg.datanode.in.net`
  - `brunchusher.global`

## Detailed Analysis
The user was tricked by a ClearFake CAPTCHA on a compromised website. The fake CAPTCHA instructed the user to open the Windows Run dialog (`Win+R`) and paste a command. This command executed a PowerShell script (`IEX`) that reached out to a GitLab repository to download the initial stager. 

Wireshark analysis confirmed the download request to GitLab had a `User-Agent` of `WindowsPowerShell/5.1.26100.9818`, proving it was script execution and not a standard browser download. The stager then reached out to `palegrove.cc` to download the final Amatera Stealer payload. 

After infection, the host communicated with multiple C2 domains over HTTPS (TCP 443) to exfiltrate browser credentials and system data.

## Conclusion & Recommendations
The host is compromised by an information stealer. 
**Recommended Actions:**
1. Isolate the machine `10.11.13.128` immediately.
2. Block all listed C2 domains and payload URLs at the network perimeter.
3. The user must reset all passwords (email, banking, social media) from a clean, uninfected device, as browser cookies and saved credentials were likely stolen.
4. Review PowerShell execution logs (Event ID 4104) to identify the exact command executed.
