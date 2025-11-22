# Born2beRoot

Born2beRoot is a system administration project from the 42 / Codam curriculum. It consists of building and hardening a minimal Linux server running inside a virtual machine, with a strong focus on security, user management, and basic monitoring.

This project was completed with both the **mandatory requirements and the bonus part** implemented on the virtual machine.

---

## 📚 Overview

The goal of the project is to:

- Set up a minimal Linux server in a virtual machine (no graphical interface).
- Use encrypted LVM partitions for storage.
- Configure network access through SSH on a non-standard port.
- Enforce a strict password and sudo policy.
- Enable a firewall and basic MAC (Mandatory Access Control) system.
- Write a Bash monitoring script that periodically reports key system metrics.
- Extend the server with additional services as bonus work.

Because the evaluation is performed directly on the virtual machine, the VM image itself is **not stored** in this repository. Instead, this repo contains the elements required by the subject (such as the disk signature) plus selected scripts and configuration files.

---

## 🧱 Mandatory features

### 1. Virtual machine & operating system

- Virtual machine created with **VirtualBox / UTM**.
- Minimal Linux installation **without any graphical interface** (no X.org or similar).
- Disk layout built using **LVM with at least two encrypted logical volumes** (for example: root, swap, /home, /var, etc.).
- The system is configured to **boot directly into the console**.

### 2. Security & network hardening

- **SSH server** enabled and listening on port **4242** instead of the default port 22.
- **Root login via SSH is disabled**; only regular users can connect via SSH.
- A host firewall is enabled:
  - **UFW** (on Debian-based systems) or **firewalld** (on Rocky Linux).
  - Only port **4242** is allowed by default; other ports are blocked unless explicitly opened for bonus services.
- **AppArmor** (Debian) or **SELinux** (Rocky) is enabled at boot and kept in enforcing/active mode.

### 3. Users & groups

- The system contains:
  - The `root` user.
  - A non-root user whose login matches my 42/Codam login.
- This user:
  - Belongs to the `user42` group.
  - Belongs to the `sudo` group to allow privilege escalation with `sudo`.

During evaluation, the system is ready to create and configure additional users and groups as required.

### 4. Password policy

A strong global password policy is enforced using the system’s authentication stack (PAM, `chage`, and related tools):

- Passwords expire every **30 days**.
- Minimum time between password changes: **2 days**.
- Users receive a warning **7 days** before their password expires.
- Passwords must:
  - Be at least **10 characters** long.
  - Contain at least **one uppercase letter**, **one lowercase letter**, and **one digit**.
  - Not contain more than **three identical consecutive characters**.
  - Not contain the username.
- When changing passwords:
  - At least **7 characters** must differ from the previous password (except for the root user where this rule is relaxed).
- The **root password** also follows the same complexity rules.

After configuring the policy, all existing accounts (including `root`) have their passwords updated to comply with it.

### 5. Sudo configuration

`sudo` is configured with a hardened policy:

- Authentication is limited to **3 attempts** per command.
- A **custom error message** is displayed when an incorrect password is entered.
- All commands executed through `sudo` are **logged**:
  - Inputs and outputs are archived under `/var/log/sudo/`.
- **TTY mode** is enforced so `sudo` can only be used from a terminal session.
- The `secure_path` for `sudo` is restricted to a known list of directories, for example:

  ```text
  /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
  ```

This configuration reduces the attack surface and improves auditability of privileged actions.

### 6. System monitoring script

A Bash script named `monitoring.sh` periodically broadcasts key system information to all logged-in users using `wall`.

**Behavior:**

- Executed automatically at boot and then **every 10 minutes** via `cron`.
- No errors or unwanted output should appear in the broadcast.

**Reported metrics include:**

- System architecture and kernel version.
- Number of **physical CPUs**.
- Number of **virtual CPUs**.
- Current **RAM usage** (absolute and percentage).
- Current **disk usage** (absolute and percentage) of the main filesystem.
- Current **CPU load** (percentage).
- Date and time of the **last reboot**.
- Whether **LVM** is in use.
- Number of **active TCP connections**.
- Number of **logged-in users**.
- Server **IPv4 address** and **MAC address**.
- Number of commands executed using `sudo`.

This script demonstrates the use of shell scripting, system tools (`uname`, `lscpu`, `free`, `df`, `top`/`vmstat`, `who`, `last`, `ss`/`netstat`, `ip`/`ifconfig`, `journalctl`/log parsing), and **cron** for periodic tasks.

---

## ⭐ Bonus features

In addition to the mandatory configuration, the project includes the bonus part as implemented on the virtual machine. Bonus work focuses on making the server closer to a real-world setup:

- A more advanced **LVM partition layout**, inspired by the example in the subject (separate logical volumes for root, home, var, log, etc.).
- Deployment of extra **network services** on top of the hardened base system, such as:
  - A lightweight **web stack** (for example a WordPress instance backed by lighttpd, MariaDB and PHP).
  - At least one **additional service** chosen for its practical usefulness on a small server (e.g. a monitoring, file-sharing or utility service).
- Firewall rules extended accordingly to allow the extra service ports while keeping the rest of the system locked down.

> Note: The exact bonus services running on the VM are documented in the project report / configuration notes and are not all directly visible from this repository alone.

---

## 📁 Repository contents

Because the subject explicitly forbids uploading the virtual machine image, this repository only contains:

- `signature.txt` – SHA-1 signature of the VM disk image (used for evaluation).
- `monitoring.sh` – the Bash script used to broadcast system information.
- Optional configuration snippets, notes or helper scripts exported from the VM.

All actual configuration (partitioning, users, policies, services) lives on the virtual machine itself and is reproduced by following the project’s setup steps.

---

## 🔁 Reproducing the setup

To reproduce a similar environment:

1. Create a new virtual machine in VirtualBox / UTM with a minimal Linux ISO.
2. Partition the virtual disk using encrypted **LVM** with multiple logical volumes.
3. Install the base system **without a graphical environment**.
4. Enable and configure:
   - SSH on port **4242** (no root login).
   - **UFW** or **firewalld** with only required ports open.
   - **AppArmor** or **SELinux** in enforcing mode.
5. Create the required user and groups (`user42`, `sudo`) and configure the **password policy**.
6. Harden `sudo` according to the rules above.
7. Install and configure any **bonus services** you want (web stack, additional daemons).
8. Deploy the `monitoring.sh` script and schedule it with `cron` to run every 10 minutes.

---

## 🧠 What I learned

Working on Born2beRoot helped me build a strong foundation in:

- Virtualization with **VirtualBox / UTM**.
- Installing and maintaining a **minimal Linux server**.
- **Disk encryption** and **LVM** partitioning.
- Basic **security hardening** (firewall, SSH, MAC systems, sudo).
- **User and group management** and password policies.
- Writing non-trivial **Bash scripts** and scheduling them with **cron**.
- Deploying and managing additional services on a hardened base system.

This project serves as my first serious step into Linux system administration and server security, and it provides a base I can extend in future DevOps / infrastructure work.
