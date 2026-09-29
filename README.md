# Exploiting vsftpd 2.3.4 Backdoor on Metasploitable 2:Gaining Root Access

## Summary

This project documents the exploitation of a known backdoor in vsftpd 2.3.4, running on a Metasploitable 2 virtual machine, resulting in root level remote code execution. The engagement was performed in an isolated lab environment for educational purposes.


<h3> Lab Environment</h3>

<table>
  <tr>
    <th>Role</th>
    <th>System</th>
    <th>IP Address</th>
  </tr>
  <tr>
    <td> Attacker</td>
    <td>Kali Linux</td>
    <td><code>192.168.0.190</code></td>
  </tr>
  <tr>
    <td> Target</td>
    <td>Metasploitable 2</td>
    <td><code>192.168.0.170</code></td>
  </tr>
</table>

## Objectives

Identify and exploit a vulnerable service on the target to obtain a remote shell with elevated privileges.

## Methodology

1. Reconnassance


Confirmed host was live and reachable:
###  Connectivity Test

```bash
$ ping 192.168.0.170

64 bytes from 192.168.0.170: icmp_seq=1 ttl=64 time=0.531 ms
64 bytes from 192.168.0.170: icmp_seq=2 ttl=64 time=0.412 ms
64 bytes from 192.168.0.170: icmp_seq=3 ttl=64 time=0.398 ms
```
<img width="1440" height="900" alt="1 reconn 2026-08-11 at 21 40 45" src="https://github.com/user-attachments/assets/4f0deab8-efac-4f97-b335-7443f86523b3" />



2. Ports & services enumeration

Ran an initial port scan, then a service/version detection scan:

<img width="1440" height="900" alt="2 version detection 2026-08-11 at 21 40 59" src="https://github.com/user-attachments/assets/d02b4879-1f7d-4b4a-9242-e7daac2abe54" />

key findings : port 21 is running

The banner was also confirmed manually via netcat:



3. Vulnerability Research

vsftpd 2.3.4 is a version of the FTP daemon whose source archive was compromised and distributed with a backdoor between June and July 2011. Any connection with a username containing :) would open a listener on port 6200, giving a root shell. Metasploit ships a dedicated module for this.
<img width="1440" height="900" alt="3 metasploit module search 2026-08-11 at 22 08 16" src="https://github.com/user-attachments/assets/2d3f3b0b-2b2d-4fc7-86c2-03fd222b80c2" />



4. Exploitation

Launched Metasploit and located the module:

<img width="1440" height="900" alt="4 RHOST troubleshooting 2026-08-11 at 22 11 56" src="https://github.com/user-attachments/assets/23d6819c-d35d-477b-b81d-c2a209b17379" />

Troubleshooting note: the module expects RHOSTS (plural), not RHOST  an early attempt using the singular option name failed validation. Setting the correct option resolved it
<img width="1440" height="900" alt="5 Successful exploit run 2026-08-11 at 22 23 16" src="https://github.com/user-attachments/assets/eae0e023-6dde-4f3e-a17a-b767b7ee8265" />



5.  Post Exploitation / Privilege Verification

Dropped into a shell and confirmed access level:
<img width="1440" height="900" alt="6 Rootshell 2026-08-11 at 22 51 02" src="https://github.com/user-attachments/assets/cb351468-a376-488d-92b4-ce7756ea3f99" />

Root access was obtained directly no privilege escalation was necessary, since the backdoor itself spawns a root level shell.



 KEY TAKEAWAYS

 
Banner grabbing matters. The vsftpd version string alone was enough to flag a known, critical vulnerability.
Supply chain compromises are real. This backdoor originated from a tampered source distribution, not a coding flaw.
Metasploit option names matter,RHOST vs RHOSTS is a small but common gotcha depending on the module.
Remediation for a real world equivalent would include: upgrading to a patched vsftpd version, verifying binary integrity against official checksums, and restricting FTP service exposure via network segmentation/firewalling.


Skills Demonstrated

Reconnaissance · Service Enumeration · Vulnerability Research · Metasploit Framework · Exploit Configuration & Troubleshooting · Privilege Verification · Technical Documentation
