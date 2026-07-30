# Deployment Validation Screenshots

This directory contains sanitized screenshots collected during the deployment, hardening, and validation of MySQL Enterprise Server 8.4.

> **Note:** All screenshots have been sanitized to remove environment-specific information such as hostnames, IP addresses, usernames, passwords, and organization identifiers.

---

## MySQL Version Validation

**Purpose:** Verifies the successful installation of MySQL Enterprise Server 8.4.

### Screenshot

<a href="mysql-version.png">
 <img src="mysql-version.png" alt="MySQL Version Validation" width="1000">
</a>

*Figure 1: Verification of the successful installation of MySQL Enterprise Server 8.4.4.*
---

## Service Status Validation

**Purpose:** Confirms that the MySQL service is active, running, and managed by systemd.

### Screenshot

<a href="systemctl-status.png">
  <img src="systemctl-status.png" alt="MySQL Service Status" width="1000">
</a>

*Figure 2: Validation of MySQL Enterprise Server service startup and operational status using systemd.*
---

## Data Directory Validation

**Purpose:** Confirms that MySQL is using the configured data directory.

### Screenshot

<a href="datadir.png">
  <img src="datadir.png" alt="MySQL Data Directory Validation" width="1000">
</a>

*Figure 3: Verification that MySQL is using the configured enterprise data directory located at /mysql/mysqldata.*
---

## Binary Log Validation

**Purpose:** Verifies binary logging is enabled for recovery and replication readiness.

### Screenshot

<a href="binary-logs.png">
  <img src="binary-logs.png" alt="MySQL Binary Log Validation" width="1000">
</a>

*Figure 4: Verification that binary logging is enabled to support replication and Point-in-Time Recovery (PITR).*
---

## Database Initialization Validation

**Purpose:** Confirms successful initialization of MySQL system databases.

### Screenshot

<a href="show-databases.png">
  <img src="show-databases.png" alt="MySQL System Databases Validation" width="1000">
</a>
<p align="center">
<i>Figure 5: Validation of MySQL system databases following successful initialization of the MySQL Enterprise Server instance.</i>
</p>
---

## Validation Summary

The deployment was successfully validated with the following checks:

- ✅ MySQL Enterprise Server 8.4 Installed
- ✅ Database Service Running
- ✅ Database Connectivity Verified
- ✅ Data Directory Configured
- ✅ Binary Logging Enabled
- ✅ System Databases Initialized
- ✅ Security Hardening Completed
- ✅ Environment Ready for Application Onboarding
