# MariaDB on RHEL 9 Lab

## Overview

This project demonstrates the installation, configuration, security hardening, and data directory migration of MariaDB on Red Hat Enterprise Linux 9.

## Prerequisites

- RHEL 9 server/VM
- Sudo privileges
- Internet connectivity
- Updated operating system

## Steps Performed

### Step 1: Update the System
```bash
sudo dnf update -y
sudo reboot
```

### Step 2: Configure Hostname
```bash
sudo hostnamectl set-hostname enos-mariadb1
hostnamectl
```

### Step 3: Install MariaDB Server
```bash
sudo dnf install mariadb-server -y
```

### Step 4: Start and Enable MariaDB
```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo systemctl status mariadb
```

### Step 5: Secure MariaDB Installation
```bash
sudo mysql_secure_installation
```

### Step 6: Access MariaDB
```bash
sudo mysql -u root -p
```

### Step 7: Create Database and User
```sql
CREATE DATABASE studentdb;

CREATE USER 'studentuser'@'localhost'
IDENTIFIED BY 'StrongPassword123';

GRANT ALL PRIVILEGES ON studentdb.* TO 'studentuser'@'localhost';

FLUSH PRIVILEGES;

SHOW DATABASES;
```

### Step 8: Create a New Data Directory
```bash
sudo mkdir -p /data/mariadb
sudo chown -R mysql:mysql /data/mariadb
```

### Step 9: Migrate MariaDB Data
```bash
sudo systemctl stop mariadb

sudo rsync -av /var/lib/mysql/ /data/mariadb/

sudo vi /etc/my.cnf.d/mariadb-server.cnf
```
Update:
```ini
[mysqld]
datadir=/data/mariadb
socket=/data/mariadb/mysql.sock
```

Restart the service:
```bash
sudo systemctl start mariadb
```

### Step 10: Verify Migration
```bash
sudo systemctl status mariadb

sudo mysql -u root -p
```

Verify the active data directory:
```sql
SHOW VARIABLES LIKE 'datadir';

## Troubleshooting

| Issue | Resolution |
| --- | --- |
| MariaDB service fails to start | Verify the configuration file and review logs using systemctl status mariadb and journalctl \-uxe.<BR>check if SELinux is blocking the service or not. if yes, use semanage to applay contex |
| Access denied for database users | Confirm user privileges and credentials. |
| Cannot connect to MariaDB | Ensure the service is running and firewall rules allow access. |
| Database unavailable after migration | Verify the new data directory path, ownership, and permissions. |
| Permission denied errors | Ensure /data/mariadb is owned by the mysql user and group. |

## Deliverables

- Running MariaDB server on RHEL 8
- Secured database environment
- Created database and user
- Custom MariaDB data directory configured
- Successful data migration and verification

## Skills Demonstrated

- Linux Administration
- MariaDB Database Management
- Service Management (systemd)
- Security Hardening
- Storage & Data Migration


