# Linux Commands - Bandit 0-10 Reference

| Command | What it does | Example |
|---|---|---|
| `ssh` | Remote login | `ssh bandit0@host -p 2220` |
| `ls` / `ls -a` | List files / show hidden | `ls -a` |
| `cd` | Change directory | `cd inhere` |
| `cat` | Read a file | `cat readme` |
| `file` | Identify file type | `file ./*` |
| `find` | Search for files by property | `find / -user bandit7 -size 33c` |
| `grep` | Search inside files | `grep millionth data.txt` |
| `sort` | Sort lines alphabetically | `sort data.txt` |
| `uniq -u` | Show unique lines only | `sort data.txt \| uniq -u` |
| `strings` | Extract readable text from binary | `strings data.txt \| grep =` |
| `\|` (pipe) | Chain command outputs | `sort file \| uniq -u` |

## Path notation
- `/` root, top of the filesystem
- `./` current directory
- `../` parent directory
- `~` home directory
