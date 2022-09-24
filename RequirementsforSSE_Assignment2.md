# CYBR/CSCI 8420, Fall 2022: Assignment 2 - Requirements for SSE


Scenario
-
Our scenario represents a privately-owned bank located in a small, Nebraskan town (<500 residents). Most of the bank's clientele are local farmers and ranchers, making loans very popular. The bank staff includes a President/Owner, Vice-President/Bank Manager, Loan Manager, 1 full-time Senior Teller, and 3 part-time tellers. The information stored in the Nextcloud application must be compliant with US federal banking regulations (e.g., PCI DSS, GLBA, etc.).


Assignment Overview / Project Board
-
This document will review five essential interactions between our hypothetical bank staff and the Nextcloud application.

[Project Board](https://github.com/orgs/unosec/projects/7/views/1)


Essential Interaction #1
-
As a Senior Bank Teller, I want to schedule a meeting with the part-time tellers so that we can review a recent policy change to our money handling policy.

### System of Interest

- Any text input functions throughout system, specifically reviewing Calendar details input in this case

### Security Requirements

- Prevent malicious content such as Javascript, HTML, external URLs, or SQL from being entered in meeting details 

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
- Role Based Access Controls 
- Audit logs of changes and actions

![image](https://github.com/unosec/project/blob/main/images/FileManagementMisUseCase.drawio.png)

*Reflection - Assess the alignment of security requirements derived from misuse case analysis with advertised features of the open-source software:*

Nextcloud supports anti-virus scans, two-factor authentication, role based access controls, and audit logging.

Essential Interaction #3 [Input from Tabot]
-

The Bank would like to have its employees initiate secure chat and video calls with both internal users and customers using the NextCloud Talk feature. Collaboration can happen with files while in chat and calls securely. 

### System of Interest
- NextCloud Talk Feature

### Security Requirements
- Chats and video calls should remain encrypted and secure.
- Password protected meeting invites.
- Prevent Mailicious file sharing and logging of events.
- Prevent DOS by limiting attendees.

![image](https://github.com/unosec/project/blob/main/images/NextCloudTalk.png)


*Reflection - Assess the alignment of security requirements derived from misuse case analysis with advertised features of the open-source software:*

Nextcloud advertises a number of security features that can help prevent file management misuse cases. Such as their built-in role-based file access control, complete audit log of all actions taken, and several supported anti-virus integrations to keep end users safe. Many other security features are available including file encryption, comprehensive user management controls, along with end-to-end secure communication protocols for enhanced protection. 


Essential Interaction #4 [Input from Josh]
-

### System of Interest
- Calendar Feature

### Security Requirements
- Make sure adding a large number of participants doesn't cause an error resulting in the system displaying internal information, such as the server IP address

![image](https://github.com/unosec/project/blob/main/images/JSAssignment2d.png)

*Reflection - Assess the alignment of security requirements derived from misuse case analysis with advertised features of the open-source software:*

We would want to review the code or configuration to make sure error messages aren't exposed.


Essential Interaction #5 
-

### System of Interest
- Browser based communication to Nextcloud

### Security Requirements
- Ensure all browser based network communication while using the Nextcloud app are adequately encrypted

![image](https://github.com/unosec/project/blob/main/images/JSAssignment2c.png)

*Reflection - Assess the alignment of security requirements derived from misuse case analysis with advertised features of the open-source software:*

Customer implementing Nextcloud software will need to implement SSL/TLS to secure communication.

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

In summary, Nextcloud appears to be a reasonably mature software product with many security features expected in modern applications such as two-factor authentication, encryption, user management and user roles, and a configurable password policy.

Planning and Reflection
-
The team met on Tuesday 9/20 at 5:30pm.  It was decided that each team member would handle one interaction, that Dillon would work on Part 2, and Jim would do the prototype drawing to discuss at the instructor meeting.  The team also uses Discord throughout the week to discuss items.

We feel the Nextcloud is a well documented product and that most key security issues have probably been addressed, however we look forward to further investigation of possible security issues.  Josh noted during discuss that any security issues with the product are probably the result of improper configuration during installation or during initial setup.  For example, not implementing SSL/TLS encryption would be a significant problem with a web application such as this.
