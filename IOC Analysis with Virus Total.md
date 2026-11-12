# VirusTotal ile IOC Analysis: Teknik İnceleme ve Uygulamalı Rehber

# 1. Introduction to VirusTotal

**VirusTotal**, files, URLs, domains, and IP addresses associated with potentially malicious activities to be investigated through multiple security engines and threat intelligence sources sağlayan bir platformdur. Cybersecurity analysts use VirusTotal to identify suspicious indicators, investigate malware samples, analyze network infrastructure, and gather additional context during security incidents.

The platform aggregates results from antivirus engines, URL and domain reputation services, behavioral analysis tools, and community contributions. This allows analysts to examine different security perspectives within a single interface rather than relying on one detection engine.

VirusTotal is particularly useful in **IOC (Indicator of Compromise) Analysis**, where technical indicators are investigated to determine whether they are associated with known threats or malicious activities.

The platform supports the investigation of several IOC types:

- **File Hashes:** MD5, SHA-1, and SHA-256 values.
- **IP Addresses:** Potentially malicious source or destination addresses.
- **Domains:** Domains associated with suspicious or malicious activities.
- **URLs:** Specific web addresses used in phishing, malware distribution, or other attacks.
- **Files:** Executables, documents, scripts, and other potentially malicious samples.

Official website: https://www.virustotal.com/

Official documentation: https://docs.virustotal.com/

# 2. Understanding the VirusTotal Interface

When an analyst searches for an IOC or submits an item for analysis, VirusTotal generates a report containing different types of security information.

The report structure depends on the submitted indicator. File reports, URL reports, domain reports, and IP address reports do not contain exactly the same information.

The most important sections generally include:

- **Summary**
- **Detection**
- **Details**
- **Relations**
- **Behavior**
- **Community**
- **History**

Some sections and advanced investigation capabilities may vary depending on the account type, available features, and report type.

## 2.1. Summary

The Summary section provides an overview of the analyzed indicator.

For a file report, this section generally includes:

- Detection statistics
- File identification information
- SHA-256 hash
- File name
- File type
- Threat classification
- Community reputation
- Relevant tags

For URL reports, the summary may include the URL's reputation, detected brands, page information, screenshot, and related threat intelligence.

The purpose of this section is to provide an initial understanding of the investigated indicator before moving into more detailed technical analysis.

## 2.2. Detection

The Detection section is one of the most important areas of VirusTotal.

It displays the results returned by individual antivirus engines and security vendors.

For example, a file report may display:

```text
Detection Results

Malicious:       15
Suspicious:       2
Undetected:      48
Total Engines:   65
```

*The numbers above are illustrative and do not represent a real scan.*

### Understanding the Detection Ratio

The detection ratio represents how many security engines classified an analyzed file or URL as malicious compared to the total number of engines that returned a result.

For example:

```text
15 / 65
```

This means that 15 out of 65 engines identified the item as malicious.

A higher detection ratio can indicate a stronger malicious reputation, but it should never be treated as definitive proof of malware.

Some important considerations:

- A high detection ratio generally increases suspicion.
- A low detection ratio does not guarantee that an item is safe.
- Different engines may use different detection methods.
- Some engines may produce False Positive results.
- Newly developed malware may not yet be recognized by security vendors.

**Important:** Detection Ratio is not the same as a probability of infection. A result of 15/65 does not mean that the file has a 23% probability of being malicious.

### Understanding Individual Engine Results

Each security engine may return a different classification.

Common results include:

**Malicious:** The engine identifies the item as malicious.

**Suspicious:** The engine identifies potentially harmful characteristics but does not necessarily classify the item as confirmed malware.

**Undetected:** The engine did not identify the item as malicious.

**Unable to Process:** The engine cannot process or analyze the submitted file type.

**Timeout:** The analysis exceeded the engine's available execution time.

**Failure:** The engine encountered an error during analysis.

For URL reports, additional classifications may appear:

- Clean site
- Unrated site
- Malware site
- Phishing site
- Suspicious site
- Spam site

An analyst should examine the detection names returned by individual engines rather than focusing exclusively on the total detection count.

For example, several engines identifying a file as a specific Trojan family provide more useful context than simply seeing a high detection ratio.

## 2.3. Community Score

The Community Score represents the reputation of an analyzed item based on community votes.

Registered VirusTotal users can provide feedback about files and URLs. These votes contribute to the item's community reputation.

The score generally uses the following visual conventions:

- **Negative Score:** Indicates a malicious reputation.
- **Positive Score:** Indicates a harmless reputation.
- **Zero or Neutral Score:** Indicates limited or balanced community feedback.

The community score is influenced by the reputation of users who submit votes.

For example:

```text
Community Score: -12
```

This indicates that the community reputation leans toward maliciousness.

However, community reputation is not equivalent to an antivirus detection result.

An item can have:

- A low detection ratio and a negative community score.
- A high detection ratio and limited community feedback.
- A positive community score despite suspicious technical characteristics.

Analysts should treat community feedback as supplementary intelligence rather than a definitive security verdict.

## 2.4. File Identification Information

VirusTotal provides several technical attributes that help uniquely identify an analyzed file.

The most important attributes include:

**SHA-256:** A cryptographic hash used to identify a file. It is commonly preferred for malware identification and file integrity verification.

**SHA-1:** An older cryptographic hash algorithm that may still be useful when searching historical threat intelligence records.

**MD5:** Another commonly encountered hash algorithm, particularly in older malware databases.

**File Name:** The name associated with the submitted file. File names can change, so they should not be treated as reliable identifiers.

**File Size:** The size of the analyzed file, generally represented in bytes.

**File Type:** The actual format or type of the file, which may differ from its extension.

For example, a file named `document.pdf` may actually contain executable content. Therefore, analysts should examine the detected file type rather than relying only on the filename.

### Why Is SHA-256 Important?

SHA-256 produces a 256-bit hash value that can be used as a digital fingerprint.

Even a small modification to a file generally results in a different hash value.

For example:

```text
Original File:
SHA256: a1b2c3...

Modified File:
SHA256: d4e5f6...
```

This makes SHA-256 useful for:

- Identifying known malware samples
- Comparing suspicious files
- Searching threat intelligence databases
- Verifying file integrity
- Tracking previously analyzed samples

A hash value identifies a specific file version, not an entire malware family. Different versions of the same malware can have completely different hashes.

# 3. Performing IOC Analysis with VirusTotal

VirusTotal supports different investigation methods depending on the type of IOC being analyzed.

The following sections explain how to investigate files, IP addresses, domains, and URLs.

## 3.1. File Hash Analysis

File hash analysis is one of the most common methods used in malware investigations.

Instead of immediately uploading a suspicious file, analysts can first calculate its hash and search for an existing report.

### Step 1: Calculate the File Hash

On Linux, SHA-256 can be calculated using:

```bash
sha256sum suspicious.exe
```

For MD5:

```bash
md5sum suspicious.exe
```

For SHA-1:

```bash
sha1sum suspicious.exe
```

On Windows PowerShell:

```powershell
Get-FileHash .\suspicious.exe -Algorithm SHA256
```

Example output:

```text
Algorithm : SHA256
Hash      : e3b0c44298fc1c149afbf4c8996fb924...
Path      : C:\Analysis\suspicious.exe
```

### Step 2: Search the Hash on VirusTotal

1. Open https://www.virustotal.com/
2. Navigate to the search field.
3. Paste the calculated SHA-256 value.
4. Open the corresponding report if it exists.

Searching by hash allows analysts to retrieve an existing report without uploading the file again.

If no report exists, the analyst may need to submit the sample for analysis, provided that doing so is authorized and appropriate.

### Step 3: Examine Detection Results

Open the Detection section and review:

- Total malicious detections
- Individual engine verdicts
- Malware classification names
- Suspicious or undetected results
- Detection consistency between vendors

Avoid making a final decision based solely on the detection ratio.

### Step 4: Examine File Details

The Details section contains additional technical information about the file.

Depending on the file type, available attributes may include:

- File type description
- File size
- First submission date
- Last submission date
- Last analysis date
- File signatures
- Compiler information
- PE information
- Import and export information
- Section information
- Embedded resources
- File metadata

For Windows Portable Executable files, PE-related information can be especially useful.

### Important PE Attributes

**PE (Portable Executable)** is the file format used by Windows executable files, DLLs, and other binary components.

Some important PE attributes include:

**Entry Point:** The address where execution begins when the executable is loaded.

**Image Base:** The preferred memory address at which the executable is loaded.

**Sections:** Different parts of the executable, such as `.text`, `.data`, and `.rsrc`.

**Imports:** External functions and libraries used by the executable.

**Exports:** Functions or symbols made available to other programs.

**Imphash:** A hash calculated from the import information of a PE file. It can help identify similarities between executable files.

**Rich Header:** Additional compiler and build-related information that may help identify the environment used to create the executable.

These attributes can provide useful information when investigating suspicious Windows executables.

## 3.2. IP Address Analysis

IP address analysis helps identify potentially malicious network infrastructure and understand its relationships with other indicators.

An IP address may be associated with:

- Command and Control infrastructure
- Malware distribution servers
- Phishing infrastructure
- Suspicious hosting providers
- Scanning and brute-force activities

### Step 1: Search the IP Address

1. Open VirusTotal.
2. Enter the IP address into the search field.
3. Open the corresponding IP report.

Unlike file and URL reports, IP address reports do not generally provide a conventional antivirus detection ratio for the IP itself.

Instead, they provide reputation and contextual information gathered from VirusTotal's datasets.

### Step 2: Examine IP Address Information

Important fields may include:

**Country:** The country associated with the IP address.

**ASN (Autonomous System Number):** Identifies the autonomous system responsible for announcing the IP address range.

**AS Owner:** The organization associated with the autonomous system.

**Network:** The IP range or network associated with the address.

**Reverse DNS:** The hostname associated with the IP address, when available.

**Reputation:** A reputation value derived from available VirusTotal information.

**Last Analysis Date:** The timestamp associated with the latest available analysis information.

These fields help analysts understand the infrastructure behind an IP address.

### Step 3: Examine Open Ports and Services

VirusTotal may provide information about observed open ports, detected services, and technologies associated with an IP address.

Examples include:

```text
Port: 22
Service: SSH

Port: 80
Service: HTTP

Port: 443
Service: HTTPS

Port: 3389
Service: RDP
```

The presence of an open port does not automatically indicate malicious activity.

However, unexpected services, outdated software, and exposed administrative interfaces may provide useful context during an investigation.

### Step 4: Examine Passive DNS

**Passive DNS** is one of the most valuable sources of information during IP and domain investigations.

It records observed relationships between domain names and IP addresses over time.

For example:

```text
Domain: suspicious-example.com
IP: 203.0.113.10

Domain: another-example.com
IP: 203.0.113.10
```

This may indicate that multiple domains have resolved to the same IP address.

Such relationships can help analysts discover:

- Related malicious domains
- Shared hosting infrastructure
- Potential phishing campaigns
- Malware distribution infrastructure
- Changes in domain resolution over time

However, shared IP addresses are common in legitimate hosting environments. A shared IP alone is not sufficient evidence that domains belong to the same threat actor.

## 3.3. Domain Analysis

Domain analysis focuses on identifying suspicious domain names and understanding their infrastructure.

Domains may be associated with:

- Phishing campaigns
- Malware distribution
- C2 communication
- Typosquatting
- Credential harvesting
- Malicious redirects

### Step 1: Search the Domain

Enter the domain name into the VirusTotal search field.

For example:

```text
suspicious-example.com
```

Open the corresponding domain report.

### Step 2: Examine Domain Information

Important fields may include:

**Registrar:** The organization through which the domain was registered.

**Creation Date:** The date the domain was initially registered.

**Expiration Date:** The domain's registration expiration date.

**Last Update Date:** The last recorded modification to the registration information.

**Name Servers:** The DNS servers responsible for the domain's DNS records.

**WHOIS Information:** Registration and ownership-related information, depending on availability.

**DNS Records:** Records associated with the domain, such as A, AAAA, MX, NS, and TXT records.

### Why Is Domain Age Important?

Recently registered domains can sometimes be associated with phishing campaigns and temporary malware infrastructure.

For example, an organization may discover a domain that:

- Was registered only a few days ago.
- Uses a name similar to a legitimate brand.
- Resolves to infrastructure associated with suspicious activities.
- Has no established legitimate reputation.

These characteristics may increase suspicion, but domain age alone does not prove malicious intent.

### Step 3: Investigate Subdomains

Subdomains can provide additional insight into an organization's or attacker's infrastructure.

For example:

```text
example.com
login.example.com
mail.example.com
cdn.example.com
```

Investigating observed subdomains may reveal additional services, web applications, or potentially compromised resources.

### Step 4: Examine Related IP Addresses

Review the domain's historical and current IP resolutions.

Compare:

- IP addresses
- DNS resolution history
- Hosting providers
- Related domains
- Observed communication patterns

This information can help establish relationships between different infrastructure components.

## 3.4. URL Analysis

URL analysis is particularly useful when investigating phishing links, suspicious redirects, and malicious web pages.

A URL can contain several components:

```text
https://example.com/login?user=admin
```

The components include:

- Scheme: `https`
- Domain: `example.com`
- Path: `/login`
- Query Parameter: `user=admin`

Attackers may manipulate these components to hide malicious destinations or imitate legitimate websites.

### Step 1: Submit the URL

1. Open VirusTotal.
2. Navigate to the URL submission area or search field.
3. Enter the suspicious URL.
4. Open the generated analysis report.

VirusTotal may normalize submitted URLs before matching them against existing reports.

### Step 2: Examine Detection Results

Review the security vendor classifications.

Pay particular attention to:

- Phishing detections
- Malware distribution classifications
- Suspicious website classifications
- Malicious redirects
- Spam-related classifications

A URL with no detections is not automatically safe. Newly created phishing pages and short-lived malicious URLs may not yet be recognized.

### Step 3: Examine HTTP Information

Depending on the available report, the Details section may provide:

**HTTP Status Code:** Indicates the server's response to the request.

Common examples:

- `200 OK`: The request was successfully processed.
- `301 Moved Permanently`: The resource has been permanently redirected.
- `302 Found`: The resource has been temporarily redirected.
- `403 Forbidden`: Access to the requested resource is denied.
- `404 Not Found`: The requested resource could not be found.
- `500 Internal Server Error`: The server encountered an internal error.

**Content-Type:** Indicates the type of content returned by the server.

**Response Headers:** Provide information about the server response and its configuration.

**Final URL:** The final destination reached after redirects.

**Page Title:** The title extracted from the webpage.

These attributes can help identify suspicious redirects and inconsistencies between the submitted URL and the final destination.

### Step 4: Examine Page Information

VirusTotal may provide additional information about the analyzed webpage, including:

- Page screenshots
- HTML information
- Identified brands
- Web technologies
- External links
- JavaScript information
- Cookies
- Contacted domains
- Contacted IP addresses

For example, a page imitating a banking website while loading resources from unrelated infrastructure may require further investigation.

**Security Note:** Do not open suspicious URLs directly in your regular browser. Use controlled analysis environments and follow your organization's security procedures.

# 4. Understanding the Relations Tab

The Relations section is one of the most valuable features for IOC investigations.

It helps analysts identify relationships between different indicators and discover additional infrastructure associated with a suspicious file, IP address, domain, or URL.

Rather than examining an indicator in isolation, analysts can investigate its connections to other objects within VirusTotal's dataset.

The available relationships depend on the analyzed object and account permissions.

## 4.1. Contacted IP Addresses

This relationship identifies IP addresses contacted by a file during its execution in an analysis environment, where such information is available.

For example:

```text
File: suspicious.exe

Contacted IPs:
- 203.0.113.10
- 198.51.100.20
```

These addresses may represent:

- C2 servers
- Remote web servers
- DNS infrastructure
- Other external services

Analysts should investigate each address to determine whether the communication is suspicious.

## 4.2. Contacted Domains

This section identifies domains contacted by a file during observed execution.

For example:

```text
File: suspicious.exe

Contacted Domains:
- update-example.com
- api-example.net
```

A file communicating with an unknown domain may indicate suspicious network behavior.

However, legitimate applications also communicate with external services. The purpose and context of the communication must be investigated.

## 4.3. Downloaded Files

This relationship identifies files downloaded from a URL or associated infrastructure.

For example:

```text
URL: suspicious-example.com/download

Downloaded Files:
- payload.exe
- update.dll
```

This information can help identify additional malware components or files distributed through the same infrastructure.

## 4.4. Communicating Files

Communicating Files identifies files that have been observed communicating with a particular IP address, domain, or URL during sandbox execution.

This relationship can help identify malware samples that share the same network infrastructure.

For example, multiple suspicious executables communicating with the same external IP address may suggest a shared C2 infrastructure.

## 4.5. Files Referring to an Indicator

VirusTotal can identify files containing references to specific IP addresses or domain names.

For example, a file may contain a hardcoded C2 domain inside its strings or configuration data.

Finding multiple files referencing the same domain can help analysts identify related malware samples.

However, a reference inside a file does not necessarily mean that the file actually communicates with that indicator.

The distinction between an embedded reference and an observed network connection is important.

# 5. Understanding the Behavior Tab

The Behavior section provides information about activities observed when a submitted file is executed in a controlled analysis environment.

This is particularly useful for malware investigations because static file information alone cannot always reveal what a program actually does.

VirusTotal uses sandbox technologies to execute supported samples and collect behavioral information.

The exact details depend on the file, sandbox, analysis configuration, and available account features.

## 5.1. Process Activity

This section may reveal processes created or executed during analysis.

For example:

```text
Parent Process: explorer.exe
Child Process: powershell.exe
```

An executable spawning PowerShell may be worth investigating, particularly if the command line contains suspicious arguments.

Other potentially interesting behaviors include:

- Unexpected child processes
- Suspicious command-line arguments
- Process injection indicators
- Unusual execution chains
- Attempts to launch system utilities

A process spawning another process is not inherently malicious. The execution context must be considered.

## 5.2. File System Activity

Malware may create, modify, or delete files during execution.

Behavioral analysis can reveal:

- Newly created files
- Modified files
- Dropped executables
- Temporary files
- Files written to startup directories

For example:

```text
Created File:
C:\Users\User\AppData\Roaming\payload.exe
```

A suspicious executable creating another binary in a user profile directory may indicate a malware installation or persistence attempt.

## 5.3. Registry Activity

Windows malware frequently interacts with the Windows Registry.

The Behavior section may identify registry keys and values modified during execution.

For example:

```text
Registry Key:
HKCU\Software\Microsoft\Windows\CurrentVersion\Run
```

This location is commonly associated with application startup configuration.

Unexpected modifications to startup-related registry locations may indicate a persistence mechanism.

Other registry activities worth investigating include:

- Security configuration changes
- Startup modifications
- System configuration changes
- Suspicious registry value creation

## 5.4. Network Activity

Behavioral reports may reveal network communication initiated by an analyzed file.

Common observations include:

- DNS queries
- HTTP requests
- HTTPS connections
- Contacted IP addresses
- Contacted domains
- Network protocols
- Remote connection attempts

For example:

```text
Process: suspicious.exe
Destination: suspicious-example.com
Protocol: HTTPS
Port: 443
```

This information can be correlated with IP and domain reports to identify potentially malicious infrastructure.

## 5.5. Mutexes

A **Mutex (Mutual Exclusion Object)** is a synchronization mechanism used by applications to control access to shared resources.

Malware may create named mutexes to:

- Prevent multiple instances from running
- Identify an existing infection
- Coordinate execution between processes
- Maintain internal execution states

A suspicious or known malware-associated mutex can provide an additional detection opportunity.

However, mutex creation is also common in legitimate software.

## 5.6. MITRE ATT&CK Techniques

VirusTotal behavioral information may include mappings to MITRE ATT&CK techniques.

These mappings help analysts understand the observed behavior within a standardized attack framework.

Examples include:

**T1059.001 – PowerShell**

Execution of commands or scripts through PowerShell.

**T1053.005 – Scheduled Task**

Use of Windows Scheduled Tasks for execution or persistence.

**T1547.001 – Registry Run Keys / Startup Folder**

Using startup-related registry keys or folders to maintain persistence.

**T1055 – Process Injection**

Injecting code into another process.

**T1071 – Application Layer Protocol**

Using application-layer protocols for communication, potentially including C2 traffic.

An ATT&CK mapping is a useful analytical clue, but it should be interpreted according to the underlying observed behavior.

Official reference: https://attack.mitre.org/

# 6. Understanding VirusTotal Graph

**VirusTotal Graph** is a visual investigation tool that helps analysts explore relationships between different indicators.

Instead of manually switching between multiple reports, analysts can visualize connected objects in a graph.

A graph may contain:

- Files
- IP addresses
- Domains
- URLs
- DNS resolutions
- Related malware samples
- Other threat intelligence objects

For example, an investigation may reveal the following relationships:

```text
             suspicious.exe
                   |
                   |
             contacts
                   |
                   v
              203.0.113.10
                   |
                   |
             resolves to
                   |
                   v
          suspicious-example.com
                   |
                   |
             distributes
                   |
                   v
               payload.dll
```

*This is an illustrative relationship diagram, not an actual VirusTotal investigation.*

### Why Is Graph Analysis Important?

Graph analysis helps identify connections that may not be obvious when examining individual indicators.

For example:

1. A suspicious file is identified.
2. Its contacted IP addresses are investigated.
3. A related domain is discovered.
4. The domain is found to be associated with additional suspicious URLs.
5. Other files communicating with the same infrastructure are identified.

This process can help analysts expand an investigation from a single IOC into a broader threat infrastructure.

Graph relationships do not automatically prove that all connected objects belong to the same threat actor. Each relationship must be validated.

# 7. Historical Analysis and First/Last Submission Dates

Time-related information is important when investigating suspicious indicators.

VirusTotal provides different timestamps that help analysts understand when an item was first observed, submitted, or analyzed.

Important fields include:

**First Submission Date:** The earliest recorded submission of the item to VirusTotal.

**Last Submission Date:** The most recent recorded submission.

**Last Analysis Date:** The timestamp of the latest available analysis.

**Last Modification Date:** The recorded modification time of the file, when available.

These timestamps should not be confused with the actual date of an attack or infection.

For example, a file submitted to VirusTotal for the first time in November does not necessarily mean that it was created or first used in November.

Historical analysis can help answer questions such as:

- When was this indicator first observed?
- Has its detection ratio changed?
- Was the indicator previously classified as harmless?
- Has the associated infrastructure changed?
- Are there historical reports connecting it to other threats?

Detection history is particularly useful for identifying changes in how security vendors classify a file or URL over time.

# 8. Practical Investigation Scenario

The following scenario demonstrates how a SOC analyst might use VirusTotal during a security investigation.

## Scenario: Investigating a Suspicious Executable

A SOC analyst receives an alert from an endpoint security solution.

The alert indicates that an unknown executable has been detected on an employee workstation.

The initial information is:

```text
Hostname: WORKSTATION-01
File Name: suspicious.exe
File Path: C:\Users\User\Downloads\suspicious.exe
Detection Source: EDR
```

The analyst decides to investigate the file using VirusTotal.

### Step 1: Calculate the SHA-256 Hash

The analyst calculates the file's SHA-256 value:

```powershell
Get-FileHash .\suspicious.exe -Algorithm SHA256
```

The resulting hash is copied for further investigation.

### Step 2: Search VirusTotal

The analyst searches for the SHA-256 value on VirusTotal.

An existing report is found.

The report contains the following illustrative information:

```text
File Type: Win32 EXE
Detection Ratio: 18/70
Community Score: -8
First Submission: 2026-10-20
Last Analysis: 2026-11-10
```

### Step 3: Analyze Detection Results

The analyst examines the Detection section.

Several security engines classify the file as a Trojan.

Other engines return Undetected.

The analyst considers the detection ratio suspicious but does not immediately conclude that the file is malicious.

The detection names and classifications are compared across multiple vendors.

### Step 4: Examine File Details

The analyst reviews the Details section.

The file is identified as a Windows PE executable.

Additional information reveals:

- File size
- SHA-256 value
- PE metadata
- Import information
- File creation-related metadata, where available
- Relevant file tags

These attributes provide additional context for the investigation.

### Step 5: Examine Behavioral Information

The Behavior section reveals several potentially suspicious activities:

```text
Process Activity:
- powershell.exe execution

Registry Activity:
- Startup-related registry modification

Network Activity:
- Outbound HTTPS connection

File Activity:
- Additional executable creation
```

The analyst investigates whether these activities are consistent with the file's expected functionality.

### Step 6: Investigate Network Indicators

The contacted domain and IP address are extracted from the behavioral report.

The analyst searches both indicators on VirusTotal.

The IP report reveals additional domains associated with the same infrastructure.

The domain report provides further information about DNS history and related indicators.

### Step 7: Correlate the Findings

The analyst compares the available information:

- File detection results
- PE attributes
- Process execution
- Registry modifications
- Contacted IP addresses
- Related domains
- Historical analysis information

The findings are evaluated together rather than treating each indicator independently.

### Step 8: Document the Investigation

The analyst records the findings in an investigation report.

The report should contain:

- Original alert information
- File hash
- VirusTotal report reference
- Detection results
- Behavioral observations
- Related network indicators
- Risk assessment
- Recommended response actions

If sufficient evidence supports a malicious classification, the incident can be escalated to the incident response team.

# 9. VirusTotal API and Automated IOC Analysis

VirusTotal also provides an API that allows analysts and developers to integrate threat intelligence capabilities into their own applications.

The API can be used to retrieve reports and submit supported indicators programmatically.

Common API operations include:

- Retrieving file reports using hashes
- Submitting files for analysis
- Scanning URLs
- Retrieving URL reports
- Retrieving domain reports
- Retrieving IP address reports
- Accessing related threat intelligence information

Official API documentation:

https://docs.virustotal.com/reference/overview

## 9.1. Getting Started with the API

To use the API:

1. Create a VirusTotal account.
2. Access your API key from your account settings.
3. Read the API documentation.
4. Make requests to the appropriate API endpoints.
5. Process the returned JSON responses.

The API key must be kept confidential and should never be published in a public GitHub repository.

## 9.2. Example: Retrieving a File Report

The following Python example retrieves an existing file report using its SHA-256 hash.

```python
import requests

API_KEY = "YOUR_API_KEY"

FILE_HASH = "YOUR_SHA256_HASH"

url = f"https://www.virustotal.com/api/v3/files/{FILE_HASH}"

headers = {
    "x-apikey": API_KEY
}

response = requests.get(url, headers=headers)

if response.status_code == 200:
    data = response.json()

    attributes = data["data"]["attributes"]

    stats = attributes.get("last_analysis_stats", {})

    print("SHA256:", attributes.get("sha256"))
    print("File Type:", attributes.get("type_description"))
    print("Detection Results:", stats)

elif response.status_code == 404:
    print("File report not found.")

elif response.status_code == 401:
    print("Invalid API key.")

elif response.status_code == 429:
    print("API quota or rate limit exceeded.")

else:
    print("Request failed:", response.status_code)
```

### Understanding the API Response

The API returns information in JSON format.

An illustrative response structure may look like this:

```json
{
    "data": {
        "type": "file",
        "id": "example_sha256_hash",
        "attributes": {
            "sha256": "example_sha256_hash",
            "type_description": "Win32 EXE",
            "last_analysis_stats": {
                "malicious": 18,
                "suspicious": 2,
                "undetected": 48,
                "harmless": 0,
                "timeout": 1,
                "failure": 1
            }
        }
    }
}
```

The most important attributes include:

**data:** Contains the main object returned by the API.

**type:** Identifies the object type, such as a file, domain, IP address, or URL.

**id:** Identifies the requested object.

**attributes:** Contains the technical information associated with the object.

**last_analysis_stats:** Provides the aggregated results of the latest analysis.

The individual engine results can also be retrieved from the file's `last_analysis_results` attribute.

API responses may contain additional fields depending on the object type and available permissions.

## 9.3. Understanding API Detection Statistics

The following fields are particularly useful:

**malicious:** Number of engines classifying the item as malicious.

**suspicious:** Number of engines classifying the item as suspicious.

**undetected:** Number of engines that did not detect malicious activity.

**harmless:** Number of engines classifying the item as harmless.

**timeout:** Number of engines that timed out during analysis.

**failure:** Number of engines that failed to complete their analysis.

**type-unsupported:** Number of engines unable to process the submitted file type, where applicable.

These statistics should be interpreted according to the analysis status and the number of engines that actually returned a verdict.

## 9.4. Automating IOC Enrichment

An analyst can extend the API integration to automatically enrich suspicious indicators.

For example:

1. Receive a suspicious file hash from an EDR alert.
2. Query VirusTotal using the hash.
3. Retrieve the file's detection statistics.
4. Extract related IP addresses and domains, where accessible.
5. Investigate the related indicators.
6. Generate a structured investigation report.

Such workflows can be integrated into SIEM and SOAR platforms to support security operations.

However, API access, quotas, rate limits, and feature availability depend on the VirusTotal subscription and API terms.

# 10. Limitations and Important Considerations

Although VirusTotal is a valuable threat intelligence platform, its results should not be treated as an absolute source of truth.

There are several important limitations.

## 10.1. Detection Does Not Guarantee Maliciousness

A high detection ratio increases suspicion, but False Positives are possible.

Analysts should investigate detection names, behavioral information, and additional intelligence before making a final decision.

## 10.2. Undetected Does Not Mean Safe

A file with zero detections may still contain malicious functionality.

New malware, modified samples, and targeted attacks may not be recognized by existing security engines.

## 10.3. Submission and Privacy Risks

VirusTotal is a threat intelligence sharing platform.

Submitted files, URLs, and queried indicators may become part of its dataset and may be accessible to security partners or other authorized users, depending on the service and account terms.

Therefore:

- Do not upload confidential company documents without authorization.
- Do not submit files containing personal information.
- Do not submit internal credentials or authentication tokens.
- Do not query sensitive internal URLs or private indicators without reviewing the applicable policies.
- Follow organizational data handling procedures.

When investigating a file, searching for its hash first is generally preferable to uploading the sample immediately.

## 10.4. Sandbox Limitations

Sandbox analysis does not guarantee that every malicious behavior will be observed.

Some malware may:

- Detect virtualized environments
- Delay execution
- Require specific user interaction
- Wait for particular network conditions
- Execute only under certain operating system configurations

As a result, a file may appear harmless in a sandbox while exhibiting malicious behavior under different conditions.

## 10.5. Shared Infrastructure

Multiple legitimate and malicious services may use the same IP address or hosting provider.

Therefore, IP and domain relationships must be interpreted carefully.

A shared IP address does not establish that all hosted domains are controlled by the same attacker.

# 11. Best Practices for SOC Analysts

When using VirusTotal during an IOC investigation, the following practices are recommended:

**Start with Existing Reports:** Search for a file hash before uploading a sample. This may provide existing intelligence while avoiding unnecessary file submission.

**Review Individual Engine Results:** Do not rely exclusively on the detection ratio.

**Investigate Relationships:** Use the Relations section to discover connected files, domains, URLs, and IP addresses.

**Examine Behavioral Reports:** Static file information may not reveal the full functionality of a suspicious executable.

**Correlate with Internal Logs:** Compare VirusTotal findings with EDR, SIEM, DNS, proxy, and firewall records.

**Check Historical Information:** First submission and last analysis dates can provide useful context about an indicator's history.

**Validate Community Information:** Community votes and comments can provide additional intelligence but should not replace technical verification.

**Document Findings:** Record the evidence, analysis steps, and conclusions for future investigations.

**Protect Sensitive Data:** Follow organizational policies before submitting or querying potentially confidential information.

**Use Multiple Intelligence Sources:** Combine VirusTotal with platforms such as AbuseIPDB, AlienVault OTX, URLhaus, and MalwareBazaar when appropriate.

# 12. Frequently Asked Questions (FAQ)

### What is VirusTotal used for in cybersecurity?

VirusTotal is used to investigate files, URLs, domains, and IP addresses by aggregating information from multiple security engines and threat intelligence sources. It helps analysts identify suspicious indicators, investigate malware, examine network infrastructure, and enrich security incidents.

### What does a detection ratio of 0/70 mean?

It means that none of the 70 participating security engines returned a malicious verdict in the displayed analysis. However, this does not guarantee that the file or URL is safe. Newly developed malware, obfuscated files, and targeted threats may remain undetected.

### Does a detection ratio of 40/70 mean that a file is 57% malicious?

No. Detection ratios do not represent the probability that a file is malicious. They show how many engines classified the item as malicious compared to the total number of engines that returned results.

### What is the difference between Detection Ratio and Community Score?

Detection Ratio represents the aggregated classifications returned by security engines. Community Score represents reputation based on user feedback and community votes.

They are separate sources of information and should be evaluated independently.

### What is the difference between Details, Relations, and Behavior?

**Details** provides technical information about the analyzed object, such as hashes, file type, metadata, and other extracted attributes.

**Relations** identifies connections between the analyzed object and other files, IP addresses, domains, and URLs.

**Behavior** provides information about activities observed during sandbox execution, including process creation, registry modifications, file operations, and network communication.

### Can VirusTotal identify a malware family?

In many cases, yes. Antivirus engine classifications, extracted metadata, behavioral analysis, and threat intelligence associations may help identify a malware family.

However, not every report contains a reliable family classification. Different vendors may also use different naming conventions for the same malware.

### Can VirusTotal identify a C2 server?

VirusTotal may help identify potential Command and Control infrastructure through behavioral reports, contacted IP addresses, contacted domains, and related threat intelligence information.

However, a suspicious connection alone does not prove that an address is a C2 server. Additional investigation is required.

### Is VirusTotal sufficient for a complete malware analysis?

No. VirusTotal provides valuable information, but it does not replace a complete malware analysis process.

Advanced investigations may require static analysis, dynamic analysis, reverse engineering, memory analysis, and network traffic inspection using additional tools.

### Can VirusTotal be integrated into a SIEM system?

Yes. VirusTotal's API can be integrated into security workflows to enrich indicators and retrieve threat intelligence information.

Depending on the integration and subscription, analysts can automate hash lookups, IP reputation checks, domain investigations, and other enrichment operations.

### Is VirusTotal free?

VirusTotal offers free access for non-commercial use subject to its Terms of Service. Some advanced features, datasets, API capabilities, and investigation tools require paid subscriptions.

The availability of individual features also depends on the account type.

# 13. Conclusion: The Role of VirusTotal in IOC Investigations

VirusTotal is an important platform for cybersecurity analysts who need to investigate suspicious indicators and gather additional threat intelligence.

Its detection engines, technical metadata, behavioral reports, relationship analysis, community information, and API capabilities provide multiple perspectives for investigating potential threats.

However, effective IOC analysis requires more than checking whether an indicator has been flagged as malicious. Analysts must understand what each report field represents, evaluate the reliability of the available information, investigate related infrastructure, and correlate findings with internal security telemetry.

By combining VirusTotal with SIEM, EDR, Threat Intelligence, and Incident Response workflows, security teams can improve their ability to identify suspicious activities and investigate potential compromises.

Ultimately, VirusTotal should be treated as an intelligence and analysis resource that supports an analyst's investigation, rather than an automated source of absolute security verdicts.

---

# References and Official Documentation

- [VirusTotal Official Website](https://www.virustotal.com/)
- [VirusTotal Documentation](https://docs.virustotal.com/)
- [VirusTotal – How It Works](https://docs.virustotal.com/docs/how-it-works)
- [VirusTotal – Reports](https://docs.virustotal.com/docs/results-reports)
- [VirusTotal – API Getting Started](https://docs.virustotal.com/reference/getting-started)
- [VirusTotal – File Object Reference](https://docs.virustotal.com/reference/files)
- [VirusTotal – URL Object Reference](https://docs.virustotal.com/reference/url-object)
- [MITRE ATT&CK Framework](https://attack.mitre.org/)
- [AbuseIPDB](https://www.abuseipdb.com/)
- [AlienVault OTX](https://otx.alienvault.com/)
- [URLhaus](https://urlhaus.abuse.ch/)
- [MalwareBazaar](https://bazaar.abuse.ch/)
