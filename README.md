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

![image alt](https://github.com/aayourmi-cyber/metasploitable2-vsftpd-backdoor/blob/4e381b2a2c501ab438432f02c198e775383fbcd3/1%20reconn%202026-08-11%20at%2021.40.45.png)
