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
Likely | | | | |
Possible | | DDos attack | Developer creating vulnerability in system | 
Unlikely | | | Hackers gaining access to source-code via Git-repositories | Hacker gaining root access, Hacker gaining access to database |
Rare | | | | DigitalOcean server downtime 

## Vulnerabilities found at group A

We haven't found any vulnerabilities which could be considered as really critical. 
We found a minor vulnerability earlier, but it didn't give us much access to the groups system or code. 
Group A had given us the link to their Grafana-page. Before we were given a login we just tried with admin-admin and we immediately got in their system. From here we could not do much as Grafana is mostly for viewing graphs and stats, but in theory we might be able to extract some information about their servers that they did not want us to know.

Furthermore we were told by group A that a hacker got access to their database and demanded a large sum in order to get their data back. Luckely the group only had unimportant test data but it still says something about their vulnerabilities. The group has since then fixed the exploit and didn't really lose anything from the attack. 
