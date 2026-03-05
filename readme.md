# Windows Server 2022 Enterprise Lab & Security Assessment

## Overview
This repository documents the end-to-end deployment, configuration, and security analysis of a Windows Server 2022 domain environment. Built within a controlled virtualized lab, this project simulates a real-world enterprise infrastructure deployment from the ground up.

The project demonstrates foundational systems administration and network engineering skills, including Active Directory Domain Services (AD DS) setup, DNS and DHCP configuration, Group Policy Object (GPO) deployment, and Layer 2 network hardening using Cisco switches. 

Following the initial deployment, a comprehensive **Vulnerability Assessment** was conducted to identify architectural flaws, misconfigurations, and procedural risks, aligning the lab environment with Microsoft's Enterprise Access Model and security best practices.

## Repository Contents

- **[Project Writeup & Implementation Guide](windows-server-2022-project.md)**  
  *The complete step-by-step documentation of the lab deployment, including virtual machine preparation, server setup, logical AD structure, and switch configurations.*

- **[Vulnerability Assessment](vulnerability-assessment.md)**  
  *A detailed security review of the deployed environment, highlighting lab-induced vulnerabilities, Tier 0 isolation risks, and actionable remediation steps for a production-ready deployment.*

## Key Technologies & Skills Demonstrated
- **Operating Systems:** Windows Server 2022
- **Identity & Access Management:** Active Directory (AD DS), Organizational Units (OUs), Security Groups
- **Core Network Services:** DNS, DHCP
- **Endpoint Management:** Group Policy Objects (GPOs), Software Deployment, Security Enforcement
- **Network Security:** Cisco Switch Configuration, VLAN Segmentation, Port Security, Spanning-Tree (BPDU Guard)
- **Security Analysis:** Microsoft Security Baselines, Tiered Administration Architecture, Defensive Infrastructure
