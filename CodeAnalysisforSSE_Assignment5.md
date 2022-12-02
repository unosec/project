# CYBR/CSCI 8420, Fall 2022: Assignment 5 - Designing for SSE


Assignment Overview / Project Board
-
This document reviews our code review and analysis results.

The scenarios we continue to focus on are listed in the Scenarios sections.


Scenarios
-

- The Nextcloud calendar form does not allow injection exploits.
- Nextcloud sufficiently prevents unauthorized file access. 
- Nextcloud prevents malicious file uploads.
- Nextcloud does not leak authentication information.
- The Nextcloud Mail app is acceptably secure.


Code Review Strategy
-

Findings - Manual Code Review
-

Findings - Automated Code Review
-
<h3><a href="https://unosec.github.io/2022-11-22-ZAP-Report.html" target="_blank">ZAP report for Nextcloud server</a> </h3>

<h3>SonarQube Findings Review</h3>

**Finding 1:**

![image](https://github.com/unosec/project/blob/main/images/UserManagement.png)

An minor issue with NextCloud’s UserManagement.php file was discovered related to CWE-478 (Missing Default Case in Switch Statement). While this typically isn’t a critical issue, the potential for unintended impact and misconfigurations is increased due to this section of code controlling the audit logging of NextCloud user’s and their activities while using the system. In previous assignments, we recognized NextCloud’s auditing and logging as a critical security component due to its ability to detect and monitor potential threats and attack vectors. 


**Finding 2:**

![image](https://github.com/unosec/project/blob/main/images/InstalledVersions.png)

NextCloud’s InstalledVersions.php contained bug alerts in our automated code scans due to its usage of “Require” instead of “Require_Once”. This code section is utilized for implementing the encryption of files saved via NextCloud’s file feature. File encryption was documented as a critical security function preventing unauthorized access and several other mis-use cases. While the difference between “Require” and “Require_Once“ is minor, the latter option would only generate warnings if improper vendor installed encryption mechanisms were implemented while the current code could generate fatal errors impacting both the confidentiality and availability of NextCloud’s file features. 


**Finding 3:**

![image](https://github.com/unosec/project/blob/main/images/config.png)

NextCloud’s config.php contained bug notifications in scan results due to its usage of “Include” instead of “Include_Once”. This code section is utilized for reading the configuration file and current settings of the NextCloud server. Similar to previous findings of “Require” and Require_Once” the finding isn’t severe, but could impact security configuration settings being read and properly implemented if NextCloud should generate an error during this code’s execution. 


**Finding 4:**

![image](https://github.com/unosec/project/blob/main/images/FTPConnection.png)

The FtpConnection.php file contains code that specifies “ftp_connect” instead of “ftp_ssl-Connect” This code section is for NextCloud’s External file storage and could cause unencrypted file transfers to occur. We previously documented that secure file transfers are a critical security requirement in order to keep data confidentially and prevent eavesdropping attacks. 


Summary of Key Findings
-


<h3><a href="https://github.com/orgs/unosec/projects/5" target="_blank">Project Board</a> </h3>

<h3><a href="https://github.com/unosec/project/issues" target="_blank">Project Issues</a> </h3>


Contributions - OSS project pull requests, issues, discussions
-

