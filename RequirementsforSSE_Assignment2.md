# CYBR/CSCI 8420, Fall 2022: Assignment 2 - Requirements for SSE


Scenario
-
Our scenario represents a privately-owned bank located in a small, Nebraskan town (<500 residents). Most of the bank's clientele are local farmers and ranchers, making loans very popular. The bank staff includes a President/Owner, Vice-President/Bank Manager, Loan Manager, 1 full-time Senior Teller, and 3 part-time tellers. The information stored in the Nextcloud application must be compliant with US federal banking regulations (e.g., PCI DSS, GLBA, etc.).


Assignment Overview
-
This document will review five essential interactions between our hypothetical bank staff and the Nextcloud application.


Essential Interaction #1
-

### User Story

- As a Senior Bank Teller, I want to schedule a meeting with the part-time tellers so that we can review a recent policy change to our money handling policy.

### System of Interest

- Calendar system

### Security Requirements

- Meeting details must remain private in transit over the network
- Adding participants should not affect system performance or cause an error displaying sensitive information
- Prevent malicious content such as Javascript, HTML, or external URLs from being entered in meeting details  

![JSAssignment2](https://github.com/unosec/project/blob/main/images/JSAssignment2.png)

Essential Interaction #2
-

### System of Interest

- NextCloud Files Feature

### Security Requirements

- Anti-virus scans of files
- Two-Factor Authentication
- Role Based Access Controls 
- Audit logs of changes and actions

![JSAssignment2](https://github.com/unosec/project/blob/main/images/FileManagementMisUseCase.drawio.png)


Essential Interaction #3

The Bank would like to have its employees initiate secure chat and video calls with both internal users and customers using the NextCloud Talk feature. Collaboration can happen with files while in chat and calls securely. 

System of Interest
- NextCloud Talk Feature

Security Requirements
- Chats and video calls should remain encrypted and secure.
- Password protected meeting invites.
- Prevent Mailicious file sharing and logging of events.
- Prevent DOS by adding too many attendees.

![image](https://github.com/unosec/project/blob/main/images/NextCloudTalk.png)



*Assess the alignment of security requirements derived from misuse case analysis with advertised features of the open-source software:*

Nextcloud advertises a number of security features that can help prevent file management misuse cases. Such as their built-in role-based file access control, complete audit log of all actions taken, and several supported anti-virus integrations to keep end users safe. Many other security features are available including file encryption, comprehensive user management controls, along with end-to-end secure communication protocols for enhanced protection. 

Part 2 
-

Review OSS project documentation for security-related configuration and installation issues. Summarize your observations.
-

Extensive documentation is available explaining Nextcloud security features and abilities. In-depth descriptions are available for following best practices, alternative solutions for common problems, multiple options for meeting basic security requirements, along with pre-written documents for audit evidence proving NextCloud’s compliance with common regulations. Below is a short list of security related documents publicly available. 

- [Managing User Password Policies](https://docs.nextcloud.com/server/latest/admin_manual/configuration_user/user_password_policy.html)
- [Two-factor Authentication](https://docs.nextcloud.com/server/latest/admin_manual/configuration_user/two_factor-auth.html)
- [File Encryption Configuration](https://docs.nextcloud.com/server/latest/admin_manual/configuration_files/encryption_configuration.html)
- [NextCloud Hardening and Security Guidance](https://docs.nextcloud.com/server/latest/admin_manual/installation/harden_server.html)
- [NextCloud Antivirus Scanner](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/antivirus_configuration.html)
- [Configuration of Admin Privileges](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/admin_delegation_configuration.html) 
- [API Best Practices](https://docs.nextcloud.com/server/latest/admin_manual/apps_management.html#using-private-api)
- [File Access Controls](https://docs.nextcloud.com/server/latest/admin_manual/file_workflows/access_control.html)
- [File Retention Policies](https://docs.nextcloud.com/server/latest/admin_manual/file_workflows/retention.html)
- [Common Misconfigurations](https://docs.nextcloud.com/server/latest/admin_manual/file_workflows/retention.html#common-misconfigurations)
- [GDPR Compliance](https://docs.nextcloud.com/server/latest/admin_manual/gdpr/cookies.html)

In addition to the above list, multiple Whitepapers are available for assistance setting up NextCloud. Common papers found on their documentation were Security and Authentication, Server-side Encryption, General Architecture Overview, End-to-end Encryption, Hackerone Case study, and Nextcloud audit by Swiss Kyos.
