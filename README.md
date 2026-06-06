<div align="center">

# DFIR Artifact Lab

### From artifact understanding to forensic analysis

Building practical DFIR skills through artifact validation, forensic experiments, malware analysis, and applied investigation scenarios.

[GitHub](https://github.com/somyagupta06) • [LinkedIn](https://www.linkedin.com/in/somyagupta06/) • [TryHackMe](https://tryhackme.com/p/somyaguptaa06)

</div>

---

# Overview

This repository documents my hands-on journey through Digital Forensics and Incident Response (DFIR).

The objective is not to collect notes or complete learning paths.

The objective is to understand how forensic artifacts behave, validate them through practical exercises, and then apply that knowledge to malware analysis, incident response, and forensic investigations.

Every topic in this repository follows the same workflow:

```text
Understand Artifact
        ↓
Validate Through Experiments
        ↓
Analyze Real Evidence
        ↓
Apply In Investigation Scenarios
```

---

# Repository Structure

```text
DFIR_Artifact_Lab/

├── File_System_Analysis/
│
│   ├── MBR/
│   │   ├── Validation/
│   │   └── Applied_Case/
│   │
│   ├── GPT/
│   │   ├── Validation/
│   │   └── Applied_Case/
│   │
│   ├── FAT32/
│   │   ├── Validation/
│   │   └── Applied_Case/
│   │
│   ├── NTFS/
│   │   ├── Validation/
│   │   └── Applied_Case/
│   │
│   ├── EXT/
│   │   ├── Validation/
│   │   └── Applied_Case/
│   │
│   └── File_Carving/
│       ├── Validation/
│       └── Applied_Case/
│
├── Windows_Artifacts/
│
│   ├── Event_Logs/
│   ├── Registry/
│   ├── Prefetch/
│   ├── Amcache/
│   ├── Shimcache/
│   ├── LNK_Files/
│   └── Jump_Lists/
│
├── Linux_Artifacts/
│
├── macOS_Artifacts/
│
├── Mobile_Forensics/
│
├── Memory_Analysis/
│
├── Malware_Analysis/
│
└── Applied_Cases/
```

---

# Methodology

Each topic is divided into two phases.

## Phase 1 — Validation

Understanding how an artifact works through practical exercises.

Examples:

- Exploring MFT records
- Recovering deleted files
- Examining FAT directory entries
- Parsing event logs
- Investigating registry artifacts
- Inspecting memory structures

Questions answered:

```text
What does this artifact do?
How is it created?
How is it modified?
What evidence can it provide?
How can investigators use it?
```

---

## Phase 2 — Applied Case

Applying artifact knowledge to a realistic forensic or malware-related scenario.

Examples:

- Malware persistence analysis
- User activity reconstruction
- Timeline reconstruction
- Deleted file investigations
- Endpoint compromise analysis
- Incident response scenarios

Questions answered:

```text
How does this artifact help during an investigation?
What attacker activity can it reveal?
How can it support findings with evidence?
How does it fit into the attack timeline?
```


---

# Tools Used

## Forensics

- FTK Imager
- Autopsy
- HxD
- Volatility

## Detection & Investigation

- Splunk
- Elastic Stack
- Sysmon

## Network Analysis

- Wireshark
- TShark

## Malware Analysis

- YARA
- CyberChef
- VirusTotal

---

# Why This Repository Exists

Most learning repositories show what was studied.

This repository focuses on what was validated and applied.

For every artifact:

1. Understand how it works
2. Validate it through practical exercises
3. Observe the evidence it produces
4. Apply it in an investigation scenario
5. Document findings and conclusions

The goal is to develop investigation skills by working with evidence instead of assumptions.

---

# Long-Term Goal

Build practical expertise in:

- Digital Forensics
- Incident Response
- Endpoint Investigation
- Malware Analysis
- Threat Hunting

while maintaining a documented record of hands-on work and forensic case studies.

---

<div align="center">

### Follow evidence. Validate assumptions. Document findings.

</div>
