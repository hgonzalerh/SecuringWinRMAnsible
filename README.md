# Securing WinRM for Ansible

Normally, if no automation is implemented across an organization, all WinRM services will be flagged by security teams. These security teams will recommend closing WinRM services at the OS and firewall levels. This is appropriate and reasonable only if no automation will be part of our processes.

When Ansible is used, WinRM services listening on Windows are the way connections are natively opened to the managed node. This is more secure than any ad-hoc agent that will continuously run on your operating system. At this point, WinRM services are **required** if automation is to be implemented.

> Ansible uses *no agents* to connect to any machine or endpoint. It uses the native connection method (ssh for Linux, WinRm for Windows, http for Vcenter, etc)

Make sure you follow the following guidelines to configure WinRM so that it will remain secure:

## Use only HTTPS on your WinRM listener 

* configure an https listener on port 5986 and attach a valid organizational or public certificate to it
* disable or delete any plain http listener
* maintain this configuration using a GPO 

## Listen to WinRM only in the addresses you intend to

* if a machine has more than one IP address, enable the listener only for the addresses that should be reachable from Ansible, removing localhost as a listening address
* configure Windows Firewall for allowing port 5986 connections only on those addresses

## Restrict WinRM listeners to connections from Ansible Automation Platform

* no other servers in the network should connect to WinRM, so restrict incoming connections to WinRM to be ** from execution nodes and hybrid nodes in the Ansible cluster **
* configure Windows firewall, enterprise firewalls, firewall appliances and security groups to block connections from hosts others than Ansible execution nodes and hybrid nodes

## Stengthen authentication and passwords

* use only NTLM or Kerberos as authentication methods, avoiding Basic authentication
* enforce strong passwords using a GPO

## Audit and monitor WinRM activity

* enable Windows Event Logging

## Configure PowerShell remoting

* restrict access to PowerShell remoting to only the user that Ansible will employ
* enable PowerShell Script Block Logging

## Escalate privilegs from a normal user account

* configure runas to escalate privileges to Administrator from a regular service account

* 
