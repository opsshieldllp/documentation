# cPGuard IPDB Firewall

## Overview
The cPGuard IPDB Firewall is a system-level security module designed to block malicious traffic before it reaches your server or applications. It uses a distributed threat intelligence system to identify and block known abusive IP addresses in real time.

## How IPDB Firewall Works

### Cloud Advisor
- Maintains a global database of malicious IPs
- Collects data from logs, WAF, brute-force systems, and partners
- Scores IPs using behavior analysis

### Server Agent
- Downloads malicious IP lists
- Applies rules using IPSET and IPTABLES
- Updates automatically

## Key Features
- Real-time IP reputation blocking
- Reduced server load
- Automatic updates
- Early-stage attack prevention

## Prerequisites
- cPGuard installed
- Root access
- Firewall enabled

## Enable IPDB Firewall

### UI
Navigate to:
cPGuard >> Protection >> Firewall

Enable:
IPDB Switch

### CLI
```bash
cpgcli fw --ipdb enable
```

## Verify IPDB

Check logs:
```bash
/var/log/messages
/var/log/kern.log
```

Example:
IPDB Blocked: SRC=x.x.x.x

## Check IP
```bash
cpgcli ip --check <IP>
```

## Whitelist IP
```bash
cpgcli ip --allow <IP> --reason "reason"
```

## Disable IPDB
```bash
cpgcli fw --ipdb disable
```

## Notes
- Keep enabled for protection
- Monitor logs
- Whitelist trusted IPs

## Troubleshooting

### Not Blocking
- Check firewall status
- Verify IPDB enabled

### False Positives
- Whitelist IP
- Review logs

## Conclusion
IPDB Firewall provides proactive protection by blocking malicious IPs before they reach your server.
