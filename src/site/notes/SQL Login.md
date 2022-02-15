# SQL Login

Created: Mar 17, 2021 11:21 AM
Tags: SQL

**I kontekst av server**

Legg bruker til på server

```sql
CREATE LOGIN username with PASSWORD='abc'
```

**I kontekst av database**

Lag bruker til database

```sql
CREATE USER username FOR LOGIN username;
```

Gi permissions til database

```sql
ALTER ROLE db_owner ADD MEMBER username;
```