# CYBR/CSCI 8420, Fall 2022: Assignment 4 - Designing for SSE


Assignment Overview / Project Board
-
This document reviews the data flow diagrams (DFDs) for each of the five scenarios that were identified when building Nextcloud application misuse cases and assurance claims for our hypothetical bank staff.


Scenarios
-

- The Nextcloud calendar form does not allow injection exploits.
- Nextcloud sufficiently prevents unauthorized file access. 
- Nextcloud prevents malicious file uploads.
- Nextcloud does not leak authentication information.
- The Nextcloud Mail app is acceptably secure.

All scenarios are represented in the same DFD.

Level 0 DFD
-

Mitigations strategies are described in the Level 2 DFD.

- <h3><a href="https://unosec.github.io/Level0DFD.htm" target="_blank">Link to Full Report</a> </h3>

Level 1 DFD 
-

Mitigations strategies are described in the Level 2 DFD.

- <h3><a href="https://unosec.github.io/Level1DFD.htm" target="_blank">Link to Full Report</a> </h3>

Level 2 DFD 
-

- <h3><a href="https://unosec.github.io/Level2DFD.htm" target="_blank">Link to Full Report</a> </h3>

Observations
-

Based on the threat analysis, the following design-related observations were made:

- Nextcloud supports username/password authentication and role-based access to various functions.  This mitigates application level spoofing.
- The system is expected to support encrypted communication across the internet threat boundary, as well as within the firewall (if supported by systems such as the database).  If the internal network is not fully trusted, then unencrypted internal communication is a gap that needs to be addressed.  This is needed to mitigate tampering and information disclosure.
- The system is not expected to support data sanitization upon input. However, all parts of the system that communicate with the database should use parameterized queries (or stored procedures) to prevent SQL injection. PHP logic is expected to sanitize output by using the "htmlspecialchars" function; this is needed to prevent tampering.
- NGINX supports configuration options for rate limiting and user memory limitations to mitigate denial of service attacks.
- NGINX and Nextcloud both use plaintext configuration files.  These files may contain sensitive information, such as server names, IP addresses, and database authorization credentials.  A gap that should be addressed is that both NGINX and Nextcloud must support encrypted configuration files, or at least reading encrypted information from standard configuration files in order to prevent tampering.
- NGINX, Nextcloud, the File System, and the SQL database (depending on database used) all support various levels of logging to address repudiation issues.
- The Nextcloud application needs an appropriate account to permit file system access, but the file system data store must also be properly secured to prevent access or tampering by other internal users or groups.
- The Calendar and Mail Plug-ins may run in the same process/memory space as Nextcloud. But since they are completely separate open source code projects, used by Nextcloud, they were diagrammed as separate processes.
- NextCloud’s lack of HA server pairing features and lack of load balancing over multiple servers may introduce availability risks if a single server crashes.
- RBAC settings need appropriate configurations to be effective in their role of preventing unauthorized file read\write access. Nextcloud’s out of the box settings will not suffice to meet standard security controls. 
- NextCloud’s optional anti-virus will need to be configured and enabled to meet standard security controls. Out of the box settings do not automatically enable this feature. 
- NextCloud’s optional server-side file encryption will need to be utilized to meet standard security controls. Out of the box settings do not automatically enable this feature.
- Five threats were found to need investigation before determining whether the threat is, in fact, mitigated within the OSS or if the threat highlights a vulnerability within the OSS. (They can be found by referring to the Level 2 DFD. Threats 37, 41, 45, 55, and 58.
 
<h3><a href="https://github.com/orgs/unosec/projects/8" target="_blank">Project Board</a> </h3>

Project Collaboration
-
The team was not able to meet during our regularly scheduled, weekly meeting. This was one of the issues for this assignment. Moving forward we will try to attend the weekly meetings, even if the date/time must be moved for it to work. The verbal collaboration is becoming very important for the assignments.

During our weekly meeting with Dr. Gandhi we clarified that an external database not part of the codebase/project should be listed as an External Interactor.  He also clarified that detailed threat/mitigation analysis was not needed on the Level 0 diagram.

Josh developed the initial Level 1 diagram and shared it with the team via Discord.  Jim added in additional details to the diagram.  Each team member documented mitigation strategies relevant to their scenarios or application familiarity. All members reviewed and submitted edits for the assignment .md and .tm7 files.

The DFD .tm7 file was shared on github and team members just made effort to not make conflicting or simultaneous changes to the files. This also became a bit of an issue to try to arrange work on the file around each other. Moving forward, if one file is to be shared between the five of us, we will try to come up with a different sharing option.

