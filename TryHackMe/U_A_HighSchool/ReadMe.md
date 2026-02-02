U.A. High School — TryHackMe
Sanitized Write-Up (Public Version)

NOTE:
This write-up is shared for educational purposes only.
Sensitive values, flags, and exact secrets have been redacted in accordance with TryHackMe guidelines.

--------------------------------------------------

Overview

This room focuses on web enumeration, remote code execution, steganography, and Linux privilege escalation.
The objective was to gain initial access, escalate privileges, and obtain both user-level and root-level access.

--------------------------------------------------

Enumeration

- Identified the target IP address provided by the lab
- Conducted a port scan using Nmap to discover exposed services
- Found HTTP (port 80) open
- Manually explored the web application and reviewed page source
- Performed directory enumeration and discovered a hidden PHP endpoint

--------------------------------------------------

Web Exploitation

- Identified an endpoint that accepted user-controlled parameters
- Performed parameter fuzzing and discovered a vulnerable parameter
- Confirmed Remote Code Execution (RCE)
- Used the RCE to execute system commands on the target
- Established a reverse shell connection to the attacker machine
- Stabilized the shell for interactive use

--------------------------------------------------

Post-Exploitation

- Enumerated the filesystem for useful artifacts
- Discovered an encoded passphrase file
- Decoded the file to retrieve a credential-related secret (REDACTED)
- Found an unused image file during further enumeration
- Identified a mismatch between the file extension and its magic number
- Repaired the file signature to make the image readable
- Performed steganography analysis
- Extracted credentials for a low-privileged user (username disclosed, password redacted)

--------------------------------------------------

User Access

- Logged in via SSH using the extracted user credentials
- Successfully obtained the user flag:
  THM{REDACTED}

--------------------------------------------------

Privilege Escalation

- Enumerated sudo permissions
- Identified a script executable with elevated privileges
- Analyzed the script and observed unsafe handling of user input
- Leveraged output redirection to write arbitrary content to files
- Generated an SSH key pair on the attacker machine
- Injected the public key into the root user's authorized_keys file
- Logged in as root using SSH key authentication
- Obtained the root flag:
  THM{REDACTED}

--------------------------------------------------

Skills Demonstrated

- Web service enumeration
- Directory and parameter fuzzing
- Remote Code Execution exploitation
- Reverse shell handling and shell stabilization
- File signature analysis (magic numbers)
- Steganography
- SSH authentication abuse
- Linux privilege escalation

--------------------------------------------------

Disclaimer

This write-up does not include flags, passwords, or direct solutions.
It is intended to demonstrate methodology and learning outcomes only.
