# Discovery and Exploitation of Vulnerabilities

## Nessus ?

Nessus is a vulnerability assessment platform.
It can be used with or without agents.
It detects OS and network vulnerabilities, and checks large CVE databases for known issues across systems.
It can identify software flows, malware, misconfigs. 
Supports different scoring systems.
Thorough and clear reporting.
It has a vast plugin database.
It uses NASL (Nessus Attack Scripting Language) (similar to Perl/C) for writing custom plugins and scan templates.
[Nessus Wikipedia](https://en.wikipedia.org/wiki/Nessus_(software))
[Tenable Documentation](https://docs.tenable.com/)

## Metasploit ?

Metasploit is a huge framework for penetration testing.
It has a vast library of payloads, with a strong focus on active testing.
Vuln-exploit, post-exploit, privesc and lateral-movement testing. 

## Metasploitable 2

A Rapid7 virtual machine running Ubuntu 8.04, widely known to be vulnerable to numerous attacks.
Running it in a host-only network, using VirtualBox.

May get a "VirtualBox: "Mp-bios bug:8254 timer not connected to io-apic" during installation.
This happens because the emulated IO-APIC by Virtualbox is apparently wrong for this Linux kernel, which causes the VM to hang during boot.
A simple workaround is to use only one CPU core (IO-APIC was initially used to route hardware interrupts to the parallel CPUs), or to add the `noapic` option in the GRUB when booting.
[APIC Wikipedia](https://en.wikipedia.org/wiki/Advanced_Programmable_Interrupt_Controller)
Linux 2.6 was far less forgiving of missing APIC routings, as they assumed a legacy PC interrupt model, and treated newer ones as a hardware defect. VirtualBox APIC emulates a more modern APIC model, which the Linux kernel doesn't handle well, leading to boot hangs or failures.

This virtual machine contains a wide range of tools, databases, open ports, along with backdoors and misconfigs. It’s a good sandbox to start with.

## Debian VM

Typical VM creation, with Nessus and MSF installed, nothing much to say here.

## First scans

Using Nessus on the Debian VM to scan the Metasploitable2.
First, we will start a Host Discovery scan, which in this case reveals basic information.
Since the two VMs are on an isolated network, the scan only discovers my host machine, the Debian VM running Nessus, and the Metasploitable 2 VM.
The Host Discovery scan is mainly ICMP-, TCP-, UDP-, ARP-based. It uses various types of probes to figure out which hosts and devices are alive on the network. It focuses on host availability.

The Basic Network Scan goes further. It uses more probes, active plugins, OS fingerprinting, service enum, vulnerability assessment. If credentials are provided, it can log into systems to spot misconfigurations. It checks for known CVEs and outdated software. 

