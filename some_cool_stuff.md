## Take FULL backup of entire DB
`pg_dump -U postgres -d bank_db > bank_db_backup.sql`

- This creates a single .sql file containing the whole database.

## ✅ Export with schema + data + everything
`pg_dump -U postgres -d bank_db --format=custom -f bank_db.backup`

- This is the best format for real backups.

## ✅ Restore later (SQL file)
- If you exported like:
`pg_dump -U postgres -d bank_db > bank_db_backup.sql`

- Restore:
`psql -U postgres -d bank_db < bank_db_backup.sql`

## ✅ Restore later (custom backup file)

- If you exported like:
`pg_dump -U postgres -d bank_db --format=custom -f bank_db.backup`

- Restore:
`pg_restore -U postgres -d bank_db bank_db.backup`

## ✅ Export ALL databases (full server backup)

`pg_dumpall -U postgres > all_databases.sql`

- This exports:

1. all databases
2. all roles/users
3. all permissions

## ⭐ Pro Tip (most common “perfect” command)
`pg_dump -U postgres -F c -b -v -f bank_db.backup bank_db`

- Meaning:

1. `-F c → custom format`
2. `-b → include blobs (rare but safe)`
3. `-v → verbose output`




