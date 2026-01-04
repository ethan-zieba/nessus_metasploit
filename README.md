# Discovery and Exploitation of Vulnerabilities

## Nessus ?

Nessus vulnerability assessment platform.
Can be used with or without agents.
Can detect OS vulnerabilities, network vulnerabilities, checks in huge CVE databases for known vulnerabilities on most systems. 
It can identify software flows, malware, misconfigs. 
Supports different scoring systems.
Thorough reporting.
Vast plugin database.
NASL (Nessus Attack Scripting Language) (similar to Perl/C) for writing custom plugins and scan templates.

## Metasploit ?

Huge framework for penetration testing.
Vast library of payloads, focused on active testing.
Vuln-exploit, post-exploit, privesc and lateral-movement testing. 

## Metasploitable 2

A Rapid7 virtual machine running Ubuntu 8.04, widely known to be vulnerable to numerous attacks.
Running it in a host-only network, using VirtualBox.

## Debian VM

Typical VM creation, with Nessus and MSF installed, nothing much to say here.

## First scan

Using Nessus on the Debian VM to scan the Metasploitable2.
