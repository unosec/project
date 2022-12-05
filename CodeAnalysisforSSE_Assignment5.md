# CYBR/CSCI 8420, Fall 2022: Assignment 5 - Designing for SSE


### Assignment Overview / Project Board

This document provides an overview of our code review strategy, as well as findings from both our automated code scanning and manual code review of critical security functions. The code review is of [Nextcloud](https://github.com/nextcloud), our semester-long, open-source software (OSS) project.

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
The critical portions of manually reviewed code included file management, user management, encryption, and security controls implemented by Nextcloud. These code sections were chosen due to their importance in security compliance and their ability to prevent the documented misuse cases. Because of the severity of a possible vulnerability, focus was placed on the following assurance claims:
- Nextcloud sufficiently prevents malicious file uploads.
- Nextcloud does not leak authentication information.
- The Nextcloud calendar form does not allow injection exploits.

For manual review, the Linux grep utility was used to search the codebase.

Often database-driven website software is susceptible to [CWE-89: SQL Injection weakness](https://cwe.mitre.org/data/definitions/89.html) or [CWE-74: HTML/Javascript Injection attacks](https://cwe.mitre.org/data/definitions/74.html). By taking a software assurance approach, investigation was focused on whether or not the software, specifically the calendar functionality, was susceptible to these weaknesses.

In PHP code, a developer should use the "htmlspecialchars" function when outputting data to a web page. This function will escape the output and help prevent cross-site scripting attacks. 

```grep -r htmlspecialchars```

This function was found being used [on line 78](https://github.com/nextcloud/3rdparty/blob/master/sabre/dav/lib/DAV/Browser/HtmlOutputHelper.php).

In searching the Server code, this function was also used in files such as _Setup.php_, _Manager.php_, *OC_Util.php*, and _EMailTemplate.php_.

This evidence would support that efforts have been made to mitigate CWE-89.

For SQL Injection, one method a developer should use is parameterized queries.  The code was searched for SQL syntax like:

```grep -r "INSERT INTO"```

In reviewing (what seems to be) the [data access code](https://github.com/nextcloud/3rdparty/blob/020d0d3892bd3b7296db8ed21448c834d33d5723/sabre/dav/lib/CalDAV/Backend/PDO.php) for the CalDAV functionality, the system appears to use parameterized queries. Lines 253-254:
 
```  
$stmt = $this->pdo->prepare('INSERT INTO '.$this->calendarTableName.' (synctoken, components) VALUES (1, ?)');  
$stmt->execute([$components]);  
```

In searching the Server code, evidence of parameterized queries was found in file _\apps\files_sharing\lib\External\Manager.php_. An instance of parameterized queries **not** being used was found in file _\lib\private\DB\Adapter.php_; however this method was marked as deprecated.


### Automated Code Review
For automated review, [SonarQube/SonarSource/SonarScanner](https://www.sonarqube.org/) was used on both Linux and Windows.  This is considered to be [one of the best tools for PHP static code analysis](https://phpmagazine.net/2020/10/top-php-security-and-malware-scanners.html).  A description of the Windows SonarQube installation and use is described in [Appendix A](https://github.com/unosec/project/blob/main/CodeAnalysisforSSE_Assignment5.md#appendix-a--using-sonarqube-on-windows). A secondary scanning tool that was used was [ZAP](https://www.zaproxy.org/). Because ZAP is a web application scanner, [the results](https://unosec.github.io/2022-11-22-ZAP-Report.html) were not as helpful so our time was spent going through the SonarQube findings instead.

The Nextcloud Server code, as well as the code used for the Calendar, Talk, and Mail apps were scanned and reviewed. 

##### SonarQube Scan Review

---
*[Finding 1:](https://potato.0x69.xyz/project/issues?files=apps%2Fadmin_audit%2Flib%2FActions%2FUserManagement.php&resolved=false&id=nextcloud&open=AYSheR9xx29KzD1GMcyt)*

![image](https://github.com/unosec/project/blob/main/images/UserManagement.png)

A minor issue with Nextcloud’s _UserManagement.php_ file was discovered related to [CWE-478: Missing Default Case in Multiple Condition Expression](https://cwe.mitre.org/data/definitions/478.html). While this typically is not a critical issue, the potential for unintended impact and misconfigurations is increased due to this section of code controlling the audit logging of system activities of Nextcloud users. In previous assignments, we recognized Nextcloud’s auditing and logging as a critical security component due to its ability to detect and monitor potential threats and attack vectors. 

---
*[Finding 2:](https://potato.0x69.xyz/project/issues?files=apps%2Fencryption%2Fcomposer%2Fcomposer%2FInstalledVersions.php&resolved=false&id=nextcloud&open=AYSheSwVx29KzD1GMeCp)*

![image](https://github.com/unosec/project/blob/main/images/InstalledVersions.png)

Nextcloud’s _InstalledVersions.php_ contained bug alerts in our automated code scans due to its usage of “Require” instead of “Require_Once”. This code section is utilized for implementing the encryption of files saved via Nextcloud’s File feature. File encryption was documented as a critical security function preventing unauthorized access and several other misuse cases. While the difference between “Require” and “Require_Once“ is minor, the latter option would only generate warnings if improper, vendor-installed, encryption mechanisms were implemented. The current code could generate fatal errors impacting both the confidentiality and availability of Nextcloud’s File features. 

---
*[Finding 3:](https://potato.0x69.xyz/project/issues?files=lib%2Fprivate%2FConfig.php&resolved=false&id=nextcloud&open=AYSheUOox29KzD1GMhPz)*

![image](https://github.com/unosec/project/blob/main/images/config.png)

Nextcloud’s _config.php_ contained bug notifications in the scan results due to its usage of “Include” instead of “Include_Once”. This code section is utilized for reading the configuration file and current settings of the Nextcloud server. Similar to the previous findings of “Require” and Require_Once”, the issue is not severe, but could impact security configuration settings being read and properly implemented if Nextcloud should generate an error during this code’s execution. 

---
*[Finding 4:](https://potato.0x69.xyz/security_hotspots?id=nextcloud&hotspots=AYSheRrrx29KzD1GMcQ0)*

![image](https://github.com/unosec/project/blob/main/images/FTPConnection.png)

The _FtpConnection.php_ file contains code that specifies “ftp_connect” instead of “ftp_ssl-Connect”. This code section is for Nextcloud’s external file storage and could cause unencrypted file transfers to occur. We previously documented that secure file transfers are a critical security requirement in order to keep data confidential and prevent eavesdropping attacks. 

---
*Finding 5:*

![image](https://github.com/unosec/project/blob/main/images/JimTest1.png)

The Calendar plug-in was scanned using SonarScanner. No "Vulnerabilities" were found. Numerous "Code Smells", indicating possible coding style improvements, were identified.

---
*Finding 6:*

![image](https://github.com/unosec/project/blob/main/images/Talk_App_scan.png)

![image](https://github.com/unosec/project/blob/main/images/Talk_App_scan2.png)

The Talk plug-in was scanned using SonarScanner. Some "Bugs" were found which can lead to an error or unexpected behaviors at runtime. Most of the bugs were "Minor" with only three "Major" bugs. Minor bugs, such as long lines and switch statements not having at least three cases, can slightly impact developer productivity. Major bugs, such as uncovered pieces of code, duplicated blocks, unused parameters, etc., are quality flaws that can highly impact the developer productivity.

Some of the major bugs included:

- **Identical sub-expressions on both sides of operator "||":** "Using the same value on either side of a binary operator is almost always a mistake. In the case of logical operators, it is either a copy/paste error and therefore a bug, or it is simply wasted code, and should be simplified."

- **Review the data-flow - use of uninitialized value:** In PHP initializing variables before their usage is not required. "However, using uninitialized variables is considered bad practice and should be avoided because of the following reasons:

	- The value and type of uninitialized variables depend on the context of their first usage. It is better to be explicit about those to avoid confusion.
	- The interpreter raises a warning or a notice in many cases."

- **Add "`<th>`" headers to this "`<table>`":** "Assistive technologies, such as sceen readers use `<th>` headers to provide some context when users navaigate a table." Without it, users can get lost in the flow of data. 

![image](https://github.com/unosec/project/blob/main/images/Talk_App_scan3.png)

"Code Smells" were found, indicating possible coding style improvements, were identified. No "Vulnerabilities" were found in the code.

---
*Finding 7:*

The Mail plug-in was scanned using the SonarScanner software. The app received passing marks but did show a few minor "Bugs" and thousands of "Code Smells".

![image](/images/MailScanResults.png)

Upon further review, 13 of the bugs were found to be improper coding techniques (e.g., adding "alt" to an `<img>` tag, etc.). The bug classified as a "Major" issue is used in the _tests_ directory, thus is only used in the development environment.

The majority of the "Hotspots Reviewed" issues are used in the _test_ directory so were moved to the "Safe" status in the SonarQube GUI. A "Medium" priority, that was found, questions the `chmod` permission command used in a file.

![image](/images/MailPermission.png)

The permissions are being tested within a _try_ block and give the _group_ and _others_ read-only permission. This should protect the program from exposure to [CWE-732: Incorrect Permission Assignment for Critical Resource](https://cwe.mitre.org/data/definitions/732.html).

Two of the "Low" priorities refer to using "https" instead of "http". While this practice is the most secure option, it appears that the two issues refer to a line of code that is being used as an "insecure fallback" option when trying to access the URL of an email address. The "https" protocol is also being used in the same function.

![image](/images/MailEncryption.png)

Due to the importance of the [CWE-200: Exposure of Sensitive Information to an Unauthorized Actor](https://cwe.mitre.org/data/definitions/200) and [CWE-319: Cleartext Transmission of Sensitive Information](https://cwe.mitre.org/data/definitions/319) weaknesses, removing the "insecure fallback" lines of code might be the needed actions in this situation.

Like the previously discussed "Low" priorities, the final three issues refer to code that does not protect sensitive information. However, the `md5` hash function can easily be replaced using `sha512`, or a different recommended hashing algorithm, to protect the information being hashed.

![image](/images/MailHash.png)

Similar to the discussion from _Finding 1_, four critical issues refer to the [CWE-478: Missing Default Case in Multiple Condition Expression](https://cwe.mitre.org/data/definitions/478.html) weakness. One of the issues refers to a part of the [file](https://github.com/nextcloud/mail/blob/main/lib/Account.php) that is being used to "convert special security mode values into Horde parameters", while the other three refer to parts of code being used for email messages and accounts (i.e., setting a tag for the type of email account, string parsing for the search function, and message mapping). While this data protection may not be considered crucial, the addition of a default case can prevent against complex logical errors and weaknesses that may be presented if the switch statement does not handle all possible values that could occur.


### Summary of Key Findings

The following weaknesses were investigated further to understand the severity if each weakness were to appear in the Nextcloud code:

- [CWE-89: Improper Neutralization of Special Elements used in an SQL Command ('SQL Injection')](https://cwe.mitre.org/data/definitions/89.html)
- [CWE-74: Improper Neutralization of Special Elements in Output Used by a Downstream Component ('Injection')](https://cwe.mitre.org/data/definitions/74.html)
- [CWE-478: Missing Default Case in Multiple Condition Expression](https://cwe.mitre.org/data/definitions/478.html)
- [CWE-732: Incorrect Permission Assignment for Critical Resource](https://cwe.mitre.org/data/definitions/732.html)
- [CWE-200: Exposure of Sensitive Information to an Unauthorized Actor](https://cwe.mitre.org/data/definitions/200)
- [CWE-319: Cleartext Transmission of Sensitive Information](https://cwe.mitre.org/data/definitions/319)

Manual reviews were performed for CWE-89 and CWE-74.  By taking a software assurance approach, we investigated if we could find supporting evidence that the software, specifically the Calendar functionality, was **not** susceptible to these weaknesses.  We found evidence that best practices for mitigating these weaknesses appeared to be in use.

An automated scan of the code for the Calendar, Talk, and Mail functionalities found no "Vulnerabilities". (One "Vulnerability" was found from the Nextcloud Server scan, but referred to an issue in the _tests_ directory.) However, multiple "Bugs", "Hotspots Reviewed", and "Code Smells" were found within the four code bases. None of the issues were severe enough to make the code "Fail", but many of the items could easily be fixed by updating the programming to preferred programming practices.


### Contributions - OSS documentation, design changes, code changes, communications, etc.

Because Nextcloud is a very mature program, when reporting a vulnerability multiple steps must be taken.

![image](/images/ReportingAVulnerability.png)

Per the steps, we noted the [threat model](https://nextcloud.com/security/threat-model/) and read the responsible disclosure guidelines.

![image](/images/DisclosureGuidelines.png)

After reviewing the mandated literature, as well as the information listed on the [hackerone](https://hackerone.com/nextcloud?type=team) site, we decided to submit a report regarding _Finding 4_.

![image](/images/FTPSubmission.png)

(Please ignore their request listed on the bottom of the image.)


### Reflections

_What challenges did we expect before starting code review?_
- With a very large, mature program, what vulnerabilities will we actually find? And how much work will it be to understand the issues found? As stated in [Secure Programming with Static Analysis](https://learning.oreilly.com/library/view/secure-programming-with/9780321424778/ch02.html), "After a project has begun, revisiting code purely to make adjustments to reduce output from the style checker will realize only a marginal benefit and at the cost of great inconvenience."
- Challenges regarding the inexperience of using PHP code review tooking and analyzing the results. With 425,000 lines of code, a manual code review will not be possible with our time constraints. Therefore, we will need to utilize automated tooling to improve our efficiency.
- Challenges regarding the third-party applications and what vulnerabilities they may add to the Nextcloud platform itself. How can these vulnerabilities be remediated or isolated even when the developers are unwilling to fix them?

_How did our code review strategy attempt to address the anticipated challenges?_  
The strategy was to break the code into portions that could be reviewed by multiple people. The three apps were also reviewed separately.

By reviewing the scanning results, we could sort the findings by using CWE tags. This helped narrow down the important issues that were found by the scanning tool.

---
cosmicspork installed and ran the scans for SonarQube and ZAP in his docker containers. He then shared the link so that the group could analyze the results.
Jim created the markdown file for this assignment, performed the manual code review of the critical portions of the code, analyzed the automated scan findings from the Calendar plug-in, installed a docker container and SonarQube to create the instructions found in Appendix A.
Dillon analyzed the automated scan findings from the Server code.
Talbot analyzed the automated scan findings from the Talk plug-in.
Larissa analyzed the automated scan findings from the Mail plug-in and reported the vulnerability to Nextcloud.
All contributed to the "Summary of Key Findings" and "Reflections" sections of the assignment.

Thankfully, we ran into no issues during this assignment as our plan-of-attack executed smoothly. We will be conducting one last Zoom meeting to discuss our plans for the Presentation assignment, as well as continuing to discuss any questions through our Discord channel.

### Appendix A: Using SonarQube on Windows

SonarQube offers a [community edition](https://docs.sonarqube.org/latest/setup/get-started-2-minutes/) with a docker implementation.  This approach was appropriate for this project.

1. Docker must first be installed on Windows.  This was completed by following [these instructions](https://docs.docker.com/desktop/install/windows-install/).  **Note**: SonarQube also requires that WSL2 ([Windows subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/install)) be installed on the PC.

2. After Docker Desktop is installed, the SonarQube docker images must be installed.  The first image installed was the base SonarQube website image.  This was installed and run by entering the following commands in powershell:

```
docker pull sonarqube
docker run -d --name sonarqube -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:latest
```

3. Once installed, the SonarQube website is accessed by going to http://localhost:9000.  A "project" is then created in the SonarQube website.  This effectively provides a bucket for the results of the security analysis performed.

4. The SonarScanner software needs to be installed to provide the specific functionality for scanning PHP files.  This is installed by entering the following command in powershell:

```docker pull sonarsource/sonar-scanner-cli```

5. In order to perform a scan, the following was needed:

- Download, from github, the project to scan and save it to a local folder.  In this case the folder was: *C:\temp\nextcloud\3rdparty\sabre\dav\lib\CalDAV*

```docker run -d --name sonarqube -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:latest```

- Run the following SonarScanner command in powershell:

```
docker run --rm -e SONAR_HOST_URL="http://172.31.144.1:9000/" -e SONAR_SCANNER_OPTS="-Dsonar.projectKey=JimTest1" -e SONAR_LOGIN="sqp_71e6b2ac956b3da33d972f36f5d0f06b6fbb36df" -v "C:\temp\nextcloud\3rdparty\sabre\dav\lib\CalDAV:/usr/src" sonarsource/sonar-scanner-cli
```

The *project key* and the *SONAR_LOGIN* are both values provided by the SonarQube website when a test project is created.  **Two other important details**:

     - the *SONAR\_HOST\_URL* value could not reference "localhost".  In order for one docker image to communicate with the other docker image, an IP address value from "ipconfig" needed to be used.
     - the parameter `-v "C:\temp\nextcloud\3rdparty\sabre\dav\lib\CalDAV:/usr/src"` is important. It links an internal folder, used by the scanner software in the docker image, to a local folder.

After running the SonarScanner docker command above, the scan ran for approximately 6 minutes.  When the scan was complete, assessment details were available on the SonarQube website.

[Return](https://github.com/unosec/project/blob/main/CodeAnalysisforSSE_Assignment5.md#automated-code-review)

