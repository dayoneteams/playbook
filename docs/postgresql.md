# PostgreSQL

## Create new database and user account
Access to PostgreSQL console (via `psql` for example). And type the follow commands.

```sql
# Create new database
CREATE DATABASE my_db;

# Create new user
CREATE USER backend_app WITH ENCRYPTED PASSWORD 'enter-strong-password-here';

# Assign user to have all permissions over the database
GRANT ALL PRIVILEGES ON DATABASE backend_app TO my_db;
GRANT USAGE, CREATE ON SCHEMA PUBLIC TO my_db;
```
