---
{"dg-publish":true,"permalink":"/ressurser/snippets/select-json-prop-in-sql/","dgHomeLink":true,"dgPassFrontmatter":false}
---

# Select json prop in sql

Tags: SQL

```sql
select SalesId, logdate
, JSON_VALUE([Data], '$.CreatedDate') as CDate
from #tmp
where SalesId = 123
and Data > ''
order by logdate desc
```