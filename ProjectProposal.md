# Project Proposal
Team 2 - Fall 2022
---

Overview
-
For our project we will review the open source project ["Nextcloud"](https://github.com/nextcloud)

Systems Engineering View
-

![System Engineering View](https://github.com/unosec/project/blob/main/images/sev.png "System Engineering View")


Security Needs, Threats, and Features
-
###Needs
Nextcloud is intended for enterprise use, general users would have an expectation of basic privacy protection when it comes to confidential files or data stored in the application. Server administrators would have an expectation of web interface security controls that protect the underlining infrastructure from basic web attacks.

###Threats
- Theft of files or data from a user
- Remote code execution on the hosting server via public web interface
- DDOS of hosting server
- Code injection via text boxes on public web interface
- Unattended resource exhaustion of server resources
- Privilege escalation
- Cross site scripting
- Authentication issues \ authentication bypass 
- No lockout \ faulty password reset?

###Features
- RBAC permission options 
- Backups \ restore from previous settings
- Verbose application logging 
- End to end encryption


Motivation 
-
We had initially planned to investigate a different project [audiobookshelf](https://github.com/advplyr/audiobookshelf) however the instructor suggested that instead we look for something that might be more likely to be used in the enterprise.  Josh had experience using Nextcloud and suggested that as an option, so we decided to go with that.  Since this is a large system with a server component, desktop component, and mobile components there should be many areas available for further security analysis.

Additionally, this project appears very active and well updated. Significant effort has been put into making this project secure, which leads us to believe they will consider any security recommendations we present at the end of our course.  We also feel the GNU AGPL-3.0 license would not restrict our contributions or hinder our performance.


OSS project description
-
[Nextcloud](https://nextcloud.com) describes itself as being "the industry-leading, on-premises content collaboration platform".  It's a solution similar to Dropbox or Google Drive, but offers the benefit of being an on-premise solution that's under the control of the organization implementing the software.  There are 258 separate repositories in the Nextcloud GitHub project site including code for self-hosting the server, retrieving documents via workstation applications, along with mobile apps to interact with any shared data. The [server software can be downloaded for use as one .zip file](https://download.nextcloud.com/server/releases/latest.zip).
 
- This project is very active and has 813 contributors. 
- 61% php, 31% Javascript, 8% other
- 20k Stars on GitHub and 3.2k forks. 
- Hosts an independent website with additional documentation, install guides, community support.



License Summary and Contributor Agreement
-
Nextcloud is licensed using the [GNU Affero General Public License v3.0 (AGPL-3.0)](https://www.gnu.org/licenses/agpl-3.0.en.html) license.  This is a "copyleft" license and requires making the complete source code of any derivative works or modifications available.   All copyright and license notices must be preserved.  Any contributors must also grant patent rights.  In summary, this is a strong or restrictive license as opposed to other permissive licenses.


Security History
-


- Publicly released vulnerabilities can be found [here](https://www.cvedetails.com/vendor/15913/Nextcloud.html)
- Numerous general security related updates are shown in recent change logs. 
- A bug bounty program has been developed and has more than 350 resolved reports.
- Published security best practices have been created for administrators to follow.


Project Planning and Reflection
-
Team 2 meets every Tuesday night at 5:30pm CT.  Josh is serving as the team lead, and the other team members are acting in whatever other role is needed to complete the given tasks and move the project forward.







