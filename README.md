# 🔍 OPERATION-RESOLVE-TRACE-SOC-ANALYST-CTF-CHALLENGE-Horoiheoma

![Project](https://img.shields.io/badge/Project-SOC%20Analyst%20CTF-blue)
![Focus](https://img.shields.io/badge/Focus-DNS%20Reconnaissance-purple)
![Category](https://img.shields.io/badge/Category-Threat%20Intelligence-green)
![Status](https://img.shields.io/badge/Status-Completed-success)

**Threat Intelligence & Reconnaissance Report | Network Investigations & DNS Reconnaissance**

**Analyst:** Chimezie Horoiheoma Lazarus  
**Role:** Tier-1 SOC Analyst  
**Course Module:** Network Investigations & DNS Reconnaissance

---

## 1. PROJECT OVERVIEW

OPERATION RESOLVE & TRACE was a practical SOC Analyst CTF challenge focused on DNS reconnaissance, infrastructure investigation, IP reputation analysis and network exposure assessment.

The exercise simulated a Security Operations Center investigation in which anomalous network traffic and DNS queries triggered SIEM alerts.

As a Tier-1 SOC Analyst, I was tasked with investigating an assigned domain and public IP address, examining DNS records, tracing reverse DNS mappings, reviewing threat intelligence sources and assessing network exposure.

**Investigation targets:**

| Parameter | Assigned Value |
|---|---|
| Target Domain | isc.org |
| Target Public IP | 204.152.184.88 |
| Internal Host IP | 172.31.1.50/24 |
| Private Subnet | 172.31.1.0/24 |
| Organization | Internet Systems Consortium (ISC) |

## 2. INVESTIGATION OBJECTIVES

The primary objectives were to:

1. Retrieve A, AAAA, MX and NS DNS records.
2. Perform reverse DNS (PTR) lookup.
3. Compare DNS results from `dig` and `nslookup`.
4. Identify domain infrastructure and associated IP addresses.
5. Investigate IP ownership, ASN and geolocation.
6. Review IP reputation using VirusTotal and AbuseIPDB.
7. Perform authorized port exposure validation.
8. Document findings and develop SOC recommendations.

## 3. TOOLS AND TECHNOLOGIES

### Linux CLI Utilities

| Tool | Investigation Purpose |
|---|---|
| dig | DNS record enumeration and tracing |
| nslookup | DNS resolution and record verification |
| host | DNS and reverse lookup |
| hostname | Host identification |
| ip a | Network interface information |
| ip r | Routing table inspection |
| Nmap | Authorized port and service assessment |
| Netcat (nc) | Authorized TCP connectivity testing |

### Web-Based OSINT and Threat Intelligence

| Platform | Purpose |
|---|---|
| [VirusTotal](https://www.virustotal.com/) | IP reputation and detection analysis |
| [AbuseIPDB](https://www.abuseipdb.com/) | Reported IP abuse information |
| [IP2Location](https://www.ip2location.com/) | IP geolocation and network information |
| [IPLocation](https://www.iplocation.net/) | IP geolocation investigation |
| [Hurricane Electric BGP](https://bgp.he.net/) | ASN and routing information |
| [DNSChecker](https://dnschecker.org/) | Global DNS record verification |

## 4. INVESTIGATION METHODOLOGY

### 4.1 Host and Network Context

Inspected the local Linux system and network configuration.

```bash
hostname
ip a
ip r
```

These commands provided information about the hostname, network interfaces, assigned IP addresses and routing configuration.

### 4.2 DNS Reconnaissance

Queried the target domain for its IPv4 records:

```bash
dig isc.org A
dig +short isc.org A
```

Queried IPv6 records:

```bash
dig isc.org AAAA
dig +short isc.org AAAA
```

Retrieved mail exchange records:

```bash
dig isc.org MX
```

Retrieved authoritative name servers:

```bash
dig isc.org NS
```

Checked CNAME records:

```bash
dig www.isc.org CNAME
```

Performed DNS trace:

```bash
dig isc.org +trace
```

### 4.3 Reverse DNS Investigation

Performed a PTR lookup against the assigned public IP:

```bash
dig -x 204.152.184.88
```

Used an alternative lookup method:

```bash
host 204.152.184.88
```

**Observation:** The PTR query returned SERVFAIL. No reverse DNS hostname was established from this query.

A SERVFAIL response does not necessarily mean that a PTR record does not exist; it indicates that the DNS resolution attempt failed.

### 4.4 DNS Verification Using Nslookup

Retrieved MX records using:

```bash
nslookup -type=MX isc.org
```

Compared the result with:

```bash
dig isc.org MX
```

The comparison helped reinforce the differences in output formatting and DNS query interpretation between the two utilities.

### 4.5 DNS Record Matrix

The investigation produced the following reported results:

| Record Type | Findings |
|---|---|
| A | 4 IPv4 addresses |
| AAAA | 4 IPv6 addresses |
| MX | 2 mail servers |
| NS | 5 name servers |
| CNAME | CDN-related web infrastructure observed |
| PTR | SERVFAIL response |

**Additional observation:** `www.isc.org` was observed resolving through CDN infrastructure, including the reported Fastly IPv4 address `199.232.58.217`.

This demonstrates that the website's CDN endpoint and the separately assigned investigation IP should not automatically be treated as the same service.

### 4.6 IP Ownership and Threat Intelligence Analysis

Investigated the assigned public IP using available threat intelligence and IP intelligence platforms.

The investigation covered:

- Network ownership.
- Autonomous System Number (ASN).
- Internet Service Provider.
- Geographic information.
- Abuse reports and reputation.
- VirusTotal detection information.

**Reported ownership:** Internet Systems Consortium (ISC).

**Reported ASN:** AS1280.

The supplied investigation results did not indicate malicious activity associated with the assigned IP.

### 4.7 Network Exposure Assessment

Reviewed the network exposure results from the authorized CTF exercise.

The reported Nmap findings were:

| Port | Service | Reported State |
|---|---|---|
| 80 | HTTP | Open |
| 443 | HTTPS | Open |
| Other assessed ports | Various | Filtered |

The results indicated that only HTTP and HTTPS were reported open, while other assessed ports were filtered.

Filtered ports do not necessarily mean that services are closed; packet filtering or firewall rules may prevent a definitive response.

For authorized laboratory validation, Nmap and Netcat can be used to inspect approved targets:

```bash
nmap -sS -sV -Pn -p 80,443 <AUTHORIZED_TARGET>
```

```bash
nc -zv <AUTHORIZED_TARGET> 80 443
```

Active scanning should only be performed against systems explicitly included in the assessment scope.

## 5. KEY INVESTIGATION FINDINGS

### 5.1 DNS Infrastructure

- Four IPv4 addresses were reported for isc.org.
- Four IPv6 addresses were reported.
- Two mail servers were identified.
- Five authoritative name servers were reported.
- CDN infrastructure was observed for www.isc.org.

### 5.2 Reverse DNS

The PTR lookup for `204.152.184.88` returned SERVFAIL.

The result was documented as an unsuccessful reverse lookup rather than an assumed hostname.

### 5.3 IP Reputation and Ownership

The reported investigation associated the assigned IP with ISC's network and ASN AS1280.

The supplied reputation assessment did not identify evidence of malicious activity.

### 5.4 Network Exposure

The reported port assessment identified:

- TCP/80 – HTTP.
- TCP/443 – HTTPS.
- Other assessed ports – Filtered.

These results are consistent with a publicly accessible web service, although they do not independently establish the security posture of the entire infrastructure.

## 6. EXECUTIVE SUMMARY AND RISK ASSESSMENT

The investigation identified publicly available DNS infrastructure information, associated network ownership and reported service exposure.

No confirmed malicious activity was established from the supplied investigation results.

**Overall assessment: LOW RISK (provisional).**

The low-risk assessment applies to the evidence available in this CTF exercise. It is not a certification that the target infrastructure is free from vulnerabilities or malicious activity.

The most important SOC consideration is to investigate why the internal host `172.31.1.50` communicated with the assigned public IP, including:

- Whether the connection was legitimate.
- Which process or application initiated the connection.
- Whether the communication was expected.
- Whether the destination was accessed repeatedly.
- Whether other suspicious indicators were present.

## 7. SOC RECOMMENDATIONS

Based on the investigation, the following actions are recommended:

1. **Validate internal activity:** Investigate the process and user responsible for connections from the internal host.
2. **Correlate SIEM alerts:** Compare DNS logs, firewall logs and endpoint telemetry.
3. **Monitor DNS activity:** Identify unusual DNS queries and unexpected destinations.
4. **Review IP reputation:** Recheck threat intelligence information when new indicators emerge.
5. **Maintain asset visibility:** Keep internal and external network asset inventories updated.
6. **Review exposed services:** Ensure publicly accessible services are necessary and properly secured.
7. **Avoid premature blocking:** Do not block an IP solely because it triggered an alert without supporting evidence.

## 8. SCREENSHOTS AND INVESTIGATION EVIDENCE

The following screenshots document the practical DNS reconnaissance, network investigation and threat intelligence activities.

### 8.1 DNS Reconnaissance Evidence

![DNS Reconnaissance Evidence](https://github.com/user-attachments/assets/1d8d56fe-35f9-4d66-b9c4-c4ab08618b7f)

### 8.2 DNS Record Investigation

![DNS Record Investigation](https://github.com/user-attachments/assets/4a6bab99-9ea5-46dc-853d-923810d6b7a3)

### 8.3 DNS Query Verification

![DNS Query Verification](https://github.com/user-attachments/assets/2a3bac54-d706-45e8-be42-3b693fffa195)

### 8.4 Reverse DNS Investigation

![Reverse DNS Investigation](https://github.com/user-attachments/assets/61556d6b-01bc-423e-8fa3-bee6079832b3)

### 8.5 IP Intelligence Investigation

![IP Intelligence](https://github.com/user-attachments/assets/7f93d823-4f72-49b0-9c89-43ddd3b4a151)

### 8.6 ASN and Network Ownership

![ASN Investigation](https://github.com/user-attachments/assets/fcf1b1be-15ce-4de6-aef5-6ba3516bacfb)

### 8.7 Threat Intelligence Assessment

![Threat Intelligence Assessment](https://github.com/user-attachments/assets/97963fbd-8b69-4c9c-b2e3-5bbd124c08d1)

### 8.8 Network Exposure Assessment

![Network Exposure Assessment](https://github.com/user-attachments/assets/f5452987-717c-4511-ba66-e71d8d059911)

### 8.9 Port Investigation Results

![Port Investigation](https://github.com/user-attachments/assets/2e1e0434-e4e7-47d4-8080-a7d3130d8801)

### 8.10 Final Investigation Evidence

![Final Investigation Evidence](https://github.com/user-attachments/assets/13fd41c0-1009-407b-873a-a2659185527e)

## 9. CONCLUSION

OPERATION RESOLVE & TRACE provided practical exposure to DNS reconnaissance, network investigations and threat intelligence analysis from a SOC Analyst perspective.

Through the use of Linux command-line utilities and external intelligence platforms, I strengthened my understanding of DNS records, reverse lookups, IP ownership, ASN identification, reputation assessment and network exposure validation.

One of the most important lessons from this exercise was that an alert involving a public IP address does not automatically establish malicious activity.

Effective SOC analysis requires evidence collection, verification, contextual investigation and correlation before making security decisions.

**A good SOC analyst does not just react to alerts; they investigate, validate and establish context before taking action.**

---

## 10. AUTHOR

**Chimezie Horoiheoma Lazarus**

Tier-1 SOC Analyst | Cybersecurity Enthusiast | Network Security | Threat Intelligence

**Project:** OPERATION RESOLVE & TRACE  
**Course Module:** Network Investigations & DNS Reconnaissance  
**Challenge:** SOC Analyst CTF

*Investigate. Validate. Correlate. Respond.*

