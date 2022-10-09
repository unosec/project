# CYBR/CSCI 8420, Fall 2022: Assignment 3 - Assurance Cases for SSE


Assignment Overview / Project Board
-
This document will review the development of five top-level assurance claims for our hypothetical bank staff using the Nextcloud application.

[Project Board](https://github.com/orgs/unosec/projects/3)


Top-Level Claim #1
-
### Claim

- The Nextcloud calendar form does not allow injection exploits.

![image](https://github.com/unosec/project/blob/main/images/JSassignment3.png)

**Alignment Assessment:**  Product documentation typically doesn't address a low-level feature such as injection attacks, so a deeper investigation such a code review is required.  I cloned the server repository and began to inspect the code.  Nextcloud itself is composed of many other open source projects reference via the "3rdparty" folder.  The calendar functionality is in the CalDAV project from [sabredav.org](https://sabredav.org).  I used grep to search the files for the "htmlspecialchars" function.  I found this function in use of line 78 of [this file](https://github.com/nextcloud/3rdparty/blob/master/sabre/dav/lib/DAV/Browser/HtmlOutputHelper.php) which indicates logic is in place to prevent HTML/Javascript injection attacks.  In reviewing what appears to be the [data access code](https://github.com/nextcloud/3rdparty/blob/020d0d3892bd3b7296db8ed21448c834d33d5723/sabre/dav/lib/CalDAV/Backend/PDO.php) for the CalDAV functionality, the system does appear to use parameterized queries, for example on lines 253-254:
``` 
$stmt = $this->pdo->prepare('INSERT INTO '.$this->calendarTableName.' (synctoken, components) VALUES (1, ?)');
$stmt->execute([$components]);
```
As such the calender data access code seems to also handle SQL injection attacks.

  

<!-- https://www.php.net/manual/en/function.htmlspecialchars.php -->

Top-Level Claim #2
-
### Claim
- Nextcloud sufficiently prevents unauthorized file access. 

![image](https://github.com/unosec/project/blob/main/images/Dpetschke-Claim.png)

**Alignment Assessment:**  Extensive documentation around [file access controls](https://docs.nextcloud.com/server/latest/admin_manual/file_workflows/access_control.html) and Role Based Access Controls currently exists in the NextCloud administrator guide. 2FA topics and [setup instructions](https://docs.nextcloud.com/server/latest/admin_manual/configuration_user/two_factor-auth.html) are also available. However, a lack of support for popular apps limits the current options for 2FA strictly to text message instead of commonly used apps like Google authenticator or Duo. [Disk encryption](https://docs.nextcloud.com/server/latest/admin_manual/configuration_files/encryption_configuration.html) and individual file encryption is also widely documented in NextCloud’s administrator’s guide. Along with several other best practice security control recommendations. 


Top-Level Claim #3
-

### Claim
- Nextcloud prevents malicious file uploads

![image](https://github.com/unosec/project/blob/main/images/MalwareProtectionUseCase.png)


Top-Level Claim #4
-

### Claim
- Nextcloud does not leak authentication information

![image](https://github.com/unosec/project/blob/main/images/LDAPAssuranceCase.drawio.png)

**Alignment Assessment:** Very little about the security of Nextcloud's LDAP integration is included in the documentation. However there are security scanning features built-in to check that the server is using secure connections. It will prompt the server administrator(s) to make changes if the software is getting HTTP requests. I was able to test this on an instance running in a Docker container on a server on my local network. There is also a Nectcloud provided [external security scanning tool](https://scan.nextcloud.com/) that will check for insecure connections. 


Top-Level Claim #5
-
### Claim
- Nextcloud minimizes person-in-the-middle attacks.

[TBD]

Part 2 
-

[TBD]

Planning and Reflection
-
The team met on Tuesday 10/4 at 5:30pm. It was decided that each team member would handle one top-level claim. The team also uses Discord throughout the week to discuss items.

We discussed that it would make sense to continue to focus on the features we have been working with throughout the project.

During our weekly meeting with Dr. Gandhi we clarified that predicates should not contain compound statements (ex. "this or that"), that rebuttals should come between claims (a claim shouldn't directly support a claim), and that inference rules were more of an advanced topic.
