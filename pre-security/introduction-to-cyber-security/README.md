# Introduction to Cyber Security

Learned the difference between offensive and defensive security, showed how ethical hacking works, and practiced with a simulated attack on a fake bank website.

## Key Concepts Learned

### Offensive Security

Offensive security is about thinking like a hacker to find vulnerabilities before real attackers do. I practiced using a tool called `dirb` to find hidden pages in a fake banking site. 

I added $2000 to account 8881 through the hidden `/bank-deposit` page and confirmed the balance was updated. It showed how dangerous hidden but unprotected features can be.

### Defensive Security

Defensive security focuses on preventing attacks, detecting intrusions, and responding to incidents. Some key actions include patching systems, monitoring logs, setting up firewalls and IPS, and building user awareness. I explored major areas of defensive security:

- **SOC (Security Operations Center)**: Where analysts monitor systems and respond to alerts using tools like SIEMs.
- **Threat Intelligence**: Involves collecting and analyzing data to understand who the attackers are and how they operate.
- **DFIR (Digital Forensics and Incident Response)**: Focused on investigating attacks, collecting evidence, and responding.
- **Malware Analysis**: Learning how malicious software like viruses, trojans, and ransomware work

## Exercise

In this lab I worked as a SOC analyst using a simplified SIEM dashboard. I investigated suspicious login failures and connections from unknown IP addresses. 

I escalated the issue to the correct team member, blocked the IP on the simulated firewall, and completed the scenario. 

## Important Terms and Tools

- **dirb**: used to find hidden web pages by brute forcing URLs  
- **SIEM**: platform that aggregates and alerts on security events  
- **IPS / Firewalls**: tools that block or monitor suspicious network traffic  
- **AbuseIPDB / Cisco Talos**: tools for checking IP reputation  
- **Malware Types**: virus, trojan horse, ransomware  
- **Incident Response Phases**: preparation, detection, containment, recovery, and post incident review  
- **Digital Forensics Sources**: file system images, memory dumps, system logs, and network logs

---
