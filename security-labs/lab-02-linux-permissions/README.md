# Lab 02: Linux Permissions & Access Control

**Domain:** Access Control · **Maps to:** CWE-732 (Incorrect Permission Assignment for Critical Resource)
**Tools:** `useradd`, `passwd`, `su`, `mkdir -p`, `touch`, `chmod`, `chown`, `chgrp`, `ls -l`

## Objective
Build the shared directory tree a small startup might keep under `/opt/data`, then lock it down so each employee can reach only the files their role needs.

## Approach
- Created user accounts for each role and switched between them with `su` to test access from each user's point of view.
- Built the directory tree with `mkdir -p` and populated it with `touch`.
- Set **ownership** (`chown`, `chgrp`) and **permission bits** (`chmod`, symbolic and octal) on every directory and file.
- Checked the result with `ls -la` at each level, then ran the lab's automated evaluation script against the final layout.

## Key concepts
| Concept | Why it matters |
|---|---|
| `r/w/x` mean different things on files and directories | On a directory, `x` lets you traverse it and `r` lets you list it. A file can be readable while its parent directory still blocks access to it. |
| Owner / group / other | Group ownership is the main way to grant shared access without making files world-readable. |
| Root bypasses DAC | The superuser ignores permission bits, so testing as root proves nothing. You have to test as each real user. |

## Defensive takeaways
- Default to **deny**, then grant the smallest set of permissions each role needs.
- Audit for world-writable files and directories (`find / -perm -o+w`). They are a common path to privilege escalation.
- Test access as the actual user, not as an administrator.
