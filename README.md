# Oracle Pluggable Databases Management 

## Overview of Tasks

This project focused on administering an Oracle Multitenant environment using Oracle Database 21c.  
The main tasks included:

- Creating a Pluggable Database (PDB) named GL_PDB_29118  
- Creating and managing a PDB for deletion named gl_to_delete_pdb_28945  
- Creating and managing database users  
- Configuring and accessing Oracle Enterprise Manager (OEM Express)  
- Assigning HTTPS port 5068 for OEM access   

---

## Oracle Environment Used

- Oracle Database 21c (Multitenant Architecture)
- Container Database (CDB)
- Pluggable Databases (PDBs)
- Oracle Enterprise Manager (OEM Express)
- HTTPS Port: 5068
- SQL Developer

---

## Task 1

### 1. Creating the Main PDB (GL_PDB_29118)

```sql
CREATE PLUGGABLE DATABASE GL_PDB_29118
ADMIN USER pdbadmin IDENTIFIED BY Oracle123
FILE_NAME_CONVERT = ('pdbseed','GL_PDB_29118');
```

<img width="1365" height="768" alt="PDB_creation" src="https://github.com/user-attachments/assets/1c1a2f8f-c6bb-4555-a3ac-e2635c7d64f5" />

```sql
ALTER PLUGGABLE DATABASE GL_PDB_29118 OPEN;
```
Verification:

```sql
SELECT name FROM v$pdbs;
```

<img width="1351" height="761" alt="PDB_opened" src="https://github.com/user-attachments/assets/2bd7ed7e-6d33-456b-8a06-3e94aaf8b37d" />

---

### 2. User Creation and Password Management

Creating a user:

```sql
CREATE USER gloria_plsqlauca_29118 IDENTIFIED BY 1234;
GRANT CONNECT, RESOURCE gloria_plsqlauca_29118;
```

<img width="1366" height="753" alt="Username_creation" src="https://github.com/user-attachments/assets/1ca66bff-15ea-48c4-a487-c74db0442bd6" />

---
## Task 2
### 1. Creating a PDB for Deletion (gl_to_delete_pdb_28945)

```sql
CREATE PLUGGABLE DATABASE gl_to_delete_pdb_29118
ADMIN USER pdbadmin IDENTIFIED BY Oracle123
FILE_NAME_CONVERT = ('pdbseed','gl_to_delete_pdb_29118');

ALTER PLUGGABLE DATABASE gl_to_delete_pdb_29118 OPEN;
```

<img width="1366" height="760" alt="temp_pdb created" src="https://github.com/user-attachments/assets/646054de-1d69-4b83-8e36-92d5ee8b8999" />

### 2. Dropping the PDB permanently:

```sql
DROP PLUGGABLE DATABASE gl_to_delete_pdb_29118 INCLUDING DATAFILES;
```

<img width="1366" height="727" alt="temp_pdb_dropped" src="https://github.com/user-attachments/assets/cba9f9ef-949d-43b0-ac53-60450263dcf7" />

---

### 3. Checking its non-existence

`SELECT name FROM v$pdbs;` shows that the PDB has been deleted successfully.
---

### 4. Checking Container and PDBs

Checking current container:

```sql
SHOW CON_NAME;
```

Switching to a PDB:

```sql
ALTER SESSION SET CONTAINER = GL_PDB_29118;
```

---

## TASK 3

### 1. Configuring Oracle Enterprise Manager (OEM Express)

- Setting HTTPS port to 5060:

```sql
EXEC DBMS_XDB_CONFIG.SETHTTPSPORT(5068);
```

- Verifying port:

```sql
SELECT DBMS_XDB_CONFIG.GETHTTPSPORT FROM dual;
```

This retrieved the assigned port in previous command/query.

- Accessing OEM:

`https://localhost:5068/em` retrieved the OEM webpage.

<img width="1350" height="648" alt="OEM dashboard" src="https://github.com/user-attachments/assets/87f2db00-b450-48d9-85c8-2c077e7d0313" />

<img width="1320" height="643" alt="OEM dashboard 2" src="https://github.com/user-attachments/assets/d0ae29d2-4c29-4890-b41b-ce7e182cc730" />

---

## Challenges Faced and Solutions

1. Invalid container (CDB$ROOT) during login  
   → Used SHOW CON_NAME to verify current container and switched using ALTER SESSION SET CONTAINER.

2. ORA-00942: view or table does not exist  
   → Ensured connection as SYSDBA before querying dynamic views such as v$pdbs.

3. OEM login popup repeatedly appearing  
   → Confirmed correct username, password, and container selection before logging in.

---
### References

## References

1. Oracle Corporation : *Oracle Database 19c Documentation Library*.  
   `https://docs.oracle.com/en/database/oracle/oracle-database/21/`

2. Youtube :
*What is Oracle Enterprise Manager| OEM Architecture| OEM Monitoring Tool*. `https://youtu.be/U9Qo5Du79oA?si=CanjLVnIdpFOm782`

---

## Integrity Statement

I hereby declare that this work is my own and that all configurations, commands, and outputs were performed and verified by me in my Oracle database environment. Any references used were properly acknowledged.
