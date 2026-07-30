# MySQL 8.4 Enterprise Server Installation Guide

## Project Overview

This project demonstrates the deployment and configuration of MySQL Enterprise Server 8.4 on Red Hat Enterprise Linux 9 using enterprise deployment standards and operational best practices.

---

# Environment Overview

## Operating System

```text
Red Hat Enterprise Linux 9
```

## Database Version

```text
MySQL Enterprise Server 8.4.4
```

## Installation Method

```text
Binary Installation
```

---

# Storage Validation

Before installation, verify available filesystem capacity.

```bash
df -h
```

Example:

```text
Filesystem              Size   Used   Avail
/mysql                  1TB    7GB    993GB
```

Ensure adequate space is available for:

- Database files
- Binary logs
- Redo logs
- Temporary files
- Future growth

---

# Operating System Optimization

## Disable Transparent Huge Pages

Transparent Huge Pages can negatively affect database performance.

```bash
echo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled

echo never | sudo tee /sys/kernel/mm/transparent_hugepage/defrag
```

To ensure persistence after reboot:

```bash
chmod +x /etc/rc.d/rc.local
```

---

# Create MySQL Operating System Account

Create a dedicated service account.

```bash
groupadd -r mysql

useradd -r -g mysql -s /sbin/nologin mysql
```

Benefits:

- Service isolation
- Least privilege access
- Improved security

---

# Create Directory Structure

## Directories

```bash
mkdir -p /mysql/mysqldata
mkdir -p /mysql/mysqlbin
mkdir -p /mysql/mysql
mkdir -p /mysql/log
```

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

---

# Create Runtime Directory

```bash
mkdir -p /run/mysql

chown mysql:mysql /run/mysql

chmod 755 /run/mysql
```

---

# MySQL Binary Installation

## Copy Installation Package

```bash
cp mysql-commercial-8.4.4-linux-glibc2.28-x86_64.tar.xz /mysql/mysql
```

Set ownership:

```bash
chown mysql:mysql mysql-commercial-8.4.4-linux-glibc2.28-x86_64.tar.xz
```

Extract software:

```bash
tar -xvf mysql-commercial-8.4.4-linux-glibc2.28-x86_64.tar.xz
```

Rename installation directory:

```bash
mv mysql-commercial-8.4.4-linux-glibc2.28-x86_64 mysql-8.4.4
```

---

# Configure Standard Symlinks

```bash
ln -s /mysql/mysql/mysql-8.4.4 /mysql/mysql/mysql

ln -s /mysql/mysql/mysql-8.4.4 /usr/local/mysql
```

Benefits:

- Simplifies upgrades
- Provides consistent paths
- Reduces maintenance effort

---

# Create MySQL Configuration

Configuration file:

```bash
/etc/my.cnf
```

Important configuration areas include:

- Base Directory
- Data Directory
- Logging
- Binary Logging
- Character Sets
- Security Configuration
- InnoDB Configuration
- Connection Management

---

# Validate Configuration

Before initialization validate the configuration.

```bash
mysqld \
--defaults-file=/etc/my.cnf \
--validate-config
```

---

# Initialize Database

Initialize the data directory.

```bash
sudo -u mysql \
mysqld \
--defaults-file=/etc/my.cnf \
--initialize
```

Retrieve temporary password.

```bash
grep "temporary password" /mysql/log/error.log
```

Example:

```text
<Temporary_Password>
```

---

# Configure Systemd Service

Create:

```bash
/etc/systemd/system/mysql.service
```

Reload systemd:

```bash
systemctl daemon-reload
```

Enable startup:

```bash
systemctl enable mysql
```

Start service:

```bash
systemctl start mysql
```

Verify:

```bash
systemctl status mysql
```

---

# Configure PATH

System-wide configuration.

```bash
echo 'export PATH=/mysql/mysql/mysql/bin:$PATH' \
> /etc/profile.d/mysql.sh
```

Reload profile.

```bash
source /etc/profile
```

---

# Installation Complete

At this stage:

- MySQL software is installed
- Data directory is initialized
- Systemd service is configured
- Database service is running
- Ready for hardening procedures

Continue with:

```text
docs/hardening.md
```
