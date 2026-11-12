# IOC Analysis (Indicators of Compromise)

## 1. What Is IOC Analysis?

**IOC (Indicator of Compromise)** refers to technical indicators that suggest a system, network, or digital infrastructure may have been compromised or involved in malicious activity. These indicators help security teams identify potential threats, investigate traces of cyberattacks, and implement appropriate security measures.

**IOC Analysis** is the process of collecting, examining, validating, and interpreting these indicators. Its primary objective is not simply to identify a suspicious file or IP address, but also to determine which threats these indicators may be associated with, their potential impact on the environment, and the possible attack scenarios behind them.

IOC analysis is an essential process used by **SOC (Security Operations Center)** teams, threat intelligence analysts, incident response specialists, and cybersecurity researchers.

A suspicious IP address discovered during a security incident, the SHA-256 hash of a malicious file, or an unusual DNS query can all be considered potential IOCs. However, not every suspicious indicator is direct evidence of an attack. Therefore, indicators must be evaluated within their context and validated using multiple sources.

## 2. Types of IOCs

IOCs can be categorized according to the type of technical data they represent and their relationship with malicious activities.

### 2.1. IP Address

These are IP addresses associated with suspicious or malicious activities.

Examples include:

- IP addresses of C2 (Command and Control) servers
- Servers distributing malicious software
- Source IP addresses of suspicious connections
- IP addresses used in brute-force attacks

An IP address appearing in threat intelligence databases does not automatically prove that it is malicious. Due to shared hosting, VPNs, proxies, and CDN infrastructure, the same IP address may be associated with both legitimate and suspicious activities.

### 2.2. Domain Name

These are domain names used by attackers or associated with malicious activities.

Examples include:

- Fake domains created for phishing purposes
- Domains hosting malware download links
- Domains used for C2 communication
- Fake websites created through typosquatting techniques

During domain analysis, WHOIS records, DNS history, SSL certificates, domain age, and associated IP addresses can be examined.

### 2.3. File Hash

Cryptographic hash values are used as digital fingerprints of files.

Commonly used algorithms include:

- MD5
- SHA-1
- SHA-256

SHA-256 is particularly preferred for file integrity verification and identifying known malicious files.

Example:

```text
File Name: suspicious.exe
Hash Type: SHA-256
Hash: e3b0c44298fc1c149afbf4c8996fb924...
```

If a file's hash value appears in a known malware database, it may indicate that the file has previously been classified as malicious. However, a hash match alone does not prove that the file has been executed in the current environment or that the system has been infected.

### 2.4. URL

These are specific web addresses used in malicious activities.

Examples include:

- Phishing pages
- Malware download links
- Exploit kit pages
- Malicious redirect URLs

URL analysis involves examining the domain, path, query parameters, redirects, and content hosted on the page.

### 2.5. Email Indicators

These are technical indicators associated with email-based attacks.

They may include:

- Suspicious sender addresses
- Spoofed email domains
- Malicious attachments
- Phishing links
- Suspicious routing information in email headers

In particular, SPF, DKIM, and DMARC checks can help evaluate email authentication.

### 2.6. Network Indicators

These are indicators observed in network traffic that may be associated with suspicious activities.

Examples include:

- Unusual port usage
- Unexpected outbound connections
- Suspicious DNS queries
- Abnormal data transfers
- C2 communication patterns
- Unexpected protocol usage

These indicators are commonly collected from firewall, IDS/IPS, EDR, and network traffic analysis systems.

### 2.7. Host-Based Indicators

These are indicators observed on endpoint devices or server systems.

Examples include:

- Execution of suspicious processes
- Unexpected registry modifications
- Persistence mechanisms
- Unusual file creation
- Unauthorized user accounts
- Suspicious scheduled task entries

Host-based IOCs are particularly important for understanding the impact of an attack on a system and identifying traces of malicious activity.

## 3. The IOC Analysis Process

Effective IOC analysis relies on a systematic investigation process. Collecting indicators alone is not sufficient; each indicator must be evaluated based on its source, reliability, context, and potential association with known threats.

### 3.1. IOC Collection

During the initial stage, potential IOC data is collected from various security sources.

Common data sources include:

- SIEM systems
- EDR and XDR solutions
- Firewall logs
- IDS/IPS alerts
- DNS and Proxy logs
- Threat Intelligence platforms
- Malware analysis reports
- Incident Response records

Whenever possible, collected data should be stored alongside timestamps, source systems, event descriptions, and relevant technical details.

### 3.2. IOC Enrichment

IOC enrichment is the process of supplementing collected indicators with additional information.

For example, the following information can be investigated for an IP address:

- ASN and ISP information
- Geographical location
- Reverse DNS records
- Associated domains
- Threat Intelligence records
- Previously reported malicious activities

For a file hash, additional details such as the filename, file type, detection ratio, malware family, and analysis reports can be examined.

Results from different sources should be compared during enrichment, and the reliability of each source should be taken into consideration.

### 3.3. IOC Validation

At this stage, analysts determine whether an IOC is genuinely suspicious or associated with malicious activity.

Validation may involve:

- Checking multiple threat intelligence sources
- Examining the indicator's freshness
- Evaluating the possibility of False Positives
- Investigating the indicator's legitimate use within the organization's infrastructure
- Comparing the indicator with relevant log records

For example, an IP address being flagged as malicious by a threat intelligence platform does not prove that communication with that IP address has occurred within the organization's network. Firewall and proxy logs must be examined to determine whether an actual connection took place.

### 3.4. Correlation

Correlation is the process of identifying relationships between IOCs and other security events or indicators.

For example:

1. A suspicious EXE file is detected on an endpoint.
2. The file's hash is queried against threat intelligence sources.
3. The file is identified as potentially associated with a specific malware family.
4. Suspicious DNS queries are discovered on the same endpoint.
5. The DNS records are found to point to a known C2 domain.

These relationships help security analysts build a more meaningful threat picture than individual indicators can provide.

### 3.5. Threat Assessment

During the final stage, the risk represented by the identified indicators is evaluated.

Factors to consider include:

- IOC reliability
- Threat freshness
- Criticality of the affected system
- Behaviors associated with the attack
- Frequency of the indicator's appearance within the environment
- Potential data loss or system impact

Based on the assessment, appropriate actions may include monitoring the IOC, conducting further investigation, blocking the indicator, or escalating the case to the incident response team.

## 4. Tools Used for IOC Analysis

Using multiple data sources during IOC analysis improves the accuracy and scope of investigations.

### 4.1. VirusTotal

**VirusTotal** is a platform that provides threat intelligence about file hashes, IP addresses, domains, and URLs.

Common use cases:

- File hash analysis
- Examination of malware detection ratios
- Domain and IP reputation checks
- Investigation of related files and network indicators

Official website: https://www.virustotal.com/

### 4.2. AbuseIPDB

**AbuseIPDB** is a platform used to investigate abuse reports associated with IP addresses.

Common use cases:

- Investigating suspicious IP addresses
- Evaluating Abuse Confidence Scores
- Reviewing reports associated with IP addresses
- Investigating potential brute-force and scanning activities

Official website: https://www.abuseipdb.com/

### 4.3. AlienVault OTX

**AlienVault Open Threat Exchange (OTX)** is a community-driven threat intelligence sharing platform.

Common use cases:

- IOC research
- Threat Intelligence Pulse investigation
- Discovering related threat indicators
- Tracking threat campaigns

Official website: https://otx.alienvault.com/

### 4.4. URLhaus

**URLhaus** is a project designed to collect and share URLs used for malware distribution.

Common use cases:

- Investigating malware distribution URLs
- Researching malicious domains
- Monitoring current URL-based threats

Official website: https://urlhaus.abuse.ch/

### 4.5. MalwareBazaar

**MalwareBazaar** is a platform designed for sharing malware samples and related technical information.

Common use cases:

- Malware hash research
- Investigation of malicious software samples
- Researching malware family information
- Collecting threat intelligence data

Official website: https://bazaar.abuse.ch/

### 4.6. MISP

**MISP (Malware Information Sharing Platform)** is an open-source platform used to collect, correlate, and share threat intelligence indicators.

Common use cases:

- IOC management
- Threat Intelligence sharing
- Indicator correlation
- Structuring threat events
- Automated data exchange and integration

Official website: https://www.misp-project.org/

## 5. The Relationship Between IOC Analysis and Threat Intelligence

IOC Analysis is an important component of Threat Intelligence processes.

Threat Intelligence involves collecting, analyzing, and transforming information about cyber threats into meaningful intelligence that can support decision-making.

IOCs serve as technical indicators within this process.

For example, a threat intelligence report may contain IP addresses, domains, and malware hash values used by a specific threat actor. Security teams can compare these indicators against their internal logs to investigate whether the associated threat has appeared within their environment.

Threat Intelligence is generally divided into three levels:

**Tactical Intelligence:** Focuses on attack techniques, TTPs, and adversary behaviors.

**Operational Intelligence:** Provides information about specific attack campaigns, threat actors, and operational activities.

**Strategic Intelligence:** Evaluates broader threat trends that may influence an organization's long-term security decisions.

IOC Analysis plays an important role in examining technical indicators and supporting Tactical Threat Intelligence processes.

## 6. IOC Analysis and MITRE ATT&CK

**MITRE ATT&CK** is a knowledge base that categorizes adversary tactics, techniques, and procedures (TTPs).

During IOC analysis, identified indicators can be mapped to techniques within the MITRE ATT&CK framework.

For example:

- Suspicious PowerShell execution: Command and Scripting Interpreter (T1059.001)
- Registry-based persistence: Registry Run Keys / Startup Folder (T1060, in older ATT&CK versions)
- C2 communication with external servers: Application Layer Protocol (T1071)
- Activities involving credential theft: OS Credential Dumping (T1003)

When mapping techniques, it is important to verify the ATT&CK version being used. Technique identifiers and classifications may change over time.

MITRE ATT&CK allows IOCs to be evaluated not merely as isolated indicators, but as part of a broader attack behavior.

Official website: https://attack.mitre.org/

## 7. IOC Analysis and SIEM Integration

SIEM (Security Information and Event Management) systems centrally collect logs from different sources and enable security events to be analyzed.

When IOCs are integrated into SIEM systems, events within an organization's network can be compared against known threat indicators.

Example scenario:

An organization obtains a list of known malicious IP addresses from a threat intelligence source. This list is imported into the SIEM system and compared against firewall logs.

If a match is found, an alert can be generated for further investigation.

Benefits of this integration include:

- Faster threat detection
- Automated IOC matching across logs
- Reduced investigation time for SOC analysts
- Improved visibility into known threats
- Better support for incident response processes

However, IOC-based detection mechanisms are not sufficient on their own. Newly created domains, modified malware samples, and previously unreported attack infrastructure may not exist in current IOC databases.

Therefore, IOC matching should be combined with behavioral analysis and other detection methods.

## 8. False Positives and False Negatives

Two major challenges in IOC analysis are False Positive and False Negative results.

### 8.1. False Positive

A False Positive occurs when an indicator that is not actually malicious is incorrectly classified as a threat.

For example:

An IP address may have previously been associated with malicious activities. However, the same IP address may also be used by a legitimate website hosted on shared infrastructure.

Blocking such an IP address without further investigation may disrupt legitimate services.

To reduce False Positives:

- Use multiple intelligence sources.
- Verify the freshness of IOCs.
- Examine the organizational network context.
- Perform validation before automated blocking.

### 8.2. False Negative

A False Negative occurs when an indicator that is genuinely malicious is not detected.

For example:

A newly created C2 domain used by an attacker may not yet have been reported by threat intelligence platforms.

As a result, IOC-based controls may fail to identify the communication.

To reduce False Negatives:

- Implement behavioral analysis.
- Monitor abnormal network traffic.
- Investigate endpoint activities.
- Perform Threat Hunting operations.
- Regularly update IOC lists.

## 9. Practical IOC Analysis Example

The following scenario demonstrates how IOC analysis can be applied in a real-world security operation.

### Scenario: Suspicious IP Address and Malware Communication

A SOC team at an organization detects repeated outbound connections from an endpoint to an unknown external IP address.

The initial investigation reveals the following information:

```text
Source Host: WORKSTATION-01
Destination IP: 198.51.100.25
Destination Port: 443
Protocol: HTTPS
Connection Frequency: Repeated
```

*Note: The IP address 198.51.100.25 belongs to the TEST-NET-2 range, reserved for documentation and testing purposes. It is not a real threat indicator.*

### Step 1: Log Investigation

The first step is to identify which system generated the connection.

The following data should be examined:

- Source IP address
- Destination IP address
- Connection timestamp
- Destination port
- Protocol
- Connection frequency

### Step 2: Process Analysis

The process responsible for initiating the connection is identified.

If the connection is found to originate from an unknown executable, the file should be investigated further.

### Step 3: Hash Analysis

The SHA-256 hash of the associated file is calculated and investigated using threat intelligence platforms.

### Step 4: IOC Correlation

The identified hash value, IP address, and other network indicators are compared.

Analysts investigate whether the same indicators appear in other threat reports or internal system logs.

### Step 5: Risk Assessment

If the analysis provides sufficient evidence that the connection may be associated with malicious activity, the scope of the incident is determined.

The affected endpoint, connection history, and potentially compromised systems are investigated.

### Step 6: Incident Response

After the necessary validation steps, the incident response process may be initiated.

Possible actions include:

- Isolating the affected system
- Importing relevant IOCs into security systems
- Analyzing malicious files
- Investigating persistence mechanisms
- Performing additional threat hunting across affected systems

## 10. IOC Formats and Data Standards

Specific formats and standards are used to enable IOC data to be shared across different security tools.

### 10.1. STIX

**STIX (Structured Threat Information Expression)** is a language designed to represent cyber threat intelligence in a structured and standardized format.

STIX allows information such as IP addresses, domains, file hashes, threat actors, and attack campaigns to be represented through structured objects.

Official resource: https://oasis-open.github.io/cti-documentation/

### 10.2. TAXII

**TAXII (Trusted Automated Exchange of Intelligence Information)** is a protocol used to exchange threat intelligence data between systems.

When STIX and TAXII are used together, threat intelligence data can be transferred more easily in a standardized format.

### 10.3. OpenIOC

**OpenIOC** is a format designed to describe IOC data in a structured manner.

It has been used particularly in digital forensics and incident response processes to represent and share indicators.

## 11. IOC Analysis and Threat Hunting

Threat Hunting is the proactive investigation of potential threats within an organization's infrastructure, including threats that may not have triggered security alerts yet.

IOC Analysis can serve as a starting point for Threat Hunting activities.

For example:

A threat intelligence report may identify domains used by a specific malware family. A security analyst can search historical DNS logs for these domains to investigate whether related connections have previously occurred.

However, Threat Hunting is not limited to searching for known IOCs. Suspicious attacker behaviors, abnormal process relationships, unusual authentication activities, and unexpected network communications should also be investigated.

For this reason, IOC-based searches should be combined with behavioral and hypothesis-driven threat hunting techniques.

## 12. Best Practices in IOC Analysis

Several fundamental principles should be considered when performing effective IOC analysis.

**IOC Freshness:** Old IOC records may lose their relevance over time. The last-seen timestamps and current validity of indicators should be evaluated.

**Source Reliability:** Not all threat intelligence sources provide the same level of accuracy. The historical performance and data quality of each source should be considered.

**Contextual Analysis:** An IOC should never be evaluated in isolation. The affected system, user, connection history, and related event records should be examined together.

**False Positive Validation:** Incorrectly classifying legitimate activities as malicious can lead to operational problems.

**Automation:** Automating IOC collection, enrichment, and matching processes can improve operational efficiency in large-scale environments.

**Behavioral Analysis:** Attackers can change IOC values. Therefore, security strategies based exclusively on known indicators are insufficient.

**Data Sharing and Privacy:** Sensitive internal information and personal data must be protected when sharing IOCs.

## 13. Frequently Asked Questions (FAQ)

### What is the difference between IOC and IOA?

IOC (Indicator of Compromise) refers to technical data suggesting that a system or network has previously been or is currently affected by a security compromise. IOA (Indicator of Attack), on the other hand, focuses on behaviors and signs that can be observed during an attempted or ongoing attack.

For example, a known malware hash is considered an IOC, whereas a process suspiciously executing PowerShell or attempting to access credentials may be considered an IOA.

While IOCs help identify traces of previous or ongoing compromises, IOAs focus on detecting attack behaviors and attempts.

### Is every suspicious IP address considered an IOC?

No. An IP address appearing suspicious is not sufficient to classify it as an IOC. Its connection history, reputation, intended use, and relationship with other threat indicators must be examined. Incorrect classifications may result in legitimate network traffic being blocked.

### Which teams use IOC Analysis?

IOC analysis is primarily used by SOC analysts, Threat Intelligence analysts, Incident Response specialists, and Threat Hunting teams. It also plays an important role in digital forensics and malware research.

### Can IOC Analysis be automated?

Yes. IOC collection, querying, enrichment, and matching can be automated using API integrations, SIEM systems, SOAR platforms, and Threat Intelligence solutions. However, automated results should be validated, and critical decisions must be evaluated within their proper context.

### How frequently should IOCs be updated?

The update frequency depends on the data source, the type of threat, and the organization's security requirements. Indicators associated with active attack campaigns should be checked more frequently. Keeping outdated or invalid IOCs in security systems may lead to false alerts.

### Is IOC Analysis sufficient to detect a cyberattack on its own?

No. IOC analysis is highly useful for identifying known threats, but newly created infrastructure, modified malicious files, and previously unreported attacks may bypass IOC-based controls. Therefore, it should be combined with behavioral analysis, anomaly detection, Threat Hunting, and other security controls.

### Is IOC Analysis the same as Malware Analysis?

No. IOC Analysis focuses on examining and correlating threat indicators. Malware Analysis investigates how malicious software operates, its behavior, the techniques it uses, and its impact on a system.

Hash values, domains, IP addresses, and file paths identified during Malware Analysis can subsequently be used as IOCs.

## 14. The Importance of IOC Analysis in Cybersecurity Operations

IOC Analysis is one of the fundamental processes in modern cybersecurity operations. It helps organizations identify threats more quickly, correlate security events, and assess the potential impact of attacks.

When combined with SIEM, EDR, Threat Intelligence, and Incident Response solutions, it improves an organization's visibility into potential threats.

However, effective IOC analysis involves much more than simply searching for known malicious indicators. Data must be validated, evaluated within its context, and correlated with other security events.

In today's threat landscape, where attackers continuously modify their infrastructure and indicators, IOC Analysis remains an essential component of a proactive cybersecurity strategy.
