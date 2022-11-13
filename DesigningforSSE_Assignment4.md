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

Level 0 DFD
-

- <h3><a href="https://unosec.github.io/Level0DFD.htm" target="_blank">Link to Full Report</a> </h3>

Level 1 DFD 
-

- <h3><a href="https://unosec.github.io/Level1DFD.htm" target="_blank">Link to Full Report</a> </h3>

Level 2 DFD 
-

- <h3><a href="https://unosec.github.io/Level2DFD.htm" target="_blank">Link to Full Report</a> </h3>

Observations
-

Based on our threat analysis, we made the following design-related observations:

- Nextcloud supports username/password authentication and role based access to various functions.  This mitigates application level spoofing.
- We expect the system to support encrypted communication across the internet threat boundary, and possibly also within the firewall if supported by systems such as the database.  If the internal network is not fully trusted, then unencrypted internal communication is a gap that needs to be addressed.  This is needed to mitigate tampering and information disclosure.
- We don't expect the system to support data sanitization upon input, but we expect all parts of the system that communicate with the database to use parameterized queries (or stored procedures) to prevent SQL injection and we expect PHP logic to sanitize output by using the "htmlspecialchars" function.  This is needed to prevent tampering.
- NGINX supports configurations options for rate limiting and user memory limitations to mitigate denial of service attacks.
- NGINX and Nextcloud both use plain text configuration files.  These files may contain sensitive information such as server names and IP address and database authorization credentials.  A gap that probably needs to be addressed is that both NGINX and Nextcloud need to support encrypted configuration files, or at least reading encrypted information from standard configuration files in order to prevent tampering.
- NGINX, Nextcloud, the File System, and the SQL database (depending on database used) all support various levels of logging to address repudiation issues.
- The Nextcloud application needs an appropriate account to permit file system access, but the file system data store also needs to properly secured to prevent access or tampering by other internal users or groups.
- While the Calendar Plug-in and Mail Plug-in may run in the same process/memory space as Nextcloud, since they are completely separate open source code projects used by Nextcloud it seemed appropriate they should diagrammed as a separate processes.
- NextCloud’s lack of HA server pairing features and lack of load balancing over multiple servers may introduce availability risks if a single server would crash. 
- RBAC settings need appropriate configurations to be effective in their role preventing unauthorized file read\write access. Nextcloud’s out of the box settings will not suffice to meet standard security controls. 
- NextCloud’s optional anti-virus will need to be configured and enabled to meet standard security controls. Out of the box settings do not automatically enable this feature. 
- NextCloud’s optional server-side file encryption will need to be utilized to meet standard security controls. Out of the box settings do not automatically enable this feature.
 
<h3><a href="https://github.com/orgs/unosec/projects/8" target="_blank">Project Board</a> </h3>

Project Collaboration
-
The team met via discord on Tuesday 10/8 at 5:30pm. The team also used Discord throughout the week to discuss items.

During our weekly meeting with Dr. Gandhi we clarified that an external database not part of the codebase/project should be listed as an External Interactor.  He also clarified that detailed threat/mitigation analysis was not needed on the Level 0 diagram.

Josh developed the initial Level 1 diagram and shared it with the team via Discord.  Jim added in additional details to the diagram.  Each team member documented mitigation strategies relevant to their scenarios or application familiarity.

The DFD .tm7 file was shared on github and team members just made effort to not make conflicting or simultaneous changes to the files.

Project collaboration could maybe be improved by the team scheduling additional Zoom or Discord meetings for in-person verbal collaboration, but this has been difficult to schedule and some team members felt that text-based discussion was sufficient.
