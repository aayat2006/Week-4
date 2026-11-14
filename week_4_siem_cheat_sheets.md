# Week 4 - SIEM & SPL Reference Cheat Sheets

## Table 1: SPL Command Reference (15 Essential Commands)

| # | SPL Command | Meaning | Practical Example |

| 1 | `search` | Filters incoming raw events by index, sourcetype, or keyword. | `index=lab sourcetype=syslog "Failed password"` |
| 2 | `stats` | Computes aggregate statistics such as `count`, `sum`, `avg`, or `dc`. | `index=lab \| stats count by src_ip` |
| 3 | `where` | Filters search results using boolean expressions and field comparisons. | `index=lab \| stats count by src_ip \| where count > 5` |
| 4 | `eval` | Creates new calculated fields or transforms existing field values. | `index=lab \| eval status=if(EventCode==4624, "Success", "Fail")` |
| 5 | `table` | Displays only specified fields in a clean, structured tabular format. | `index=lab \| table _time, host, user, src_ip` |
| 6 | `timechart` | Aggregates statistical metrics over discrete time intervals (`span`). | `index=lab \| timechart span=5m count by process` |
| 7 | `rex` | Extracts custom fields from unstructured raw logs using regular expressions. | `index=lab \| rex field=_raw "from (?<src_ip>\d+\.\d+\.\d+\.\d+)"` |
| 8 | `rename` | Renames one or more fields for readable report or dashboard headers. | `index=lab \| stats count by src_ip \| rename src_ip AS "Source IP"` |
| 9 | `sort` | Orders results in ascending (`+`) or descending (`-`) sequence. | `index=lab \| stats count by user \| sort - count` |
| 10 | `dedup` | Removes duplicate events based on matching values of specified fields. | `index=lab \| dedup src_ip, user` |
| 11 | `head` | Returns the first N results from the search pipeline. | `index=lab \| sort - _time \| head 10` |
| 12 | `top` | Displays the most frequent values of a field along with count and percentage. | `index=lab \| top limit=5 TargetUserName` |
| 13 | `rare` | Displays the least frequent values of a field to spot anomalies. | `index=lab EventCode=1 \| rare ParentImage, Image` |
| 14 | `lookup` | Enriches events by matching field values against an external CSV lookup table. | `index=lab \| lookup threat_intel.csv ip AS src_ip OUTPUT threat_level` |
| 15 | `fillnull` | Replaces null/empty field values with a default string or number. | `index=lab \| fillnull value="Unknown" user` |

---

## Table 2: SIEM Alert Triage Playbook (10 Common Alerts)

| # | SIEM Alert | Likely Cause | First Check |

| 1 | **High Volume Failed Logons (EventCode 4625 / SSH)** | Password spraying or automated dictionary brute-force attack (T1110). | Check if all failures originate from a single `src_ip` and verify whether a successful logon (`4624` / `Accepted password`) followed. |
| 2 | **Encoded PowerShell Execution (Sysmon ID 1)** | Malicious script execution, downloader cradle, or reverse shell payload (T1059.001). | Extract the `CommandLine` field and decode the Base64 string in a safe sandbox to inspect the payload URL or commands. |
| 3 | **New Local User Account Created (EventCode 4720)** | Unauthorized attacker persistence or legitimate IT onboarding (T1136.001). | Verify the `SubjectUserName` (creator account) and cross-check against active IT change management tickets. |
| 4 | **Windows Security Audit Log Cleared (EventCode 1102)** | Adversary attempting defense evasion by wiping forensic tracks (T1070.001). | Identify the user session and source host that triggered the clear event and isolate the endpoint immediately. |
| 5 | **File Integrity Monitoring (FIM) Change on Core Binary** | Rootkit installation, unauthorized patching, or binary tampering. | Compare the new file SHA-256 hash against known vendor baselines and check recent administrative maintenance windows. |
| 6 | **Rare Parent-Child Process Execution** | Process injection or exploitation (e.g., `winword.exe` spawning `cmd.exe`). | Inspect the parent process file path, user context, and any outbound network connections (`Sysmon ID 3`). |
| 7 | **Internal Port Scan / Multiple Connection Failures** | Lateral movement reconnaissance across internal subnets (T1046). | Check the originating internal host IP, destination port distribution, and whether the host belongs to an authorized vulnerability scanner. |
| 8 | **Outbound Traffic to Known Malicious IP / C2 Port** | Command-and-Control (C2) beaconing or active data exfiltration (T1071). | Query the destination IP/domain in Threat Intelligence feeds and inspect firewall bytes-out volume. |
| 9 | **Execution from `/tmp` or `AppData\Local\Temp`** | Malware dropper or web shell payload executing from world-writable directories. | Retrieve the binary hash from Sysmon Event ID 1 and check process creation command-line arguments. |
| 10 | **Rapid File Renaming / High Disk Write Activity** | Active ransomware encryption routine modifying user files (T1486). | Identify the host and process PID performing the writes and immediately isolate the endpoint from the network. |