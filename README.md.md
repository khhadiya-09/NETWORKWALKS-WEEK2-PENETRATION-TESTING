# WEEK 2 – PENETRATION TESTING REPORT
### Footprinting, Reconnaissance & Network Scanning

**W2-PM-FINAL | CYBERSECURITY | NETWORKWALKS**

| Field | Details |
|---|---|
| **Trainee Name** | **Hadiya Khan** |
| **Program / Batch** | Networkwalks – B084 |
| **Date** | 11 November 2026 |
| **Internship** | Cybersecurity |
| **Modules Completed** | W2-PM1 – Footprinting with Multiple Kali Tools<br>W2-PM4 – Footprinting with theHarvester<br>W2-PM5 – Network Scanning with Zenmap |
| **Footprinting Domain** | `github.com` |
| **Network Scanning Scope** | Own Wi-Fi LAN – `192.168.0.0/24` |
| **Platforms** | Kali Linux and Windows 11 |

---

## 1. Liability Disclaimer

The activities documented in this repository were performed for educational purposes as part of the Networkwalks cybersecurity internship. Domain reconnaissance was limited to publicly available information about `github.com`. Network discovery was performed only on my own local Wi-Fi network, `192.168.0.0/24`.

These activities did not include exploitation, unauthorized access, credential attacks, or modification of target systems. The results are observations from the time of testing and should not be treated as proof of vulnerabilities. Any security testing must be performed only on systems and networks that are owned by the tester or covered by explicit authorization.

## 2. Introduction

This repository documents my Week 2 practical work for the Networkwalks Cybersecurity Internship. The work covers three activities:

- **W2-PM1 – Footprinting with Multiple Kali Tools:** Used six Kali Linux tools to collect publicly available domain, web, HTTP and DNS information about `github.com`.
- **W2-PM4 – Footprinting with theHarvester:** Used theHarvester with the Hackertarget source and then with all configured sources to collect publicly indexed host and infrastructure information for `github.com`.
- **W2-PM5 – Network Scanning with Zenmap:** Used Zenmap on Windows to discover active hosts on my own Wi-Fi subnet, `192.168.0.0/24`, and review the network topology.

The repository includes the commands used, a summary of the observed results, limitations, and screenshots collected during the practical tasks. The findings are documented for learning and reporting purposes; they do not represent a full security assessment of the domain or local network.

## 3. Tools Used

| Tool | Purpose |
|---|---|
| Kali Linux | Environment used for footprinting and reconnaissance |
| WHOIS | Retrieve public domain registration and name-server information |
| WhatWeb | Identify visible web technologies and HTTP characteristics |
| `nslookup` | Resolve a domain name through DNS |
| `curl -I` | Retrieve HTTP response headers |
| WAFW00F | Check whether a web application firewall is detected |
| DNSRecon | Enumerate publicly available DNS records |
| theHarvester | Gather publicly indexed host, IP, ASN and URL information from configured sources |
| Zenmap / Nmap | Discover live hosts on the local subnet and visualize topology |
| Windows CMD | Check local IP configuration and the Wi-Fi adapter's MAC address |

## 4. Activities Performed

### 4.1 W2-PM1 – Footprinting & Reconnaissance with Multiple Kali Tools

**Target domain:** `github.com`

Six Kali Linux tools were used to collect different categories of publicly available information.

#### 4.1.1 WHOIS – Domain Registration Information

```bash
whois github.com
```

The WHOIS output identified MarkMonitor Inc. as the registrar. The record showed a domain creation date of 9 October 2007 and a registry expiry date of 9 October 2028. It also listed name servers and domain status codes. Registrant details were privacy-redacted.

![WHOIS output](screenshots/01_whois.png)

#### 4.1.2 WhatWeb – Web Technology Fingerprinting

```bash
whatweb github.com
```

WhatWeb reported an HTTP redirect to HTTPS and an HTTP 200 OK response from the HTTPS endpoint. The output identified the IP address `20.207.73.82`, HTML5, page-title information, cookies and HTTP response headers.

![WhatWeb output](screenshots/02_whatweb.png)

#### 4.1.3 nslookup – DNS Resolution

```bash
nslookup github.com
```

The query used DNS server `8.8.8.8` and returned a non-authoritative answer resolving `github.com` to `20.207.73.82`.

![nslookup output](screenshots/03_nslookup.png)

#### 4.1.4 curl – HTTP Response Headers

```bash
curl -I https://github.com
```

The server returned `HTTP/2 200`. The response included headers such as `Strict-Transport-Security`, `X-Frame-Options`, `X-Content-Type-Options`, `Referrer-Policy` and `Content-Security-Policy`. These headers describe the server response and browser security policies.

![curl output](screenshots/04_curl.png)

#### 4.1.5 WAFW00F – Web Application Firewall Detection

```bash
wafw00f github.com
```

WAFW00F v2.4.2 reported **“No WAF detected by the generic detection”** and recorded seven requests. This result only means that the generic detection did not identify a WAF; it does not prove that no firewall or other protective control is present.

![WAFW00F output](screenshots/05_wafw00f.png)

#### 4.1.6 DNSRecon – DNS Enumeration

```bash
dnsrecon -d github.com
```

DNSRecon reported SOA, NS, MX, A and TXT information. The A record shown for `github.com` was `20.207.73.82`. The output also listed name servers, the mail exchanger and several TXT records, including SPF and domain/service verification records. The DNSSEC query returned no answer in this run; this was recorded as an observation, not as a confirmed vulnerability.

![DNSRecon output](screenshots/06_dnsrecon.png)

### 4.2 W2-PM4 – Footprinting with theHarvester

**Target domain:** `github.com`  
**Tool version:** theHarvester 4.10.1

#### 4.2.1 Hackertarget Source

```bash
theHarvester -d github.com -l 1000 -b hackertarget
```

The run reported no IP addresses, email addresses or people, and identified **295 hosts**. The output included hostnames and associated IP addresses for some entries.

![theHarvester Hackertarget results](screenshots/07_theharvester_hackertarget.png)

#### 4.2.2 All Configured Sources

```bash
theHarvester -d github.com -l 50 -b all
```

The all-sources run reported:

- **64 IP addresses**
- **0 email addresses**
- **0 people**
- **4,680 hosts**
- **11 ASNs**
- **23 interesting URLs**

Several providers returned missing API-key, invalid-credential, authentication or unavailable-service messages. Therefore, these totals represent the results returned during this run and should not be treated as a complete inventory of GitHub-related assets. TheHarvester's results depend on provider availability, credentials and service limits.

![theHarvester all-sources results](screenshots/08_theharvester_all_sources.png)

### 4.3 W2-PM5 – Network Scanning with Zenmap

**Scan scope:** Own Wi-Fi LAN – `192.168.0.0/24`

Zenmap was used on Windows to identify active hosts on my own local Wi-Fi network. The Ping Scan profile was selected.

#### 4.3.1 Local IP Address and Subnet Identification

```cmd
ipconfig
```

The active Wi-Fi adapter showed:

- IPv4 address: `192.168.0.102`
- Subnet mask: `255.255.255.0`
- Default gateway: `192.168.0.1`
- Network subnet: `192.168.0.0/24`

![Windows IP configuration](screenshots/09_zenmap_ipconfig.png)

#### 4.3.2 Ping Scan and Live Host Discovery

```bash
nmap -sn 192.168.0.0/24
```

Zenmap scanned 256 IP addresses and reported **six hosts up**:

- `192.168.0.1`
- `192.168.0.100`
- `192.168.0.101`
- `192.168.0.102`
- `192.168.0.104` (`host.docker.internal`)
- `192.168.0.107`

The scan completed in approximately 7.59 seconds. The `-sn` option performs host discovery without a port scan, so these results do not identify which TCP or UDP services are open.

![Zenmap Ping Scan results](screenshots/10_zenmap_ping_scan.png)

#### 4.3.3 MAC Address Identification

Zenmap displayed MAC addresses for five discovered hosts. The MAC address for my Windows Wi-Fi adapter (`192.168.0.102`) was checked separately using `ipconfig /all`.

![Windows Wi-Fi adapter MAC address](screenshots/11_zenmap_mac_address.png)

| IP Address | MAC Address | Identification shown |
|---|---|---|
| `192.168.0.1` | `D8:47:32:2D:FA:AA` | TP-Link Technologies |
| `192.168.0.100` | `12:BA:C9:79:76:6C` | Unknown |
| `192.168.0.101` | `F6:41:C1:5F:4F:DE` | Unknown |
| `192.168.0.104` | `74:F2:FA:94:C0:3D` | Xiaomi Communications; `host.docker.internal` |
| `192.168.0.107` | `D6:DF:AC:25:4C:9A` | Unknown |
| `192.168.0.102` | `F8:54:F6:3B:6E:73` | Windows Wi-Fi adapter; locally verified |

#### 4.3.4 Network Topology

The Zenmap Topology view displayed the six discovered hosts around the local host node. The topology legend was reviewed. Since the selected scan was a Ping Scan rather than a port scan, the host-discovery results should not be interpreted as a full port assessment.

![Zenmap network topology and legend](screenshots/12_zenmap_topology.png)

The topology was also exported separately as `Week2_Zenmap_Network_Topology.pdf`.

## 5. Risk Analysis / Impact

The following are potential information-exposure or asset-management observations, not confirmed vulnerabilities. The risk levels are qualitative.

| # | Observation | Potential Impact | Level |
|---|---|---|---|
| 1 | Public WHOIS and name-server information | Can help an observer understand domain registration and DNS delegation. | Low |
| 2 | Web technology and HTTP metadata visible | May support technology fingerprinting and further authorized review. | Low |
| 3 | Public DNS records and mail configuration | Can help map public-facing infrastructure and email routing. | Low–Medium |
| 4 | WAFW00F did not generically detect a WAF | A negative generic result does not confirm the absence of protective controls. | Informational |
| 5 | Host and IP datasets returned by theHarvester | Publicly indexed hostnames and infrastructure details may increase the external information footprint. | Medium |
| 6 | Six live hosts found on the local subnet | Unrecognized devices should be inventoried and verified by the network owner. | Low–Medium |

No exploitation or vulnerability validation was performed. A hostname, IP address, DNS record, HTTP header or live host does not by itself indicate a security weakness. Further authorized assessment would be needed to establish whether a control is misconfigured or a vulnerability exists.

## 6. Recommendations

1. Review publicly available domain and DNS information periodically and remove records that are no longer required.
2. Keep web platforms, dependencies and exposed services updated.
3. Review HTTP response headers and ensure browser security controls are configured appropriately.
4. Maintain and monitor web application protection controls. Do not infer that a WAF is absent solely from a generic WAFW00F result.
5. Maintain an inventory of public subdomains and hosts, validate ownership, and investigate unexpected or obsolete entries.
6. Maintain an up-to-date inventory of devices connected to the local network and investigate unknown devices.
7. Restrict network scanning to owned systems or explicitly authorized scopes.
8. Document data-source limitations when using aggregation tools such as theHarvester.

## 7. Conclusion

During Week 2 of the Networkwalks cybersecurity internship, I completed practical activities in domain footprinting, open-source reconnaissance and local network discovery. In W2-PM1, I used WHOIS, WhatWeb, nslookup, curl, WAFW00F and DNSRecon to examine public registration, web, HTTP and DNS information for `github.com`.

In W2-PM4, I used theHarvester with the Hackertarget source and then with all configured sources. The runs returned host and IP information, while several providers were limited by missing or invalid API credentials or service availability.

In W2-PM5, I used Zenmap to scan my own Wi-Fi subnet, `192.168.0.0/24`. The scan identified six live hosts. I recorded their IP addresses, checked the local Windows Wi-Fi MAC address, reviewed the topology and exported it to PDF.

These exercises strengthened my understanding of reconnaissance workflows, DNS and HTTP information gathering, OSINT aggregation, network discovery and technical documentation. They also reinforced the importance of authorization, scope control and careful interpretation of scan results.

## 8. Repository Structure

```text
Week2-Penetration-Testing/
│
├── README.md
├── Hadiya_Khan_Networkwalks_Week2_Penetration_Testing_Report.pdf
├── Week2_Zenmap_Network_Topology.pdf
│
└── screenshots/
    ├── 01_whois.png
    ├── 02_whatweb.png
    ├── 03_nslookup.png
    ├── 04_curl.png
    ├── 05_wafw00f.png
    ├── 06_dnsrecon.png
    ├── 07_theharvester_hackertarget.png
    ├── 08_theharvester_all_sources.png
    ├── 09_zenmap_ipconfig.png
    ├── 10_zenmap_ping_scan.png
    ├── 11_zenmap_mac_address.png
    └── 12_zenmap_topology.png
```

## 9. Acknowledgement

I would like to thank **Waqas Karim (CCIE)** and the **Networkwalks** team for their guidance, support and learning resources throughout these practical activities.

---

**Hadiya Khan**  
**Networkwalks Cybersecurity Internship | Batch B084**
