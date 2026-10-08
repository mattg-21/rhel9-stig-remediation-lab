\# RHEL 9 STIG Remediation Lab



\## Overview



This project demonstrates the assessment, manual remediation, automation, and validation of selected DISA STIG security controls on Red Hat Enterprise Linux 9 systems.



The lab was designed to show the full security-hardening workflow rather than only the final configuration. I first established baseline systems and evaluated them with OpenSCAP. Selected findings were then remediated manually on one RHEL 9 virtual machine to understand the underlying configuration changes and validation process.



The same remediations were then recreated with Ansible on a separate RHEL 9 virtual machine. The automated configurations were tested for successful execution, idempotence, and OpenSCAP compliance.



\## Objectives



The primary goals of this project were to:



\- Build a repeatable RHEL 9 security-hardening lab.

\- Identify STIG findings using OpenSCAP.

\- Research and manually remediate selected findings.

\- Validate each remediation before moving forward.

\- Recreate the manual fixes using Ansible.

\- Test Ansible playbooks for idempotence.

\- Troubleshoot failed compliance checks and configuration conflicts.

\- Perform final OpenSCAP scans to validate the completed remediation work.

\- Maintain a clean separation between manual remediation and automated remediation.



\## Lab Architecture



The environment consisted of three RHEL 9 virtual machines hosted in Oracle VirtualBox:



| System | Purpose |

|---|---|

| Admin VM | Ansible control node and central administration system |

| Manual VM | Target used for manual STIG remediation |

| Automated VM | Target used for Ansible-based remediation |



The target systems began from the same base configuration so the manual and automated remediation approaches could be compared against similar starting environments.



\## Technologies Used



\- Red Hat Enterprise Linux 9

\- DISA STIG security guidance

\- OpenSCAP

\- SCAP Security Guide

\- Ansible Core

\- `ansible.builtin` modules

\- `ansible.posix.sysctl`

\- SSH

\- Oracle VirtualBox

\- PowerShell

\- Git / GitHub



\## Project Workflow



\### 1. Build the Lab



A reusable RHEL 9 base virtual machine was created and cloned into separate manual-remediation and automated-remediation targets.



Unique system identities, SSH host keys, hostnames, and network configurations were established after cloning.



Connectivity between the administration system and both target systems was verified before security testing began.



\### 2. Establish the Baseline



OpenSCAP and the SCAP Security Guide were installed on both target systems.



Each system was evaluated against the RHEL 9 STIG profile to establish its initial security posture and identify findings for remediation.



\### 3. Manual Remediation



Selected STIG findings were first investigated and remediated manually.



For each control, the general process was:



1\. Identify the failed OpenSCAP rule.

2\. Inspect the current system configuration.

3\. Research the required secure state.

4\. Apply the configuration change.

5\. Verify the configuration manually.

6\. Run OpenSCAP validation.

7\. Troubleshoot any failed validation before continuing.



This phase provided the configuration knowledge later used to build the Ansible automation.



\### 4. Automated Remediation



The manual remediation steps were recreated as Ansible playbooks.



The automation is separated by security area:



| Playbook | Purpose |

|---|---|

| `password\_policy.yml` | Configures password policy requirements |

| `account\_lockout.yml` | Configures account lockout behavior and faillock integration |

| `ssh\_hardening.yml` | Applies SSH security settings |

| `cron\_hardening.yml` | Restricts cron configuration permissions |

| `file\_integrity.yml` | Configures AIDE file-integrity monitoring |

| `network\_hardening.yml` | Applies IPv4 kernel/network security settings |

| `audit\_hardening.yml` | Adds audit rules for privileged configuration changes |



\### 5. Idempotence Testing



Playbooks were executed again after successful remediation.



A properly idempotent playbook should report no unnecessary changes when the system is already in the desired state.



This helped verify that the automation was suitable for repeatable configuration management rather than acting only as a one-time script.



\### 6. Troubleshooting and Validation



Not every remediation passed OpenSCAP on the first attempt.



Some controls required additional investigation when the configured state and the compliance scan did not initially agree. These situations were used to inspect configuration precedence, identify conflicting settings, and modify the remediation approach.



For example, SSH configuration required accounting for an existing Red Hat configuration file that could conflict with the desired X11 forwarding setting.



\### 7. Final Validation



After all selected remediations were complete, final OpenSCAP scans were performed on both the manually hardened and Ansible-managed systems.



The final validation demonstrated that the targeted controls could be remediated through both approaches.



\## Repository Structure



```text

rhel9-stig-remediation-lab/

├── README.md

├── evidence/

│   ├── automated-remediation/

│   └── manual-remediation/

├── inventory/

│   └── inventory.example.ini

└── playbooks/

&#x20;   ├── account\_lockout.yml

&#x20;   ├── audit\_hardening.yml

&#x20;   ├── cron\_hardening.yml

&#x20;   ├── file\_integrity.yml

&#x20;   ├── network\_hardening.yml

&#x20;   ├── password\_policy.yml

&#x20;   └── ssh\_hardening.yml
