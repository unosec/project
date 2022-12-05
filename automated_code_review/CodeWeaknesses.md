### Misuse Cases
- Prevent malicious content from being entered into Nextcloud Calendar
  - Follow OWASP programming best practices
- Prevent ransomware
  - 2FA
- Prevent unauthorized change of files
  - RBAC
- Prevent the sharing of malicious files
  - AV Scan
- Provide encrypted chats and video calls
- Provide password-protected meeting invites
- Prevent DOS
- Protect login credentials
  - LDAP integration
- Prevent packet sniffing
  - Enforce SSL/TLS
- Prevent PITM attacks
  - Message encryption through Mailvelope browser extension


### Assurance Claims
1. The Nextcloud calendar form does not allow injection exploits.
   - PHP framework automatically handles script injection attacks
     - Developer has used “htmlspecialchars” function for output of any user input
   - Developer used parameterized queries
   - Developer used stored procedures for data access  
Similar for SQL injection attacks  
**Jim performed a manual code review for this Assurance Case**
2. Nextcloud sufficiently prevents unauthorized file access.
   - Role-based access controls are implemented correctly
   - Two-Factor Authentication is enabled effectively
   - Disk encryption prevents file access  
2FA is limited to text messages rather than Google authenticator, Duo, etc.
3. Nextcloud prevents malicious file uploads.
   - Antivirus scanner
   - Ransomware protection
4. Nextcloud does not leak authentication information
   - LDAP requests are encrypted
     - Keys are generated and stored securely
     - All unencrypted traffic is redirected to HTTPS
       - Nextcloud external security tools check for HTTP traffic
5. The Nextcloud Mail app is acceptably secure.
   - Mailvelope provides end-to-end encryption on email messages
 

### Threat Models
- Prevent application-level spoofing
  - Username/password authentication
  - RBAC
- Support encrypted communication across the internet threat boundary
  - Mitigate tampering and information disclosure
    - **If the internal network is not fully trusted, then unencrypted internal communication is a gap**
- Prevent SQL injection
  - Parameterized queries
- Prevent tampering by sanitizing output
  - PHP logic like “htmlspecialchars”
- Mitigate Denial of Service attacks
  - NGINX supports options for rate limiting and user memory limitations
- Prevent tampering
  - **A gap that should be addressed is that both NGINX and Nextcloud must support encrypted configuration files, or at least reading encrypted information from standard configuration files**
- Provide a secure file system data store
- Prevent unauthorized file read/write access
  - **Nextcloud’s out of the box settings will not suffice to meet standard security controls.**
- **Nextcloud’s optional anti-virus will need to be configured and enabled to meet standard security controls. Out of the box settings do not automatically enable this feature.**
- **Nextcloud’s optional server-side file encryption will need to be utilized to meet standard security controls. Out of the box settings do not automatically enable this feature.**
- Further investigation of the threat to see if it highlights a vulnerability within Nextcloud:
  - Threat 37
    - Potential Process Crash or Stop for Nextcloud
    - Description: Nextcloud crashes, halts, stops or runs slowly; in all cases violating an availability metric.
    - Justification: Nextcloud supports configuration backup and restore in cases of system crashes. No information is available regarding HA pairing or load balancing over multiple server instances to prevent unavailability.
  - Threat 41
    - Elevation by Changing the Execution Flow in Nextcloud
    - Description: An attacker may pass data into Nextcloud in order to change the flow of program execution within Nextcloud to the attacker's choosing.
    - Justification: Research is needed to discover if NextCloud has features to prevent changing the flow of process execution
  - Threat 45
    - Spoofing of Source Data Store Configuration File
    - Description: Configuration File may be spoofed by an attacker and this may lead to incorrect data delivered to Nextcloud. Consider using a standard authentication mechanism to identify the source data store.
    - Justification: If an attacker obtains access to internal network, then corruption of a clear text configuration file could cause issues.
  - Threat 55
    - Potential Lack of Input Validation for Nextcloud
    - Description: Data flowing across TCP may be tampered with by an attacker. This may lead to a denial of service attack against Nextcloud or an elevation of privilege attack against Nextcloud or an information disclosure by Nextcloud. Failure to verify that input is as expected is a root cause of a very large number of exploitable issues. Consider all paths and the way they handle data. Verify that all input is verified for correctness using an approved list input validation approach.
    - Justification: If database support encrypted communication this should be utilized instead of unencrypted TCP communication.
  - Threat 58
    - Data Flow Sniffing
    - Description: Data flowing across TCP may be sniffed by an attacker. Depending on what type of data an attacker can read, it may be used to attack other parts of the system or simply be a disclosure of information leading to compliance violations. Consider encrypting the data flow.
    - Justification: If database support encrypted communication this should be utilized instead of unencrypted TCP communication.
