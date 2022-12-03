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

The group decided we would focus on looking for issues in the core Nextcloud server code base, as well as the two plugins used to for the calendar and mail functionality.  The goal would be be to find issues, specifically but not limited to those related to our 5 use case and assurance scenarios.  Additionally, but taking an assurance approach, we also looked to seek evidence of proper coding strategies which would then indicate best practices are understood and utilized.

For automated review, the team primarily used [SonarQube/SonarSource](https://www.sonarqube.org/) on both Linux and Windows.  This is considered to be [one of the best tools for PHP static code analysis](https://phpmagazine.net/2020/10/top-php-security-and-malware-scanners.html).  A summary of the SonarQube process on Windows is listed in Appendix A.  As a secondary check to support the lack of significant findings, ZAP was also used.

For manual review, the Linux grep utility was used to search the codebase.



Findings - Manual Code Review
-

Often database-driven website software is susceptible to the [SQL Injection weakness - CWE-89](https://cwe.mitre.org/data/definitions/89.html) or the [HTML/Javascript injection attacks - CWE-74](https://cwe.mitre.org/data/definitions/74.html) weakness.  By taking a software assurance approach, we decided to investigate if we could find evidence that the software, specifically the calendar functionality, was **not** susceptible to these weaknesses.

In PHP code, a developer should use the "htmlspecialchars" function when outputting data to a web page.   

- ```grep -r htmlspecialchars```

We found this function in use on line 78 of [this file](https://github.com/nextcloud/3rdparty/blob/master/sabre/dav/lib/DAV/Browser/HtmlOutputHelper.php).

In searching the main codebase, we also found this function used in files such as Setup.php, Manager.php, OC_Util.php, and EMailTemplate.php.

This evidence would support that efforts have been made to mitigate CWE-89.

For SQL Injection, one method a developer should use is parameterized queries.  We searched the code for SQL-like syntax:

- ```grep -r "INSERT INTO"```

In reviewing, what appears to be, the [data access code](https://github.com/nextcloud/3rdparty/blob/020d0d3892bd3b7296db8ed21448c834d33d5723/sabre/dav/lib/CalDAV/Backend/PDO.php) for the CalDAV functionality, the system does appear to use parameterized queries. Lines 253-254:
 
- ```$stmt = $this->pdo->prepare('INSERT INTO '.$this->calendarTableName.' (synctoken, components) VALUES (1, ?)'); $stmt->execute([$components]);```

In searching the main codebase, evidence of parameterized queries was found in "apps\files_sharing\lib\External\Manager.php".  An instance of parameterized queries **not** being used was found in file "\lib\private\DB\Adapter.php", however this method was marked as deprecated.

Findings - Automated Code Review
-

<h3>SonarQube Findings Review</h3>

**[Finding 1:](https://potato.0x69.xyz/project/issues?files=apps%2Fadmin_audit%2Flib%2FActions%2FUserManagement.php&resolved=false&id=nextcloud&open=AYSheR9xx29KzD1GMcyt)**

![image](https://github.com/unosec/project/blob/main/images/UserManagement.png)

An minor issue with NextCloud’s UserManagement.php file was discovered related to [CWE-478 (Missing Default Case in Switch Statement)](https://cwe.mitre.org/data/definitions/478.html). While this typically isn’t a critical issue, the potential for unintended impact and misconfigurations is increased due to this section of code controlling the audit logging of NextCloud user’s and their activities while using the system. In previous assignments, we recognized NextCloud’s auditing and logging as a critical security component due to its ability to detect and monitor potential threats and attack vectors. 


**[Finding 2:](https://potato.0x69.xyz/project/issues?files=apps%2Fencryption%2Fcomposer%2Fcomposer%2FInstalledVersions.php&resolved=false&id=nextcloud&open=AYSheSwVx29KzD1GMeCp)**

![image](https://github.com/unosec/project/blob/main/images/InstalledVersions.png)

NextCloud’s InstalledVersions.php contained bug alerts in our automated code scans due to its usage of “Require” instead of “Require_Once”. This code section is utilized for implementing the encryption of files saved via NextCloud’s file feature. File encryption was documented as a critical security function preventing unauthorized access and several other mis-use cases. While the difference between “Require” and “Require_Once“ is minor, the latter option would only generate warnings if improper vendor installed encryption mechanisms were implemented while the current code could generate fatal errors impacting both the confidentiality and availability of NextCloud’s file features. 


**[Finding 3:](https://potato.0x69.xyz/project/issues?files=lib%2Fprivate%2FConfig.php&resolved=false&id=nextcloud&open=AYSheUOox29KzD1GMhPz)**

![image](https://github.com/unosec/project/blob/main/images/config.png)

NextCloud’s config.php contained bug notifications in scan results due to its usage of “Include” instead of “Include_Once”. This code section is utilized for reading the configuration file and current settings of the NextCloud server. Similar to previous findings of “Require” and Require_Once” the finding isn’t severe, but could impact security configuration settings being read and properly implemented if NextCloud should generate an error during this code’s execution. 

**[Finding 5:](https://potato.0x69.xyz/security_hotspots?id=nextcloud&hotspots=AYSheRrrx29KzD1GMcQ0)**

![image](https://github.com/unosec/project/blob/main/images/FTPConnection.png)

The FtpConnection.php file contains code that specifies “ftp_connect” instead of “ftp_ssl-Connect” This code section is for NextCloud’s External file storage and could cause unencrypted file transfers to occur. We previously documented that secure file transfers are a critical security requirement in order to keep data confidentially and prevent eavesdropping attacks. 

**Finding 6:**

![image](https://github.com/unosec/project/blob/main/images/JimTest1.png)

The calendar plug-in was scanned using SonarScanner.  No vulnerabilities were found.  Numerous "code smells" indicating possible coding style improvements were identified.

<h3><a href="https://unosec.github.io/2022-11-22-ZAP-Report.html" target="_blank">ZAP 
report for Nextcloud server</a> </h3>


Summary of Key Findings
- 

We analyzed the code in relation to the following weaknesses:

- [CWE-478: Missing Default Case in Multiple Condition Expression](https://cwe.mitre.org/data/definitions/478.html)
- [CWE-89: Improper Neutralization of Special Elements used in an SQL Command ('SQL Injection')](https://cwe.mitre.org/data/definitions/89.html)
- [CWE-74: Improper Neutralization of Special Elements in Output Used by a Downstream Component ('Injection')](https://cwe.mitre.org/data/definitions/74.html)

For CWE-89 and CWE-87 we performed manual reviews.  By taking a software assurance approach, we decided to investigate if we could find evidence supporting that the software, specifically the calendar functionality, was **not** susceptible to these weaknesses.  We found evidence that best practices for mitigating these weaknesses appeared to be in use.

For CWE-478 we performed an automated review as summarized under Finding 1.  A missing default cause was found in UserManagement.php.

An automated scan of the codebase for the calendar functionality found no vulnerabilities.

<h3><a href="https://github.com/orgs/unosec/projects/5" target="_blank">Project Board</a> </h3>

<h3><a href="https://github.com/unosec/project/issues" target="_blank">Project Issues</a> </h3>


Contributions - OSS project pull requests, issues, discussions
-

[team notes - possibly open github issues on the nextcloud project to suggest fixing issues found in findings #1 or findings #5 - especially #5 - or indicate why we chose not to do so]

Appendix A:  Using SonarQube on Windows
-

SonarQube offers a [community edition](https://docs.sonarqube.org/latest/setup/get-started-2-minutes/) with a docker implementation.  This approach seemed appropriate for this project.

Docker must first be installed on Windows.  This was done by following [these instructions](https://docs.docker.com/desktop/install/windows-install/).  Note that this also required WSL2 ([Windows subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/install)) to be installed on the PC.

After docker desktop is installed, the SonarQube docker images must be installed.  The first image installed was the base SonarQube website image.  This was installed and ran by entering the following commands in powershell:

- ```docker pull sonarqube```
- ```docker run -d --name sonarqube -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:latest```

Once installed, the SonarQube website could be accessed by going to http://localhost:9000.  Following the SonarQube documentation, a "project" was then created in the SonarQube website.  This effectively provided a bucket for the results of the security analysis performed in the next step.

Next, the SonarScanner software needed to be installed to provide the specific functionality to scan PHP files.  This was installed by entering the following commands in powershell:

- ```docker pull sonarsource/sonar-scanner-cli```

In order to then perform a scan, I did the following:

- download the project to scan from github to a local folder.  In this case the folder was: C:\temp\nextcloud\3rdparty\sabre\dav\lib\CalDAV

- ```docker run -d --name sonarqube -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:latest```

Run the following SonarScanner command in powershell:

- ```docker run --rm -e SONAR_HOST_URL="http://172.31.144.1:9000/" -e SONAR_SCANNER_OPTS="-Dsonar.projectKey=JimTest1" -e SONAR_LOGIN="sqp_71e6b2ac956b3da33d972f36f5d0f06b6fbb36df" -v "C:\temp\nextcloud\3rdparty\sabre\dav\lib\CalDAV:/usr/src" sonarsource/sonar-scanner-cli```

The project key and the SONAR_LOGIN were both values provided by the SonarQube website when I created a test project.  **Two other important details**:

- the SONAR\_HOST\_URL value could not reference "localhost".  In order for one docker image to communicate to the other docker image, and IP address value from "ipconfig" needed to be used.
- the parameter -v "C:\temp\nextcloud\3rdparty\sabre\dav\lib\CalDAV:/usr/src" is important and it links a local folder to an internal folder used by the scanner software in the docker image.

After running the SonarScanner docker command above, the scan ran for approximately 6 minutes.  When the scan was complete assessment details were available in the SonarQube website.

