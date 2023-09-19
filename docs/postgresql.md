# PostgreSQL

## Access database from host server
SSH to host server. Then use `sudo -u postgres psql` command to access the database console.

## Create new database and user account
Access to PostgreSQL console (via `psql` for example). And type the follow commands.

```sql
# Create new database
CREATE DATABASE my_db;

# Create new user
CREATE USER backend_app WITH ENCRYPTED PASSWORD 'enter-strong-password-here';

# Assign user to have all permissions over the database
GRANT ALL PRIVILEGES ON DATABASE my_db TO backend_app;
```

## Allow access from same machine (localhost)
Modify `/etc/postgresql/{version}/main/postgresql.conf` file.

Change two following lines:
`local   all             all                        peer`
`host    all             all         ::1/128        ident`

To be:
`local   all             all                        trust`
`host    all             all         ::1/128        trust`

Restart Postgres
```bash
sudo service postgresql restart
```
