# CYBR/CSCI 8420, Fall 2022: Assignment 5 - Designing for SSE


### Assignment Overview / Project Board
This document provides an overview of our code review strategy, as well as findings from our manual code review of critical security functions and findings from our automated code scanning. The code review is of [Nextcloud](https://github.com/nextcloud) our semester-long, open-source software (OSS) project.

[Project Board](https://github.com/orgs/unosec/projects/5)


### Scenario
Our scenario represents a privately-owned bank located in a small, Nebraskan town (<500 residents). Most of the bank's clientele are local farmers and ranchers, making loans very popular. The bank staff includes a President/Owner, Vice-President/Bank Manager, Loan Manager, 1 full-time Senior Teller, and 3 part-time tellers. The information stored in the Nextcloud application must be compliant with US federal banking regulations (e.g., PCI DSS, GLBA, etc.).


### Code Review Strategy
After reviewing the misuse cases created in [Assignment 2](/RequirementsforSSE_Assignment2.md), the assurance claims from [Assignment 3](/AssuranceCasesforSSE_Assignment3.md), and the threat model strategies analyzed in [Assignment 4](/DesigningforSSE_Assignment4.md), an overview of the [OSS weaknesses](/automated_code_review/CodeWeaknesses.md) was created. A number of common CWEs were chosen based on analyzing the list of OSS weaknesses. These CWEs were used to narrow the searches and improve the code review techniques. The following CWEs were identified as highly impactful, thus warranting manual and automated code review to verify their non-existence:

- CWE-22: Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')
- CWE-78: Improper Neutralization of Special Elements used in an OS Command ('OS Command Injection')
- CWE-94: Improper Control of Generation of Code ('Code Injection')
- CWE-125: Out-of-bounds Read
- CWE-276: Incorrect Default Permissions
- CWE-287: Improper Authentication
- CWE-434: Unrestricted Upload of File with Dangerous Type
- CWE-798: Use of Hard-coded Credentials
- CWE-862: Missing Authorization


### Manual Code Review
The critical portions of code, manually reviewed, included file management, user management, encryption, and security controls implemented by Nextcloud. These code sections were chosen due to their importance in security compliance and their ability to prevent the documented misuse cases. Because of the severity of a possible vulnerability, focus was placed on the following assurance claims:
- Nextcloud sufficiently prevents malicious file uploads.
- NextCloud does not leak authentication information.
- The Nextcloud calendar form does not allow injection exploits.

For manual review, the Linux grep utility was used to search the codebase.

Often database-driven website software is susceptible to [CWE-89: SQL Injection weakness](https://cwe.mitre.org/data/definitions/89.html) or [CWE-74: HTML/Javascript injection attacks](https://cwe.mitre.org/data/definitions/74.html). By taking a software assurance approach, investigation was taken into whether the software, specifically the calendar functionality, was susceptible or not to these weaknesses.

In PHP code, a developer should use the "htmlspecialchars" function when outputting data to a web page. This function will escape the output and help prevent cross-site scripting attacks. 

```grep -r htmlspecialchars```

This function was found being used [on line 78](https://github.com/nextcloud/3rdparty/blob/master/sabre/dav/lib/DAV/Browser/HtmlOutputHelper.php).

In searching the Server code, this function was also used in files such as Setup.php, Manager.php, OC_Util.php, and EMailTemplate.php.

This evidence would support that efforts have been made to mitigate CWE-89.

For SQL Injection, one method a developer should use is parameterized queries.  The code was searched for SQL-like syntax like:

```grep -r "INSERT INTO"```

In reviewing (what seems to be) the [data access code](https://github.com/nextcloud/3rdparty/blob/020d0d3892bd3b7296db8ed21448c834d33d5723/sabre/dav/lib/CalDAV/Backend/PDO.php) for the CalDAV functionality, the system appears to use parameterized queries. Lines 253-254:
 
```$stmt = $this->pdo->prepare('INSERT INTO '.$this->calendarTableName.' (synctoken, components) VALUES (1, ?)'); $stmt->execute([$components]);```

In searching the Server code, evidence of parameterized queries was found in file _\apps\files_sharing\lib\External\Manager.php_. An instance of parameterized queries **not** being used was found in file _\lib\private\DB\Adapter.php_, however this method was marked as deprecated.


### Automated Code Review
The Nextcloud Server code, as well as the code used for the Calendar, Talk, and Mail apps were scanned and reviewed.  

For automated review, [SonarQube/SonarSource](https://www.sonarqube.org/) was used on both Linux and Windows.  This is considered to be [one of the best tools for PHP static code analysis](https://phpmagazine.net/2020/10/top-php-security-and-malware-scanners.html).  A description of the Windows SonarQube installation and use is listed in [Appendix A](https://github.com/unosec/project/blob/main/CodeAnalysisforSSE_Assignment5.md#appendix-a--using-sonarqube-on-windows). [ZAP](https://www.zaproxy.org/) was used as a secondary scanning tool. Because ZAP is a web application scanner, [the results](https://unosec.github.io/2022-11-22-ZAP-Report.html) were not as helpful so all of our time was spent going through the SonarQube findings instead.

##### SonarQube Scan Review

**[Finding 1:](https://potato.0x69.xyz/project/issues?files=apps%2Fadmin_audit%2Flib%2FActions%2FUserManagement.php&resolved=false&id=nextcloud&open=AYSheR9xx29KzD1GMcyt)**

![image](https://github.com/unosec/project/blob/main/images/UserManagement.png)

An minor issue with NextCloud’s UserManagement.php file was discovered related to [CWE-478 (Missing Default Case in Switch Statement)](https://cwe.mitre.org/data/definitions/478.html). While this typically isn’t a critical issue, the potential for unintended impact and misconfigurations is increased due to this section of code controlling the audit logging of NextCloud user’s and their activities while using the system. In previous assignments, we recognized NextCloud’s auditing and logging as a critical security component due to its ability to detect and monitor potential threats and attack vectors. 


**[Finding 2:](https://potato.0x69.xyz/project/issues?files=apps%2Fencryption%2Fcomposer%2Fcomposer%2FInstalledVersions.php&resolved=false&id=nextcloud&open=AYSheSwVx29KzD1GMeCp)**

![image](https://github.com/unosec/project/blob/main/images/InstalledVersions.png)

NextCloud’s InstalledVersions.php contained bug alerts in our automated code scans due to its usage of “Require” instead of “Require_Once”. This code section is utilized for implementing the encryption of files saved via NextCloud’s file feature. File encryption was documented as a critical security function preventing unauthorized access and several other mis-use cases. While the difference between “Require” and “Require_Once“ is minor, the latter option would only generate warnings if improper vendor installed encryption mechanisms were implemented while the current code could generate fatal errors impacting both the confidentiality and availability of NextCloud’s file features. 


**[Finding 3:](https://potato.0x69.xyz/project/issues?files=lib%2Fprivate%2FConfig.php&resolved=false&id=nextcloud&open=AYSheUOox29KzD1GMhPz)**

![image](https://github.com/unosec/project/blob/main/images/config.png)

NextCloud’s config.php contained bug notifications in scan results due to its usage of “Include” instead of “Include_Once”. This code section is utilized for reading the configuration file and current settings of the NextCloud server. Similar to previous findings of “Require” and Require_Once” the finding isn’t severe, but could impact security configuration settings being read and properly implemented if NextCloud should generate an error during this code’s execution. 

**[Finding 4:](https://potato.0x69.xyz/security_hotspots?id=nextcloud&hotspots=AYSheRrrx29KzD1GMcQ0)**

![image](https://github.com/unosec/project/blob/main/images/FTPConnection.png)

The FtpConnection.php file contains code that specifies “ftp_connect” instead of “ftp_ssl-Connect” This code section is for NextCloud’s External file storage and could cause unencrypted file transfers to occur. We previously documented that secure file transfers are a critical security requirement in order to keep data confidentially and prevent eavesdropping attacks. 

**Finding 5:**

![image](https://github.com/unosec/project/blob/main/images/JimTest1.png)

The calendar plug-in was scanned using SonarScanner.  No vulnerabilities were found.  Numerous "code smells" indicating possible coding style improvements were identified.


**Finding 6:**

![image](https://github.com/unosec/project/blob/main/images/Talk_App_scan.png)

![image](https://github.com/unosec/project/blob/main/images/Talk_App_scan2.png)

The Talk plug-in was scanned using SonarScanner. Some "bugs" were found which can lead to an error or unexpected behavious at runtime. Most of the "bugs" are minor with 3 major bugs, of which minor bugs can slightly impact developer productivity such as long lines and switch staements not having atleast 3 cases and major bugs quality flaw which can highly impact the developer productivity such as uncovered piece of code, duplicated blocks, unused parameters and more. 

Some of the major bugs included:

- <bold>Identical sub-expressions on both sides of operator "||":</bold> The use of uninitialized value: Using thesame values on either side of a binary operator is mostlt a mistake. In this case of the code it may be a copy and paste error or a bug or simply wasted code  and should be removed or simplified.

- Review the data-flow (Non Compliant code): In PHP initializing variables before their usage is not required. However, using uninitialized variables is considered bad practice and should be avoided because of the following reasons:

	- The value and type of uninitialized variables depend on the context of their first usage. 
	- It is better to be explicit about those to avoid confusion.
	- The interpreter raises a warning or a notice in many cases.

- Add "th" headers to this "table": Assitive technologies such as sceen readers use "th" headers to provide some context when users navaigate a table. Without it users can get lost in the flow of data. 

![image](https://github.com/unosec/project/blob/main/images/Talk_App_scan3.png)

Also, "code smells" were found indicating possible coding style improvements were identified. No vulnerabilities were found in the code.


Summary of Key Findings
- 

We analyzed the code in relation to the following weaknesses:

- [CWE-478: Missing Default Case in Multiple Condition Expression](https://cwe.mitre.org/data/definitions/478.html)
- [CWE-89: Improper Neutralization of Special Elements used in an SQL Command ('SQL Injection')](https://cwe.mitre.org/data/definitions/89.html)
- [CWE-74: Improper Neutralization of Special Elements in Output Used by a Downstream Component ('Injection')](https://cwe.mitre.org/data/definitions/74.html)

For CWE-89 and CWE-87 we performed manual reviews.  By taking a software assurance approach, we decided to investigate if we could find evidence supporting that the software, specifically the calendar functionality, was **not** susceptible to these weaknesses.  We found evidence that best practices for mitigating these weaknesses appeared to be in use.

For CWE-478 we performed an automated review as summarized under Finding 1.  A missing default cause was found in UserManagement.php.

An automated scan of the codebase for the calendar functionality found no vulnerabilities.




Contributions - OSS documentation, design changes, code changes, communications, etc.
-

[team notes - possibly open github issues on the nextcloud project to suggest fixing issues found in findings #1 or findings #4 - especially #4 - or indicate why we chose not to do so]

Reflections

Appendix A:  Using SonarQube on Windows
-

SonarQube offers a [community edition](https://docs.sonarqube.org/latest/setup/get-started-2-minutes/) with a docker implementation.  This approach seemed appropriate for this project.

Docker must first be installed on Windows.  This was done by following [these instructions](https://docs.docker.com/desktop/install/windows-install/).  Note that this also required WSL2 ([Windows subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/install)) to be installed on the PC.

After docker desktop is installed, the SonarQube docker images must be installed.  The first image installed was the base SonarQube website image.  This was installed and ran by entering the following commands in powershell:


```docker pull sonarqube docker run -d --name sonarqube -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:latest```


Once installed, the SonarQube website could be accessed by going to http://localhost:9000.  Following the SonarQube documentation, a "project" was then created in the SonarQube website.  This effectively provided a bucket for the results of the security analysis performed in the next step.

Next, the SonarScanner software needed to be installed to provide the specific functionality to scan PHP files.  This was installed by entering the following commands in powershell:

```docker pull sonarsource/sonar-scanner-cli```

In order to then perform a scan, I did the following:

- download the project to scan from github to a local folder.  In this case the folder was: C:\temp\nextcloud\3rdparty\sabre\dav\lib\CalDAV

```docker run -d --name sonarqube -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:latest```

Run the following SonarScanner command in powershell:

```docker run --rm -e SONAR_HOST_URL="http://172.31.144.1:9000/" -e SONAR_SCANNER_OPTS="-Dsonar.projectKey=JimTest1" -e SONAR_LOGIN="sqp_71e6b2ac956b3da33d972f36f5d0f06b6fbb36df" -v "C:\temp\nextcloud\3rdparty\sabre\dav\lib\CalDAV:/usr/src" sonarsource/sonar-scanner-cli```

The project key and the SONAR_LOGIN were both values provided by the SonarQube website when I created a test project.  **Two other important details**:

- the SONAR\_HOST\_URL value could not reference "localhost".  In order for one docker image to communicate to the other docker image, and IP address value from "ipconfig" needed to be used.
- the parameter -v "C:\temp\nextcloud\3rdparty\sabre\dav\lib\CalDAV:/usr/src" is important and it links a local folder to an internal folder used by the scanner software in the docker image.

After running the SonarScanner docker command above, the scan ran for approximately 6 minutes.  When the scan was complete assessment details were available in the SonarQube website.

