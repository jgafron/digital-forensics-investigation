# Network Log Analysis — Technical Notes

## Data Sources

Worked from three log sources:

- RADIUS wireless authentication logs
- DHCP logs
- MAC address vendor information

The core of the analysis was correlating usernames, device identifiers, IP addresses, hostnames, access points, and authentication timestamps against each other.

## Device Correlation

Used `grep` and `awk` together to pull MAC addresses out of the authentication and DHCP logs.

| Device | MAC address | Manufacturer | DHCP hostname | IP address |
|---|---|---|---|---|
| Laptop | `14-10-9F-DD-C8-BD` | Apple | `laptopp10r051012` | Not documented |
| Mac mini | `28-CF-DA-05-6D-11` | Apple | `Fores-Mac-mini` | Not documented |
| iPhone | `68-A8-6D-55-66-DA` | Apple | `iphone` | `172.17.214.127` |
| MacBook Pro | `6C-40-08-92-D5-D0` | Apple | `Schmos-MBP` | `172.17.32.166` |
| Android phone | `8C-3A-E3-97-67-37` | LG | `android-v5133cf2ce5f5931` | `172.17.18.18` |

The two devices reported missing were:

```text
MacBook Pro: 6C-40-08-92-D5-D0
Android phone: 8C-3A-E3-97-67-37
```

## Wireless Access Points

The MacBook Pro connected through two access points: `332i-CampusNet` and `832i-CampusNet`.

## Suspicious Authentication Activity

The MacBook Pro's MAC address, `6C-40-08-92-D5-D0`, showed up in RADIUS records authenticating under the username `jerk` through `332i-CampusNet`.

Selected events:

2015-06-19 09:03:08 - Auth OK for jerk
2015-06-19 09:13:01 - Auth OK for jerk
2015-06-19 09:19:55 - Auth OK for jerk
2015-06-19 09:57:31 - Auth FAIL for jerk
2015-06-19 09:57:44 - Auth OK for jerk
2015-06-19 11:53:20 - Auth OK for jerk
2015-06-19 11:55:43 - Auth OK for jerk
2015-06-19 14:09:03 - Auth OK for jerk
2015-06-19 15:13:47 - Auth OK for jerk


The failed authentication at 9:57:31 AM was followed about thirteen seconds later by a successful one, from that same username and device MAC.

**Example RADIUS record:**

2015-06-19T09:57:44.183849-05:00 radius6.big.campus.edu radius.auth2[16448]:
[wireless] Auth OK for jerk
(Outer EAP Identity jerk)
on 6C-40-08-92-D5-D0
via 172.21.128.208:00-90-0B-28-85-CF:332i-CampusNet


Breaking that down:

- username: `jerk`
- device MAC: `6C-40-08-92-D5-D0`
- access point: `332i-CampusNet`
- access-point MAC: `00-90-0B-28-85-CF`
- network address in the record: `172.21.128.208`

## Interpretation

The strongest evidence was the repeated pairing of username `jerk` with device MAC `6C-40-08-92-D5-D0`, a MAC address that had previously belonged to John's MacBook Pro. Since that same device MAC kept authenticating under a different username, it points to the missing device being used by another account.

That said, the logs establish a device-to-account correlation, they don't independently prove who was physically behind the keyboard, or establish theft on their own. The portfolio framing reflects that: the logs support unauthorized use of the missing MacBook Pro, not a definitive conclusion that `jerk` stole it.

## Failed Logins for John's Account

The failed logins under `jschmo` weren't tightly clustered, they were spread out and generally followed by successful logins on the same account shortly after. That pattern doesn't really support a brute-force attempt or a concentrated compromise effort.

## Analytical Approach

Rather than leaning on one field, the investigation correlated across the full chain:

username
↓
RADIUS authentication event
↓
MAC address
↓
DHCP hostname
↓
IP address
↓
device type and manufacturer
↓
access point and timestamp


## Skills Demonstrated

- RADIUS authentication analysis
- DHCP log correlation
- `grep` and `awk` filtering
- MAC address attribution
- Vendor identification
- Hostname and IP correlation
- Access point analysis
- Authentication timeline reconstruction
- Distinguishing suspicious activity from unsupported conclusions

## Limitations

- A MAC address identifies a network interface, not the person using it.
- A successful login under a username doesn't by itself prove theft.
- The original source material didn't preserve the exact `grep`/`awk` command syntax used.
- IP addresses can change over time through DHCP.
- Failed logins are better interpreted by timing and context than by raw count alone.

Your folder structure is set:

text
case-studies/network-log-analysis/
├── README.md
└── technical-notes.md
