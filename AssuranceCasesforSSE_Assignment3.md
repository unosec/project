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

![image](https://github.com/unosec/project/blob/main/images/dpetschke-Claim.png)

Top-Level Claim #3
-
[TBD]


Top-Level Claim #4
-
[TBD]


Top-Level Claim #5
-
[TBD]

Part 2 
-

[TBD]

Planning and Reflection
-
[TBD]
