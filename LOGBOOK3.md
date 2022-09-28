# Trabalho realizado na Semana #3

## Identification
- CVE 2021-26084
- Critical severity CVE, allowing unauthenticated attacker to inject remote code execution vulnerability in Confluence Server and Data Center instances.		
- Certain versions are still affected today

## Catalogation

- 9.8, considered a critical CVE
- Benny Jacob (SnowyOwl) published it on the Atlassian at  08/30/2021
- Before it was discovered, around 13000 hosts were exploitable
- Average payout on the bug bounty program was 923$

## Exploit

- An Object-Graph Navigation Language (OGNL) injection was used to take advantage of system weaknesses
- Remote code execution by targeting HTML fields interpreted and rendered by OGNL.

## Attacks

- An attacker can leverage this vulnerability to execute any command with the same permissions as the user-id running the service.
- An attacker can then use this access to gain elevated administrative permissions if the host has unpatched local vulnerabilities
- Attackers installed the XMRig cryptocurrency miner on vulnerable Confluence servers on both Windows and Linux to take advantage

<br>

## CTF
### Resolution
-In the rules section we copied the exemple flag as the solution.
