# CYBR/CSCI 8420, Fall 2022: Group Project Proposal

**Team 2**:
* cosmicspork -- Team Lead
* Dillon Petschke
* Larissa Sazama
* Jim Svoboda
* Tabot Tambe
---

Project Information
-
**Chosen Software Project**: [Nextcloud](https://github.com/nextcloud)  
[Group GitHub Repository](https://github.com/unosec/project)  
[Group Project Boards](https://github.com/orgs/unosec/projects)

---
Operational Environment
-
A common operational environment for the community version of Nextcloud is in a small to medium-sized business where there is no full-time, IT staff, and the software was set up by a 3<sup>rd</sup> party. Often in this case, the software is hosted locally (installed on a server within the building) or via a Virtual Private Server. One or two employees are trained to use the Graphical User Interface (GUI) for administration of the software. In this environment there is an expectation the software will not leak sensitive information through the local network or onto the internet. Administrators expect to manage user access and be notified of common security issues. Users expect personal information, confidential files, and data, stored within the software, to not be unnecessarily available for Nextcloud use. The business anticipates that vulnerabilities be disclosed, for the software to receive timely security updates, and for the GUI to be protected against basic web attacks.

<p align="center" width="100%">
 <b>Systems Engineering View</b><br>
 <img width="75%" src="https://github.com/unosec/project/blob/main/images/sev.png" alt="Systems Engineering View" />
</p>

### Threats Perceived by Users
- Theft of files or data
- Remote code execution on the hosting server via public web interface
- Distributed denial-of-service attack on hosting server
- Code injection via text boxes on public web interface
- Unattended resource exhaustion of server resources
- Privilege escalation
- Cross-site scripting
- Authentication issues\authentication bypass 
  - No lockout\faulty password reset

### [Security Features in the Software](security/Security-Whitepaper-WebVersion-072018.pdf)
- Role-based access control permission options 
- Backup\restore from previous settings
- Verbose application logging 
- End-to-end encryption
- Some vulnerable features disabled
- Brute force protection
- Guarantee compliance with business and legal requirements (HIPAA, GDPR, and more)
- Aligns with [Clause 14 of ISO/IEC27001-2013](https://www.itgovernance.co.uk/blog/iso-27001-the-14-control-sets-of-annex-a-explained)
- Provide principal baseline security
- Keeps compliance-ready activity logs
- Monitoring and logging tools
- Single and SAML 2.0 authentication
- Two-Factor Authentication
- Video verification

---
Team Motivation for Selecting this Software
-
This software appears very active and well-maintained. Significant efforts have been made to ensure the software's security. Because this software is a large system with a server component, desktop component, and mobile component, there should be many areas available for further security analysis. The GNU AGPL-3.0 license will not restrict contributions or hinder our project performance.

---
Software Description
-
[Nextcloud](https://nextcloud.com) describes itself as being "the industry-leading, on-premises content collaboration platform".  It is a solution similar to Dropbox or Google Drive, but offers the benefit of being an on-premises solution that is under the control of the organization implementing the software.  There are 258 separate repositories in the Nextcloud GitHub project site. These repositories include code for self-hosting the server, retrieving documents via workstation applications, mobile apps to interact with any shared data, among many other applications. The [server software can be downloaded for use as one .zip file](https://download.nextcloud.com/server/releases/latest.zip).

_Server Repository_:
- 826 contributors
- 61% PHP, 34% JavaScript, 5% other
- 20k Stars on GitHub and 3.2k forks

Hosts an independent [website](https://nextcloud.com) containing downloads, installation guides, community support, etc.

---
License Summary and Contributor Agreement
-
Nextcloud is licensed using the [GNU Affero General Public License v3.0 (AGPL-3.0)](https://www.gnu.org/licenses/agpl-3.0.en.html) license. This is a "copyleft" license and requires making the complete source code of any derivative works or modifications available. All copyright and license notices must be preserved. Any contributors must also grant patent rights. In summary, this is a strong or restrictive license as opposed to other permissive licenses.

Nextcloud provides a contributor [Code of Conduct](https://nextcloud.com/contribute/code-of-conduct/) and [guidelines for contributing](https://github.com/nextcloud/server/blob/master/.github/CONTRIBUTING.md). To note, any potential security bugs should be reported using the [Nextcloud HackerOne page](https://hackerone.com/nextcloud) instead of filing an issue in the bug tracker.

---
Security History
-
- Publicly released vulnerabilities can be found [here](https://www.cvedetails.com/vendor/15913/Nextcloud.html)
- Threat model and accepted risks listed on [Nextcloud's website](https://nextcloud.com/security/threat-model/)
- Numerous general security related updates are shown in recent change logs. 
- A bug bounty program has been developed and has more than 350 resolved reports.
- Published security best practices have been created for administrators to follow.

---
Project Planning and Reflection
-
_Team 2_ meets every Tuesday night at 5:30pm CST. Josh is serving as the team lead, while the other team members fill any role needed to complete the given tasks and move the project forward.  All team members contributed to various parts of this documentation, and we shared information in the team discord channel. No issues among team members occurred for the proposal. As the semester progresses, team members will try to rotate through team roles.







