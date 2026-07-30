# MySQL 8.4 Enterprise Security Hardening Guide

## Overview

This document outlines the security hardening procedures implemented following the installation of MySQL Enterprise Server 8.4 on Red Hat Enterprise Linux 9.

The objective was to reduce the attack surface, enforce security best practices, strengthen authentication controls, and prepare the environment for enterprise application deployment.

---

# Security Objectives

The hardening activities focused on:

- Operating system level security
- MySQL service account isolation
- Secure file permissions
- Root account protection
- Password policy enforcement
- Removal of insecure default settings
- Restriction of privileged access
- Secure runtime directory configuration

---

# Operating System Hardening

## Disable Transparent Huge Pages

Transparent Huge Pages (THP) were disabled to improve database performance predictability and reduce latency.

```bash
echo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled

echo never | sudo tee /sys/kernel/mm/transparent_hugepage/defrag
```

Persistence was configured to ensure the settings survive server reboots.

---

# Dedicated MySQL Service Account

A dedicated operating system account was created for database operations.

```bash
groupadd -r mysql

useradd -r -g mysql -s /sbin/nologin mysql
```

Security Benefits:

- Service isolation
- Least privilege principle
- Reduced attack surface
- Prevention of interactive logins

---

# Secure Directory Permissions

Database directories were created using controlled ownership and permissions.

## Ownership

```bash
chown -R mysql:mysql /mysql
```

## Permissions

```bash
chmod 750 /mysql/mysqldata

chmod 750 /mysql/mysqlbin

chmod 755 /mysql/mysql

chmod 754 /mysql/log
```

Security Benefits:

- Prevent unauthorized file access
- Restrict modification of database files
- Protect binary logs
- Protect configuration files

---

# MySQL Configuration Hardening

The following security parameters were configured in the MySQL server configuration.

## Disable LOCAL INFILE

```ini
local_infile = OFF
```

Purpose:

Prevents unauthorized local file imports that could expose sensitive information.

---

## Disable Symbolic Links

```ini
symbolic-links = 0
```

Purpose:

Prevents symbolic link abuse and strengthens filesystem security.

---

## Character Set Standardization

```ini
character-set-server = utf8mb4

collation-server = utf8mb4_0900_ai_ci
```

Purpose:

Ensures consistent character handling across applications.

---

# Root Password Security

Following database initialization, the automatically generated temporary password was replaced.

Example:

```sql
ALTER USER 'root'@'localhost'
IDENTIFIED BY '<Strong_Root_Password>';
```

Apply changes:

```sql
FLUSH PRIVILEGES;
```

Security Benefits:

- Eliminates temporary password exposure
- Establishes strong administrative credentials
- Complies with enterprise password standards

---

# MySQL Secure Installation

The built-in security utility was executed.

```bash
mysql_secure_installation
```

The following actions were performed.

---

## Enable Password Validation

Password validation component enabled.

Selected Policy:

```text
MEDIUM
```

Requirements include:

- Minimum length
- Mixed case
- Numeric characters
- Special characters

---

## Remove Anonymous Users

Action:

```text
YES
```

Purpose:

Removes default accounts that allow access without authentication.

---

## Disable Remote Root Access

Action:

```text
YES
```

Purpose:

Restricts root account access to the local server only.

Security Benefit:

Reduces risk of brute-force and remote administrative attacks.

---

## Remove Test Database

Action:

```text
YES
```

Purpose:

Removes unnecessary databases that are not required in enterprise environments.

---

## Reload Privilege Tables

Action:

```text
YES
```

Purpose:

Ensures security changes become effective immediately.

---

# Runtime Directory Protection

A dedicated runtime directory was configured.

```bash
mkdir -p /run/mysql

chown mysql:mysql /run/mysql

chmod 755 /run/mysql
```

---

# Persistent Runtime Directory Creation

Because the /run filesystem is recreated after reboot, systemd tmpfiles was configured.

File:

```text
/etc/tmpfiles.d/mysql.conf
```

Configuration:

```text
d /run/mysql 0755 mysql mysql -
```

Apply configuration:

```bash
systemd-tmpfiles --create
```

Benefits:

- Automatic runtime directory creation
- Improved service reliability
- Compliance with modern RHEL practices

---

# Logging and Audit Readiness

The following logging controls were enabled.

## Error Logging

```ini
log-error = /mysql/log/error.log
```

## Slow Query Logging

```ini
slow_query_log = ON
```

## Binary Logging

```ini
log_bin = /mysql/mysqlbin/mysql-bin
```

Benefits:

- Operational troubleshooting
- Security investigations
- Audit readiness
- Point-in-time recovery support

---

# Security Outcome

The MySQL Enterprise Server deployment was successfully hardened in accordance with enterprise database administration best practices.

Implemented controls included:

✅ Dedicated service account

✅ Secure filesystem permissions

✅ Strong password policy

✅ Password validation

✅ Removal of anonymous accounts

✅ Restricted root access

✅ Disabled LOCAL INFILE

✅ Disabled symbolic links

✅ Persistent runtime directory management

✅ Error logging

✅ Slow query logging

✅ Binary logging
