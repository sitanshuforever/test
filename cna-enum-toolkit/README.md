# CNA Enum Toolkit

Cobalt Strike Aggressor Script toolkit for host and AD enumeration during authorized red team engagements.

## Structure

```
cna-enum-toolkit/
├── enum_toolkit.cna          # Main loader — load this one file
└── modules/
    ├── sys_enum.cna          # System / host enumeration
    ├── net_enum.cna          # Network enumeration
    ├── ad_enum.cna           # Active Directory enumeration
    └── user_enum.cna         # Users, sessions, credential surface
```

## Loading

In the Cobalt Strike Aggressor console:

```
aggressor> load /path/to/cna-enum-toolkit/enum_toolkit.cna
```

Or via the Cobalt Strike UI: **Cobalt Strike → Script Manager → Load**.

## Modules

### sys_enum.cna
| Alias | What it does |
|---|---|
| `sysinfo` | OS version, hostname, systeminfo, hardware |
| `proclist` | Running processes (bps + tasklist csv) |
| `services-enum` | All services via sc and wmic |
| `tasks-enum` | Scheduled tasks |
| `software-enum` | Installed software via wmic + registry |
| `env-enum` | Environment variables and PATH |
| `drives-enum` | Logical drives, network shares, mapped drives |
| `reg-autorun` | HKLM/HKCU Run/RunOnce registry keys |
| `av-enum` | AV/EDR detection via SecurityCenter2 + Defender |
| `uac-check` | UAC configuration and LocalAccountTokenFilterPolicy |
| `sys-enum-all` | Runs all of the above |

### net_enum.cna
| Alias | What it does |
|---|---|
| `ipconfig-enum` | IP config, NIC details via wmic |
| `netconn-enum` | Active connections (netstat -ano) |
| `arp-enum` | ARP cache |
| `route-enum` | Routing table |
| `dns-cache` | DNS client cache |
| `hosts-file` | Contents of hosts file |
| `firewall-enum` | Firewall profiles and all rules |
| `proxy-enum` | IE/WinHTTP proxy settings |
| `wifi-enum` | Wireless profiles (incl. saved key dump) |
| `smb-sessions` | SMB sessions, open files, statistics |
| `port-scan-local` | Locally listening ports |
| `net-enum-all` | Runs all of the above |

### ad_enum.cna
| Alias | What it does |
|---|---|
| `domain-info` | Domain name, role, joined status |
| `dc-enum` | Domain controllers list |
| `domain-users` | All domain users |
| `domain-groups` | All domain groups |
| `da-enum` | Domain Admins, Enterprise Admins, Schema Admins |
| `spn-enum` | Kerberoastable accounts (SPN set) |
| `asrep-enum` | AS-REP roastable accounts |
| `gpo-enum` | GPOs and gpresult |
| `trust-enum` | Domain/forest trusts |
| `ou-enum` | Organizational Units |
| `computers-enum` | Domain computers with OS info |
| `unconstrained-enum` | Unconstrained delegation |
| `constrained-enum` | Constrained delegation (msDS-AllowedToDelegateTo) |
| `ad-enum-all` | Runs all of the above |

### user_enum.cna
| Alias | What it does |
|---|---|
| `whoami-full` | Full token context, privs, groups |
| `local-users` | Local accounts and groups |
| `logged-on` | Active sessions (query user/session) |
| `password-policy` | Local + domain password policy |
| `laps-check` | LAPS deployment indicators |
| `sam-check` | SAM hive accessibility |
| `cred-files` | Disk search for credential/config files |
| `vault-enum` | Windows Credential Manager |
| `dpapi-blobs` | DPAPI master keys and credential blobs |
| `rdp-saved` | Saved RDP connection registry entries |
| `token-privs` | Abusable token privileges |
| `user-enum-all` | Runs all of the above |

## Auto-Enum on Beacon Check-In

`enum_toolkit.cna` hooks `beacon_initial`. When a new beacon first phones home it automatically runs a low-noise subset:

- `whoami /all`
- `hostname`
- `ipconfig /all`
- `net user /domain`
- Domain-joined check
- Abusable privilege check

## Usage Notes

- All commands execute in the beacon's current token context. Impersonate before running domain queries when operating as a local user.
- `ad_enum.cna` relies on `Get-ADUser` / `Get-ADComputer` — these require RSAT AD module. For environments without RSAT, `nltest`, `net.exe`, and raw LDAP queries are used as fallback.
- Right-click menu entries are grouped under **Enum: System**, **Enum: Network**, **Enum: Active Directory**, **Enum: Users & Creds**, and **Enum: [FULL SUITE]** in the beacon table context menu.

## Legal

For authorized red team engagements only. Ensure written authorization covers all target systems before use.
