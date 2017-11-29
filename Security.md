# Security Rapport

Every group member was present and contributed during the assignment. 

## Threat Model

### Assets

#### Script kiddies

There are a lot of easily accessible software for DDOS’ing and such. A DDOS attack can take your servers offline for a while and cause costly downtime.

### Threats 

Among other threats are:
- Brute force attacks (trying to gain access to the server/user accounts)
- Port scrapers, searching for open ports on our remote servers
- Injection attacks (SQL/Script)
- Internal threats: Developers being careless with their passwords/sensitive information/source code.


### Vulnerabilities

- Lackluster password security eg. having passwords in the github repository
- Compromising account security by storing passwords in plain text
- Insecure passwords (“1234”, “password” etc…)
- Not sanitizing user input (to prevent injection) 
- Vulnerabilities in libraries used/runtime environment
- The guy who knows nothing about IT and opens phishing mails exposes your system from within.

## Risk Matrix

| | Negligible | Marginal | Critical | Catastrophic |
--- | --- | --- | --- | --- |
Certain | Port scraping | 
Likely | | | Bots gaining access to database | |
Possible | | DDos attack | Developer creating vulnerability in system | 
Unlikely | | | Hackers gaining access to source-code via Git-repositories | Hacker gaining root access |
Rare | | | | DigitalOcean server downtime 

## Vulnerabilities found at group A

