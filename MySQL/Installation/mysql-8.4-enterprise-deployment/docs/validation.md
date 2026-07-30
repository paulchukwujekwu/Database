# MySQL 8.4 Enterprise Post-Installation Validation Guide

## Overview

This document outlines the validation and verification procedures performed after the successful deployment and hardening of MySQL Enterprise Server 8.4 on Red Hat Enterprise Linux 9.

The purpose of these checks is to verify that:

- MySQL is running correctly
- The configured directories are being used
- Logging is functioning
- Binary logging is enabled
- Administrative access is operational
- The deployment is ready for application onboarding

---

# Validation Objectives

The validation process confirms:

✅ Database service availability

✅ Correct software version

✅ Data directory configuration

✅ Binary log configuration

✅ Database connectivity

✅ Service startup verification

✅ Operational readiness

---

# Verify MySQL Version

Confirm the installed MySQL version.

```bash
mysql --version
```

Expected output:

```text
mysql Ver 8.4.4-commercial for Linux on x86_64
```

Validation Result:

```text
PASS
```

---

# Verify Service Status

Validate that the database service is running.

```bash
systemctl status mysql
```

Expected output:

```text
Active: active (running)
```

Validation checklist:

- Service running
- No startup errors
- No crashed processes
- MySQL PID present

Validation Result:

```text
PASS
```

---

# Verify MySQL Connectivity

Connect to the database.

```bash
mysql -u root -p
```

Expected:

```text
Welcome to the MySQL monitor
```

Validation Result:

```text
PASS
```

---

# Verify System Databases

Confirm required system databases exist.

```sql
SHOW DATABASES;
```

Expected output:

```text
information_schema
mysql
performance_schema
sys
```

Validation Result:

```text
PASS
```

---

# Verify Installed Version from Database

Validate the running server version.

```sql
SHOW VARIABLES LIKE 'version';
```

Expected output:

```text
8.4.4-commercial
```

Validation Result:

```text
PASS
```

---

# Verify Data Directory

Confirm MySQL is using the configured data directory.

```sql
SHOW VARIABLES LIKE 'datadir';
```

Expected output:

```text
/mysql/mysqldata/
```

Validation Result:

```text
PASS
```

---

# Verify Binary Logging

Confirm binary logging is enabled.

```sql
SHOW BINARY LOGS;
```

Expected output:

```text
mysql-bin.000001
mysql-bin.000002
```

Validation Result:

```text
PASS
```

---

# Verify Log File Creation

Confirm required logs exist.

Expected files:

```text
/mysql/log/error.log
/mysql/log/slow.log
```

Verification:

```bash
ls -ltr /mysql/log
```

Validation Result:

```text
PASS
```

---

# Verify Runtime Directory

Confirm socket runtime directory exists.

```bash
ls -ld /run/mysql
```

Expected output:

```text
drwxr-xr-x mysql mysql
```

Validation Result:

```text
PASS
```

---

# Verify Runtime Directory Persistence

Validate the tmpfiles configuration.

Check file:

```bash
cat /etc/tmpfiles.d/mysql.conf
```

Expected:

```text
d /run/mysql 0755 mysql mysql -
```

Validation Result:

```text
PASS
```

---

# Verify Startup Configuration

Reload daemon configuration.

```bash
systemctl daemon-reload
```

Enable automatic startup.

```bash
systemctl enable mysql
```

Verification:

```bash
systemctl is-enabled mysql
```

Expected output:

```text
enabled
```

Validation Result:

```text
PASS
```

---

# Verify Service Restart

Test restart operation.

```bash
systemctl restart mysql
```

Verify service state:

```bash
systemctl status mysql
```

Expected:

```text
active (running)
```

Validation Result:

```text
PASS
```

---

# Verify Storage Availability

Check database filesystem utilization.

```bash
df -h
```

Validation Criteria:

- Adequate free space available
- Data filesystem mounted
- No storage-related errors

Validation Result:

```text
PASS
```

---

# Operational Readiness Checklist

| Check | Status |
|---------|---------|
| MySQL Installed | ✅ |
| MySQL Running | ✅ |
| Root Access Verified | ✅ |
| Data Directory Verified | ✅ |
| Binary Logging Enabled | ✅ |
| Error Logging Enabled | ✅ |
| Runtime Directory Configured | ✅ |
| Runtime Persistence Configured | ✅ |
| Service Auto-Start Enabled | ✅ |
| Security Hardening Completed | ✅ |

---

# Validation Outcome

The MySQL Enterprise Server 8.4 deployment successfully passed all installation, security, and operational readiness checks.

The environment was verified to be:

- Stable
- Secure
- Recoverable
- Operationally ready
- Prepared for application onboarding

## Deployment Status

```text
SUCCESSFULLY VALIDATED
```
