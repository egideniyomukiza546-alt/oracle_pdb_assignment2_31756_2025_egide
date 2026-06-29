# Oracle PDB Administration Assignment

## Assignment Overview

This assignment demonstrates practical administration of Oracle Multitenant Architecture using **Oracle 21c XE**. It covers the creation and configuration of permanent and temporary Pluggable Databases (PDBs), user management, troubleshooting connectivity issues, and monitoring via Oracle Enterprise Manager (EM) Express.

**Student:** Egide  
**Student ID:** 31756-2025  
**Course:** DPR400210 – Database Programming  
**Instructor:** Eric Maniraguha  
**Date:** June 29, 2026  

---

## Oracle Environment

| Component | Details |
|-----------|---------|
| **Oracle Version** | Oracle Database 21c Express Edition (Release 21.0.0.0.0) |
| **Operating System** | Microsoft Windows 10 (64-bit) |
| **Tools Used** | SQL*Plus, Oracle Enterprise Manager (EM Express) |
| **Container Database (CDB)** | `XE` |
| **EM Express Port** | 5500 (HTTPS) |

![Environment Details](Environment%20details.png)
![Current Environment](Current%20Environment%20of%20db.png)

---

## Task 1: Create and Configure Permanent PDB

### PDB Information
- **PDB Name:** `Eg_pdb_31756_2025`
- **Admin User:** `pdb_admin` 
- **Application User:** `Egide_plsqlaua_31756_2025`

### User Information
- **Username:** `Egide_plsqlaua_31756_2025`
- **Password:** `egide@2021`
- **Privileges Granted:** `CONNECT`, `RESOURCE`, `CREATE SESSION`, `CREATE TABLE`, `CREATE VIEW`, `CREATE PROCEDURE`, `CREATE SEQUENCE`, `DBA`

### Process Overview

1. **Connect as SYSDBA to the Root Container** and create the PDB.
2. **Open the PDB** and save its state.
3. **Switch to the new PDB** and create the application user.
4. **Grant necessary privileges** to the user.
5. **Test login** using the full connect descriptor (to bypass TNS issues).

### Screenshots

| Step | Screenshot |
|------|------------|
| PDB Creation Command | ![PDB Creation](pdb_creation.png) |
| Created PDB Information | ![Created PDB Info](created%20pdb%20information.png) |
| Verify and Open the PDB | ![Verify and Open](Verify%20and%20Open%20the%20PDB.png) |
| User Creation | ![User Creation](user_creation.png) |
| Privileges Granted | ![Privileges](Show%20the%20privilege%20listings..png) |
| Successful Login Test | ![User Login](user_login.png) |

---

## Task 2: Manage Temporary PDB (Create and Delete)

This task involved creating a temporary PDB named `Eg_to_delete_pdb_31756_2025` and then dropping it.

### Process Overview

1. **Create the Temporary PDB** from the Root Container (`CDB$ROOT`).
2. **Verify** the PDB exists.
3. **Open** the PDB to confirm functionality.
4. **Drop (Delete)** the PDB permanently using `INCLUDING DATAFILES`.
5. **Confirm** the PDB is removed.

### Screenshots

| Step | Screenshot |
|------|------------|
| Temporary PDB Creation | ![Temp Create](temporary_pdb_creation.png) |
| Verify Existence | ![Verify Temp](Verify%20Existence%20Temporary%20PDB.png) |
| Open the Temporary PDB | ![Open Temp](Open%20the%20Temporary%20PDB.png) |
| Confirm Deletion (Drop) | ![Drop Temp](Confirm%20Deletion.png) |

---

## Task 3: Oracle Enterprise Manager (EM) Express

### Access Information
- **URL:** `https://localhost:5500/em`
- **Username:** `system`
- **Password:** (Password set during Oracle XE installation)
- **Container:** `Eg_pdb_31756_2025`

### Process Overview

1. **Access EM Express** via the browser (bypass the self-signed certificate warning).
2. **Log in** with the `system` user.
3. **Select** the `Eg_pdb_31756_2025` container.
4. Explore the dashboard to monitor performance, storage, and security.

### Screenshots

| Step | Screenshot |
|------|------------|
| EM Express Dashboard | ![OEM Dashboard](oem_dashboard.png) |
| Instance Details | ![Instance Details](nstance%20details.png) |

---

## Challenges Encountered & Resolutions

### 1. TNS Connectivity Issue (ORA-12154)
- **Problem:** `sqlplus` failed with `ORA-12154` while `tnsping` worked.
- **Root Cause:** Two different Oracle homes existed (`OraDB21Home1` vs `dbhomeXE`). `sqlplus` was looking at the wrong `tnsnames.ora`.
- **Resolution:** Used the **Full Connect Descriptor** to bypass TNS entirely:
  ```sql
  CONNECT Egide_plsqlaua_31756_2025/"egide@2021"@"(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=localhost)(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=eg_pdb_31756_2025)))";
