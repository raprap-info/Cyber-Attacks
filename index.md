🔍 Malware Investigation Report: Perl CGI Web Shell
📄 Script Overview
Filename (likely): Unknown (could be cmd.cgi, shell.pl, etc.)
Interpreter: Perl (#!/usr/bin/perl)
Execution Type: CGI (Common Gateway Interface)
Purpose: Executes base64-decoded shell commands received via POST requests.

⚙️ Functionality Summary
This script:

Accepts HTTP POST requests.

Decodes POST parameters using URL-decoding.

Base64-decodes the values of two fields:

check: Displayed (for human-readable message).

cmd: Executed as a shell command using system().

Outputs results in an HTML <pre> block.

✅ Example Request (Exploit):
http
Copy
Edit
POST /cgi-bin/shell.pl HTTP/1.1
Host: victim.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 58

check=U2VydmVyIEV4ZWN1dGlvbjo=&cmd=bHMgLWFs
Which decodes to:

perl
Copy
Edit
check = "Server Execution:"
cmd = "ls -al"
🛑 Security Risks
This script presents a critical security vulnerability:

Risk	Description
Remote Code Execution	Arbitrary system commands can be run remotely.
Privilege Escalation	If CGI runs as root or high-privilege user, full system access may be possible.
Persistence	Attacker could upload persistent malware, backdoors, or reverse shells.
Lack of Authentication	No login or token required—anyone with access can exploit it.

🔍 Forensic Investigation Steps
Locate the Script

Search for .pl, .cgi, or .txt files under:

swift
Copy
Edit
/var/www/cgi-bin/
/usr/lib/cgi-bin/
/home/*/public_html/
Example:

bash
Copy
Edit
sudo find / -type f -name "*.pl" -o -name "*.cgi"
Check Web Logs

Look at access logs to find if the script was accessed:

bash
Copy
Edit
sudo grep -i 'cmd=' /var/log/apache2/access.log
Also review:

swift
Copy
Edit
/var/log/httpd/
/var/log/nginx/
Review Modification Times

bash
Copy
Edit
sudo find / -type f -iname "*.pl" -printf "%TY-%Tm-%Td %TT %p\n" | sort
Check for Other Backdoors

Scan for other base64-using files:

bash
Copy
Edit
sudo grep -r "decode_base64" /
Assess File Permissions

Who owns the script?

Who has write/execute access?

Check Running Processes

bash
Copy
Edit
ps aux | grep perl
Network Connections (optional)

bash
Copy
Edit
sudo lsof -i -n -P | grep perl
🛠️ Remediation Steps
Immediately delete or quarantine the script.

Audit all web-accessible directories for other malicious files.

Revoke compromised credentials, especially if root or sudo was used.

Check for new cron jobs or startup scripts:

bash
Copy
Edit
crontab -l
sudo ls -l /etc/cron*
Consider restoring from a known-clean backup.

Update all software (Perl, Apache, etc.).

Apply strict file permissions:

CGI scripts: chmod 700

Remove write access from the web server user where not needed.

✅ Prevention Tips
Never allow unauthenticated command execution via web interfaces.

Use server-side input validation and authentication.

Disable CGI entirely unless needed.

Monitor logs for suspicious activity.
