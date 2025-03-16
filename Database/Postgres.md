# PostgreSQL Database and Schema Management

## 1. Create a Database
To create a new PostgreSQL database, follow these steps:

```sh
su postgres
psql
```

Create the database:
```sql
CREATE DATABASE hsms_billing;
```

Verify the database creation:
```sql
\l
```

---
## 2. Create a Schema in the Database
Connect to the newly created database:
```sql
\c hsms_billing;
```

Create a new schema:
```sql
CREATE SCHEMA hsms_inventory;
```

Verify the schema creation:
```sql
SELECT schema_name FROM information_schema.schemata WHERE schema_name = 'hsms_inventory';
```

---
## 3. Modify Schema

### Rename a Schema
To rename an existing schema from `inventory` to `billing`, run:
```sql
ALTER SCHEMA inventory RENAME TO billing;
```

Verify the schema renaming:
```sql
SELECT schema_name FROM information_schema.schemata WHERE schema_name = 'billing';
```

### Drop a Schema
To delete the schema and all its contents:
```sql
DROP SCHEMA billing CASCADE;
```

Verify the deletion:
```sql
SELECT schema_name FROM information_schema.schemata WHERE schema_name = 'billing';
```

---
## 4. Modify the Database

### Change Database Owner
Change the owner of the `hsms_billing` database:
```sql
ALTER DATABASE hsms_billing OWNER TO new_user;
```

### Modify Database Encoding
Set the encoding to UTF-8:
```sql
ALTER DATABASE hsms_billing SET ENCODING TO 'UTF8';
```

### Modify Database Collation
Set collation to English (US):
```sql
ALTER DATABASE hsms_billing SET LC_COLLATE TO 'en_US.UTF-8';
```

---
## Conclusion
This guide provides essential commands to create, modify, and manage PostgreSQL databases and schemas effectively. Follow these steps carefully to structure your database efficiently.


