# Conditional Logic Lab

A hands-on home lab covering Microsoft identity end to end: on-prem Active Directory,
hybrid sync to Entra ID, Conditional Access, passwordless sign-in, privileged access,
identity governance and identity as code.

Each week ends with a write-up in this repo: the goal, the design, the steps, what broke,
the evidence, and what I would do differently in production.

> This is a personal lab. It contains no real company data, no production tenant details
> and no secrets. All users, groups and devices are fictional.

## What it covers

| Phase | Topic | Folders |
|---|---|---|
| 1 | Active Directory: build, FSMO, Group Policy, sites, hardening, AD CS | 01 to 04 |
| 2 | Hybrid identity: Entra Connect Sync, migration to Cloud Sync | 05 to 06 |
| 3 | Modern authentication: Conditional Access, passwordless, certificate-based auth | 07 to 08 |
| 4 | Privileged access: PIM, administrative units, custom roles | 09 |
| 5 | Identity governance: joiner/mover/leaver automation, access packages, reviews | 10 to 11 |
| 6 | Apps and B2B: OIDC and SAML SSO, app proxy, cross-tenant sync | 12 |
| 7 | Identity as code: GitHub Actions, workload identity federation, Maester | 13 |
| 8 | Monitoring and portfolio: KQL, workbooks | 14 |

## Lab environment

Proxmox VE on a single Dell OptiPlex, with pfSense handling routing and firewalling.
Each AD site has its own VLAN, and only AD traffic is allowed between them.

| Host | Role | Site | Network |
|---|---|---|---|
| LON-DC01 | Domain controller, DNS | London | VLAN 50, 10.10.50.10 |
| COV-DC01 | Domain controller, DNS | Coventry | VLAN 60, 10.10.60.11 |
| LON-CL01 | Windows 11 Enterprise client | London | VLAN 50, DHCP |

Hosts are named `SITE-ROLE##`.

## Naming plan

| Name | Used for |
|---|---|
| corp.internal (NetBIOS: CORP) | AD forest |
| conditionallogic.co.uk | Main Entra tenant, user UPN suffix, portfolio site |
| conditionallogic.uk | Partner tenant for B2B and cross-tenant sync |
| pki.conditionallogic.uk | Public HTTP location for certificate revocation lists |

**Why corp.internal:** I built the forest before buying the domains. `.internal` is reserved
by ICANN for private use, so it will never clash with a public domain. Users get
conditionallogic.co.uk as an additional UPN suffix, which is what Entra needs for sync.
For a new production forest I would use a subdomain of a domain the organisation owns.

## Repo layout

| Path | Contents |
|---|---|
| `00-setup` to `14-monitoring-portfolio` | One write-up per week |
| `diagrams/` | Lab diagrams, with their source files |
| `scripts/` | PowerShell and other scripts used in the lab |
| `templates/` | Write-up template |

## Rules for this repo

- No passwords, keys, certificates with private keys, client secrets, tenant IDs or public IP addresses.
- Screenshots and command output are checked and redacted before they are committed.

## Progress

Write-ups are linked here as they are finished.