# CYBR/CSCI 8420, Fall 2022: Assignment 2 - Requirements for SSE


Scenario
-
Our scenario represents a privately-owned bank located in a small, Nebraskan town (<500 residents). Most of the bank's clientele are local farmers and ranchers, making loans very popular. The bank staff includes a President/Owner, Vice-President/Bank Manager, Loan Manager, 1 full-time Senior Teller, and 3 part-time tellers. The information stored in the Nextcloud application must be compliant with US federal banking regulations (e.g., PCI DSS, GLBA, etc.).


Assignment Overview / Project Board
-
This document will review five essential interactions between our hypothetical bank staff and the Nextcloud application.

[Project Board](https://github.com/orgs/unosec/projects/2)


Essential Interaction #1
-
A Senior Bank Teller wants to schedule a meeting with the part-time tellers so that they can review a recent, policy change to the money-handling policy.

### System of Interest

- Any text input functions throughout the Nextcloud system (specifically reviewing Calendar input in this scenario).

### Security Requirements

- Prevent malicious content such as Javascript, HTML, external URLs, or SQL from being entered into meeting details 

![JSAssignment2](https://github.com/unosec/project/blob/main/images/JSAssignment2.png)

*Reflection - Assess the alignment of security requirements derived from misuse case analysis with advertised features of the open-source software:*

The misuse cases in this essential interaction describes low-level attacks that try to exploit developers not following programming best practices commonly identified by [OWASP](https://owasp.org/Top10/).  Product documentation would not advertise the presence of security features to handle these situations, but further investigation into the code should provide details.

Essential Interaction #2
-
The Bank would like to have its employees store electronic copies of loan applications in a secure location with proper back-up and data retention procedures.

### System of Interest

- NextCloud Files Feature

### Security Requirements

- Anti-virus scans of files
- Two-Factor Authentication
- Role-Based Access Controls 
- Audit logs of changes and actions

![image](https://github.com/unosec/project/blob/main/images/FileManagementMisUseCase.drawio.png)

*Reflection - Assess the alignment of security requirements derived from misuse case analysis with advertised features of the open-source software:*

Nextcloud supports anti-virus scans, two-factor authentication, role based access controls, and audit logging.

Essential Interaction #3
-

The Bank would like to have its employees initiate secure chat and video calls with both internal users and customers using the NextCloud Talk feature. File-sharing, during chat and video calls, is also expected. 

### System of Interest
- NextCloud Talk Feature

### Security Requirements
- Chats and video calls to remain encrypted and secure
- Password-protected meeting invites
- Prevent Mailicious file-sharing and logging of events
- Prevent DOS by limiting attendees
- Audit violators and malicious events

![image](https://github.com/unosec/project/blob/main/images/NextCloudTalk.png)


*Reflection - Assess the alignment of security requirements derived from misuse case analysis with advertised features of the open-source software:*

Nextcloud advertises a number of security features that can help prevent collaboration misuse cases while in a group call or chat. Files can be tagged and used to limit or deny sharing and access based on several factors like IP Addresses, Geo-location, and user permissions. Other security features include built-in role-based file access control, complete audit logging of all actions taken, and several supported anti-virus integrations to keep end-users safe. Many other security features are available including file encryption, comprehensive user management controls, along with end-to-end secure communication protocols for enhanced protection. 


Essential Interaction #4
-

### System of Interest
- LDAP Integration

### Security Requirements
- Protect the user's login credentials
- Prevent disclosure of login information
- Protect the server's LDAP credentials

![image](https://github.com/unosec/project/blob/1a0a11bac2f7b7d1c0e9162798b7d294099ccaf4/images/rsse-cs.png)

*Reflection - Assess the alignment of security requirements derived from misuse case analysis with advertised features of the open-source software:*

Nextcloud advertises their LDAP integration as a feature for enterprise customers. These common mis-use cases not only implact the security of the software but everything else connected to their active directory (or other backend). SSL/TLS is enforced for LDAP integrations and generic error messages are used at login to prevent information disclosure. 


Essential Interaction #5 
-
Any employee using the Mail application has access to sending and receiving emails through the client. An email being sent from an employee may be intercepted by an attacker. An email being received by an employee can include a malicious link that could include a phishing attempt or spyware.

### System of Interest
- Nextcloud Mail Application

### Security Requirements
- Anti-virus scans of the workstation
- Message encryption

![image](https://github.com/unosec/project/blob/main/images/SSE-LS.png)

*Reflection - Assess the alignment of security requirements derived from misuse case analysis with advertised features of the open-source software:*

The security requirements derived from this misuse case analysis include an Anti-virus application as well as email message encryption. Nextcloud advertises an [Anti-virus for files](https://apps.nextcloud.com/apps/files_antivirus) application that comes with the Nextcloud application. The anti-virus application "inspects files that are uploaded to Nextcloud for viruses before they are written to the Nextcloud storage". There is nothing stating that the anti-virus application scans the workstation on which Nextcloud is being used, so this may leave the computer vulnerable while protecting the Nextcloud application and server. The Nextcloud Mail Application is compatible with the [Mailvelope](https://mailvelope.com/) browser extension. This extension provides end-to-end message encryption.

One aspect that was not found advertised by Nextcloud is a spam filter option. This will be investigated further.

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
- [Logging](https://docs.nextcloud.com/server/latest/admin_manual/configuration_server/logging_configuration.html?highlight=logging)

In addition to the above list, multiple Whitepapers are available for assistance setting up NextCloud. Common papers found on their documentation were Security and Authentication, Server-side Encryption, General Architecture Overview, End-to-end Encryption, Hackerone Case study, and Nextcloud audit by Swiss Kyos.

Nextcloud appears to be a reasonably mature, software product with many security features expected in modern applications such as two-factor authentication, encryption, user management and user roles, and a configurable password policy. The documentation, viewed for this assignment, includes the necessary steps for application installation and use. We were not able to find any issues regarding security-related configuration or installation. However, as discussed in "Essential Interaction #5", a spam filter is missing from the Mail client. This would definitely be a popular addition to this particular application.

Planning and Reflection
-
The team met on Tuesday 9/20 at 5:30pm.  It was decided that each team member would handle one interaction, that Dillon would work on Part 2, and Jim would do the prototype drawing to discuss at the instructor meeting.  The team also uses Discord throughout the week to discuss items.

We feel the Nextcloud is a well-documented product and that most key security issues have probably been addressed, however we look forward to further investigation of possible security issues.  Josh noted during discuss that any security issues with the product are probably the result of improper configuration during installation or during initial setup.  For example, not implementing SSL/TLS encryption would be a significant problem with a web application such as this.
