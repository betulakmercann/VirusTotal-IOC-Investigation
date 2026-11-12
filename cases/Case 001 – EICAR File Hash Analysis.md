# Case 001 – EICAR File Hash Analysis

## 1. Case Overview

In this case study, an EICAR test file was analyzed using its SHA-256 hash value on the VirusTotal platform. The objective was to examine the file's detection results, investigate YARA rule matches, evaluate associated domains, and interpret MITRE ATT&CK mappings.

**File Name:** eicar.pdf  
**File Size:** 68 Bytes  
**Indicator Type:** SHA-256  
**Detection Ratio:** 66/68  
**Threat Level:** Low  
**Final Assessment:** Harmless – EICAR Test File

## 2. Detection Analysis

The VirusTotal analysis showed that 66 out of 68 security engines flagged the file as malicious.

Despite this high detection ratio, the result does not indicate that the file is an actual malware sample. EICAR is a standardized test file designed to evaluate antivirus detection mechanisms. It allows security products to be tested under controlled conditions without containing actual malicious code.

Therefore, the detection ratio was evaluated in the context of the file's known purpose as an EICAR test sample.

## 3. YARA Rule Analysis

The following YARA rules were observed during the file analysis:

- `malw_eicar`
- `MULTI_EICAR_ac8f42d6`
- `SUSP_Just_EICAR`

YARA is a rule-based pattern-matching system used to identify files through specific content, byte sequences, and patterns.

These matches indicate that the file contains patterns associated with known EICAR test content. In particular, the rule names containing EICAR support the identification of the file as an antivirus test sample.

## 4. Relations Analysis

Several domains associated with the file were examined under the Relations tab in VirusTotal.

The majority of the analyzed domains showed a detection ratio of 0/92, while one domain had a detection ratio of 1/92.

These values represent how the corresponding domains are classified by security vendors on VirusTotal:

- **0/92:** None of the 92 security vendors flagged the domain as malicious.
- **1/92:** One out of 92 security vendors flagged the domain as malicious.

The associated domains included domains belonging to major technology companies such as Apple, Adobe, Akamai, and Microsoft.

The presence of these domains in the Relations section does not necessarily indicate malicious activity or prove that the EICAR file performed any attacks involving these companies. The nature and origin of each relationship should be investigated separately.

Based on the available findings, the majority of the associated domains did not show negative reputation indicators.

## 5. Behavior Analysis – MITRE ATT&CK

The VirusTotal Behavior tab displayed several tactics and techniques associated with the MITRE ATT&CK framework.

The following technique identifiers (Technique IDs) were observed during the analysis:

| Technique ID | Technique Name | Tactic |
|---|---|---|
| T1059 | Command and Scripting Interpreter | Execution |
| T1064 | Scripting (Legacy) | Execution |
| T1106 | Native API | Execution |
| T1547 | Boot or Logon Autostart Execution | Persistence, Privilege Escalation |
| T1055 | Process Injection | Defense Evasion, Privilege Escalation |
| T1036 | Masquerading | Defense Evasion |
| T1057 | Process Discovery | Discovery |
| T1082 | System Information Discovery | Discovery |
| T1083 | File and Directory Discovery | Discovery |
| T1574 | Hijack Execution Flow | Persistence, Privilege Escalation, Defense Evasion |
| T1497 | Virtualization/Sandbox Evasion | Defense Evasion |
| T1564 | Hide Artifacts | Defense Evasion |
| T1518 | Software Discovery | Discovery |

### 5.1 Technique Analysis

**T1059 – Command and Scripting Interpreter**

Refers to the execution of commands or scripts through command-line interfaces and scripting interpreters. PowerShell, Windows Command Shell, and similar interpreters fall within this category.

**T1064 – Scripting (Legacy)**

A scripting technique used in older versions of the MITRE ATT&CK framework. It has since been deprecated in the current ATT&CK structure, with related behaviors incorporated into broader techniques such as T1059.

**T1106 – Native API**

Refers to the direct use of native operating system API functions to perform system operations.

**T1547 – Boot or Logon Autostart Execution**

Describes techniques that allow programs to execute automatically during system startup or user logon. It is associated with the Persistence and Privilege Escalation tactics.

**T1055 – Process Injection**

Refers to techniques in which code is injected into another process's memory space or executed through another process. It is associated with Defense Evasion and Privilege Escalation.

**T1036 – Masquerading**

Describes methods used to make a file, process, or other system component appear legitimate in order to conceal its actual identity or purpose.

**T1057 – Process Discovery**

Refers to identifying processes and applications currently running on a system.

**T1082 – System Information Discovery**

Describes the collection of information about an operating system, system version, hardware, and other system characteristics.

**T1083 – File and Directory Discovery**

Refers to searching for files, directories, and file paths within a system.

**T1574 – Hijack Execution Flow**

Describes techniques that modify or hijack the normal execution flow of an application to execute code.

**T1497 – Virtualization/Sandbox Evasion**

Covers techniques used to identify virtual machines or sandbox environments and potentially evade analysis within those environments.

**T1564 – Hide Artifacts**

Refers to methods used to conceal files, processes, or other system artifacts from security tools and users.

**T1518 – Software Discovery**

Describes techniques used to identify installed software and applications on a system.

### 5.2 MITRE ATT&CK Findings and Limitations

The techniques listed above represent MITRE ATT&CK mappings associated with the VirusTotal Behavior report. Their presence does not prove that every technique was actually executed by the analyzed file.

To determine whether these mappings are based on directly observed behavior or potential behavioral associations, the corresponding sandbox activities should be examined. Relevant areas include process execution, file system activity, registry operations, and network communications.

Considering the known purpose of the EICAR test file, these technique mappings should not be interpreted as evidence of an actual attack chain.

## 6. Threat Assessment

The findings from the analysis can be summarized as follows:

- The file was flagged as malicious by 66 out of 68 security engines.
- Three YARA rule matches were observed.
- Most associated domains had a detection ratio of 0/92.
- One associated domain had a detection ratio of 1/92.
- A total of 13 unique MITRE ATT&CK technique identifiers were listed.
- The file is identified as an EICAR test file.

Despite the high detection ratio, the file's known purpose as a test sample was the determining factor in the final assessment.

**Final Threat Assessment: Low – Harmless Test File**

## 7. Analyst Conclusion

In this case study, file analysis was performed on VirusTotal using a SHA-256 hash value. Findings from the Detection, YARA Rules, Relations, and Behavior sections were examined to evaluate the file from a security perspective.

The analysis demonstrated that a high detection ratio alone does not necessarily indicate the presence of actual malware. It also highlighted the importance of distinguishing MITRE ATT&CK mappings from directly observed malicious behavior.

The EICAR sample served as a suitable starting point for understanding antivirus detection mechanisms and learning the IOC analysis workflow within VirusTotal.
