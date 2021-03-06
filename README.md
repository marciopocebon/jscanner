# JScanner
Analyze target Joomla! installation using several different techniques.

## Why another tool?
Currently there are several tools that will detect the installed Joomla! version, you have online resources and even a Metasploit auxiliary module. However they perform a very simply check: they try to download the manifest file of Joomla! and read the version inside it.  
Sadly this method is not very reliable: such XML file could (and will) be deleted by scrupolous webmasters, moreover it's easy to block any request that tries to fetch any "internal" file.  

## The solution
JScanner will try several ways:

1. **Read the `joomla.xml` manifest file** 100% accuracy, but usually this file is missing or blocked
2. **Scan `com_admin` folder for SQL files** Every Joomla! version ships with a different SQL files for updates. By checking their presence, JScanner can build a list of possible candidates
3. **Fingerprint media files** If everything fails, we can still try to fetch media files. For each version, a signature has been generated and we are going to compare it vs the remote source.

## Usage
```
python jscanner.py analyze -u http://www.example.com

JScanner 1.0.0 - What's under the hood?
Copyright (C) 2016 FabbricaBinaria - Davide Tampellini
===============================================================================
JScanner is Free Software, distributed under the terms of the GNU General
Public License version 3 or, at your option, any later version.
This program comes with ABSOLUTELY NO WARRANTY as per sections 15 & 16 of the
license. See http://www.gnu.org/licenses/gpl-3.0.html for details.
===============================================================================
[*] Analyzing site http://www.example.com
[*] Trying to get the exact version from the XML file...
[*] Trying to detect version using SQL installation files...
[*] Trying to detect version using media file fingerprints...

[+] Detected Joomla! versions: 3.6.3, 3.6.4
```

## Other commands
#### Enumerate users and email addresses
Given a list of usernames or email addresses, you can check if they are actually used on target website (requires user registration being enabled).  
```
python jscanner.py enumerate -u localhost/vergine -U users.txt 
JScanner 1.1.0 - What's under the hood?
Copyright (C) 2016 FabbricaBinaria - Davide Tampellini
===============================================================================
JScanner is Free Software, distributed under the terms of the GNU General
Public License version 3 or, at your option, any later version.
This program comes with ABSOLUTELY NO WARRANTY as per sections 15 & 16 of the
license. See http://www.gnu.org/licenses/gpl-3.0.html for details.
===============================================================================
[*] Checking if URL http://www.example.com is online
[+] Site http://www.example.com seems online
[*] Checking if user registration is enabled
[+] User registration seems enabled
[*] Trying to fetch CSRF token
[*] Got CSRF token: 2f48a75c99e06d8723396c14ff2c2884
[*] Trying to fetch username usage
[+] Found used username: admin
```
## Known issues
Currently JScanner works only on version 3.x of Joomla!. More versions could be added in the future.
