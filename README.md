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
Getting a "VirtualBox: "Mp-bios bug:8254 timer not connected to io-apic" during the install.
The emulated IO-APIC by Virtualbox is apparently wrong, which causes the VM to stop booting.
Using only one CPU core fixes the issue (IO-APIC was initially used to route hardware interrupts to the parallel CPUs), or adding "noapic" option to the GRUB when booting.
[APIC Wikipedia](https://en.wikipedia.org/wiki/Advanced_Programmable_Interrupt_Controller)

This virtual machine contains a wide range of tools, databases, open ports... including backdoors and misconfigs. It’s a good sandbox to start with.

## Debian VM

Typical VM creation, with Nessus and MSF installed, nothing much to say here.

## First scan

Using Nessus on the Debian VM to scan the Metasploitable2.
