# DevOps Linux Assignments

This directory contains solutions for the six assignment groups in `Assignments.md`. The shell scripts target Bash on Linux. Python scripts use only the Python 3 standard library. Run them from this directory, read each script first, and use administration scripts only on systems you are authorized to manage.

## Assignment 1 — Software installation

### Script 1: Install `curl`

```bash
bash assignment-1/install_curl.sh
```

Detects Debian/Ubuntu, Red Hat family, or Alpine from `/etc/os-release`, then selects APT, DNF/YUM, or APK. It asks for `sudo` when needed and exits successfully if `curl` is already installed. For an unsupported or unidentified distribution it writes exactly `Distro not identified` to standard error and exits nonzero.

Expected output: `curl is already installed (...)` or package-manager progress. On an unsupported OS: `Distro not identified` on stderr.

## Assignment 2 — File operations

### Script 1: List files

```bash
bash assignment-2/list_files.sh "/path/to/directory"
```

Prints regular files directly inside the directory, including hidden files. Expected output: one path per line.

### Script 2: Prefix `.txt` files

```bash
bash assignment-2/backup_txt_files.sh "/path/to/directory"
```

Renames `file.txt` to `backup_file.txt` without descending into subdirectories. It skips names already prefixed with `backup_` and reports collisions instead of overwriting. Expected output: one `old -> new` line per renamed file.

### Script 3: Compare files

```bash
bash assignment-2/compare_files.sh file-a.txt file-b.txt
```

Prints `Files are identical.` when the files match; otherwise it displays a unified diff and exits with status 1. Invalid or unreadable files produce an error on stderr.

## Assignment 3 — System information

### Script 1: System summary

```bash
bash assignment-3/system_info.sh
```

Prints distribution, kernel, CPU, and memory information. Expected output has `Operating system`, `Kernel`, `CPU`, and `Memory` sections.

### Script 2: Check or start a process

```bash
bash assignment-3/check_or_start_process.sh nginx nginx
```

Pass the process name first, then the command and arguments used to start it. It uses exact process-name matching (`pgrep -x`). Expected output is `Process is running: nginx` or a start confirmation. The example command should remain running in the background.

### Script 3: Disk-space email alert

```bash
bash assignment-3/disk_space_alert.sh / 80 admin@example.com
```

Prints current filesystem usage and emails an alert at or above the integer threshold. Requires a working `mail` command and configured mail transport. Expected output: `Disk usage for /: 42% (alert threshold: 80%)`; above threshold it sends an email or reports why it could not.

## Assignment 4 — Data manipulation

### Script 1: Sum a CSV column

```bash
python3 assignment-4/sum_csv_column.py sales.csv 3
```

Column numbers are one-based; the first CSV row is treated as a header. Python's CSV reader handles quoted fields containing commas. Values in the selected column must be numeric. Expected output: `Sum of column 3 (2 data rows): 25.50`.

### Script 2: Count IP addresses in logs

```bash
python3 assignment-4/count_request_ips.py access.log 100
```

Counts valid IPv4 address occurrences and prints addresses whose request count is strictly greater than the supplied threshold. Expected output is a two-column IP/count table, or a no-matches message.

### Script 3: Count words

```bash
python3 assignment-4/word_frequency.py "Red fish, blue fish!"
```

Counts case-insensitively and sorts by descending count, then alphabetically. Without a sentence argument, it prompts for one. Expected output includes `fish` with count `2`, and `blue` and `red` with count `1`.

## Assignment 5 — Network operations

### Script 1: Check SSH hosts

Create `hosts.txt` with one `user@host` or host alias per line; blank lines and `#` comments are ignored.

```bash
bash assignment-5/check_ssh_hosts.sh hosts.txt
```

Uses the existing SSH configuration and host-key checks, with batch mode and a five-second connection timeout. Public-key authentication should already be configured. Prints `REACHABLE` or `UNREACHABLE` per host.

### Script 2: Average ping time

Create `addresses.txt` with one IP address or hostname per line.

```bash
bash assignment-5/ping_average.sh addresses.txt 4
```

Sends four ICMP echo requests by default and reports average RTT when the host replies. Requires `ping`; firewalls may block ICMP. Expected output: `REPLY 192.0.2.10 average=12.3 ms` or `NO REPLY ...`.

### Script 3: Check a website

```bash
bash assignment-5/check_website.sh https://example.com
```

Follows redirects and considers final HTTP statuses from 200 through 399 up. Expected output: `UP https://example.com (HTTP 200)`. Requires `curl` and network access.

## Assignment 6 — User management

### Script 1: Add a user

```bash
sudo bash assignment-6/add_user.sh demo-user /home/demo-user
```

Creates the account and home directory, then uses the system `passwd` prompt so the password is not placed in shell history or command-line arguments. Run only with administrator authorization. Expected output confirms the created username and home path.

### Script 2: Check password strength

```bash
bash assignment-6/check_password_strength.sh
```

Reads the password without echoing it, then clears the shell variable. The example policy requires 12 characters, lowercase and uppercase letters, a digit, and a symbol. This educational check is not a substitute for an organization's policy or a breached-password check.

### Script 3: List last login times

```bash
bash assignment-6/list_users_last_login.sh
```

Displays the local account last-login table using `lastlog`. Expected output is the `lastlog` table, including “Never logged in” for accounts without a recorded login. Requires the distribution's `lastlog` utility.

## Notes

- Review commands and target paths before running scripts that rename files, install packages, send email, start processes, or create accounts.
- SSH and network results depend on routing, firewall rules, DNS, host-key setup, and remote service configuration.
- Scripts return zero on success and nonzero for invalid input or failures. The file-comparison script also returns nonzero when files differ.
