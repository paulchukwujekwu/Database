# MySQL 8.4 Enterprise Deployment Architecture

## Overview

This architecture illustrates the deployment of MySQL Enterprise Server 8.4 on Red Hat Enterprise Linux 9 using a dedicated storage layout, custom configuration, enterprise logging standards, and systemd service management.

## Architecture Diagram

<a href="mysql-enterprise-architecture.png">
  <img src="mysql-enterprise-architecture.png" alt="MySQL Enterprise Deployment Architecture" width="1000">
</a>

<p align="center">
<sub><i>Figure 1: MySQL Enterprise Server 8.4 deployment architecture on Red Hat Enterprise Linux 9.</i></sub>
</p>

---

## Components

### Application Layer

Client applications connect to MySQL using TCP/IP over port 3306.

### MySQL Enterprise Server

Provides:

- Query processing
- Transaction management
- Storage engine services
- Authentication and authorization

### Data Directory

```text
/mysql/mysqldata
```

Stores:

- System databases
- User databases
- InnoDB tablespaces
- Internal metadata

### MySQL Binary Files

```text
/mysql
```

Stores:

- MySQL Enterprise software binaries
- MySQL server executable (mysqld)
- MySQL client utilities
- Administrative tools
- Supporting libraries

Example:

```text
/mysql/mysql/mysql/bin/mysqld
/mysql/mysql/mysql/bin/mysql
/mysql/mysql/mysql/bin/mysqladmin
```

Purpose:

- Provides the MySQL database software installation location.
- Separates application software binaries from database files.
- Simplifies future upgrades and patch management.
- Supports enterprise filesystem layout and deployment standards.

### Binary Log Directory

```text
/mysql/mysqlbin
```

Stores:

- Binary logs
- Replication records
- Recovery information
- Change history

### Log Directory

```text
/mysql/log
```

Stores:

- Error logs
- Slow query logs
- Diagnostic information

### Systemd Service

```text
mysql.service
```

Provides:

- Automatic startup
- Service monitoring
- Failure recovery
- Controlled shutdown

### Runtime Directory

```text
/run/mysql
```

Stores:

- MySQL socket file
- Process ID (PID) file

### Operating System

```text
Red Hat Enterprise Linux 9
```

Provides:

- Filesystem management
- Process scheduling
- Memory management
- Security controls

---

## Design Highlights

```markdown
✅ Dedicated MySQL software installation directory

✅ Dedicated data filesystem

✅ Dedicated binary log location

✅ Dedicated log directory

✅ Runtime directory persistence

✅ Systemd integration

✅ Enterprise security hardening

✅ Recovery-ready logging configuration

✅ Production-oriented deployment standards
