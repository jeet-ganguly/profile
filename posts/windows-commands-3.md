# Windows Important Commands (Part 3)

> Windows provides several built-in networking commands that help administrators, SOC analysts, incident responders, and penetration testers troubleshoot connectivity, enumerate network configuration, inspect DNS settings, and identify shared resources.

These notes cover:

- Scheduled Tasks
- Network Information
- DNS Commands
- Shared Resources

---

# Table of Contents

- [1. Scheduled Tasks](#1-scheduled-tasks)
- [2. Network Information](#2-network-information)
- [3. DNS Commands](#3-dns-commands)
- [4. Shared Resources](#4-shared-resources)
- [5. Quick Revision Sheet](#5-quick-revision-sheet)

---

# 1. Scheduled Tasks

Scheduled Tasks are programs or scripts that Windows executes automatically at a specified time or when a particular event occurs.

They are commonly used for:

- Automatic backups
- System maintenance
- Software updates
- Script execution

---

## schtasks

Displays help information for the `schtasks` command.

Syntax:

```cmd
schtasks
```

---

## schtasks /query

Displays all scheduled tasks configured on the system.

Syntax:

```cmd
schtasks /query
```

Example Output:

```text
Windows Defender Scan

Disk Cleanup

Google Update
```

---

## schtasks /query /fo LIST /v

Displays detailed information about scheduled tasks.

Syntax:

```cmd
schtasks /query /fo LIST /v
```

Shows information such as:

- Task Name
- Author
- Status
- Trigger
- Next Run Time
- Task To Run

---

# 2. Network Information

Network Information commands help display network configuration, routing information, active connections, and connectivity status.

---

## ipconfig

Displays the basic IP configuration of the system.

Syntax:

```cmd
ipconfig
```

Shows:

- IPv4 Address
- IPv6 Address
- Subnet Mask
- Default Gateway

---

## ipconfig /all

Displays complete network configuration.

Syntax:

```cmd
ipconfig /all
```

Shows additional information such as:

- Host Name
- MAC Address
- DNS Servers
- DHCP Status
- Lease Information

---

## arp -a

Displays the ARP cache.

Syntax:

```cmd
arp -a
```

Shows:

- IP Address
- MAC Address
- Interface

---

## route print

Displays the routing table.

Syntax:

```cmd
route print
```

Shows:

- Network Routes
- Default Gateway
- Interface Metrics

---

## netstat -ano

Displays active network connections.

Syntax:

```cmd
netstat -ano
```

Shows:

- Local Address
- Remote Address
- Connection State
- Process ID (PID)

---

## ping <host>

Tests connectivity with another host.

Syntax:

```cmd
ping <host>
```

Example:

```cmd
ping google.com
```

---

## tracert <host>

Displays the route taken by packets to reach a destination.

Syntax:

```cmd
tracert <host>
```

Example:

```cmd
tracert google.com
```

---

## nslookup <domain>

Queries DNS records for a domain.

Syntax:

```cmd
nslookup google.com
```

Displays:

- DNS Server
- Resolved IP Address

---

# 3. DNS Commands

These commands help view and manage the local DNS resolver cache.

---

## ipconfig /displaydns

Displays the contents of the DNS Resolver Cache.

Syntax:

```cmd
ipconfig /displaydns
```

Useful for viewing recently resolved domain names.

---

## ipconfig /flushdns

Clears the DNS Resolver Cache.

Syntax:

```cmd
ipconfig /flushdns
```

Useful after:

- DNS changes
- Troubleshooting
- Cache poisoning recovery

---

## nslookup <domain>

Resolves a hostname to its IP address using DNS.

Syntax:

```cmd
nslookup example.com
```

---

# 4. Shared Resources

Windows allows files and folders to be shared across a network.

These commands help enumerate shared resources.

---

## net share

Displays all shared folders on the local computer.

Syntax:

```cmd
net share
```

Example Output:

```text
ADMIN$

C$

IPC$

Shared
```

---

## net view

Displays shared resources available on the local network.

Syntax:

```cmd
net view
```

---

## net view \\ComputerName

Displays shared folders available on a remote computer.

Syntax:

```cmd
net view \\ComputerName
```

---

## net use

Displays or manages mapped network drives.

Syntax:

```cmd
net use
```

Shows:

- Drive Letter
- Remote Share
- Connection Status

---

# 5. Quick Revision Sheet

## Scheduled Tasks

```cmd
schtasks
```

Display command help.

---

```cmd
schtasks /query
```

List scheduled tasks.

---

```cmd
schtasks /query /fo LIST /v
```

Display detailed scheduled task information.

---

## Network Information

```cmd
ipconfig
```

Display IP configuration.

---

```cmd
ipconfig /all
```

Display complete network configuration.

---

```cmd
arp -a
```

Display ARP cache.

---

```cmd
route print
```

Display routing table.

---

```cmd
netstat -ano
```

Display active network connections with PIDs.

---

```cmd
ping <host>
```

Test network connectivity.

---

```cmd
tracert <host>
```

Trace the network route.

---

```cmd
nslookup <domain>
```

Query DNS records.

---

## DNS Commands

```cmd
ipconfig /displaydns
```

Display DNS Resolver Cache.

---

```cmd
ipconfig /flushdns
```

Clear DNS Resolver Cache.

---

```cmd
nslookup <domain>
```

Resolve a hostname using DNS.

---

## Shared Resources

```cmd
net share
```

Display local shared folders.

---

```cmd
net view
```

Display shared resources on the local network.

---

```cmd
net view \\ComputerName
```

Display shares on a remote computer.

---

```cmd
net use
```

Display or manage mapped network drives.

---

*End of Part 3*