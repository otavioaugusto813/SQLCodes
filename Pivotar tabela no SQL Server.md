#sql #SQL_Server  #bi 

```sql
select Firstname, Amount, PostalCode, LastName, AccountNumber
from
(
  select value, columnname
  from yourtable
) d
pivot
(
  max(value)
  for columnname in (Firstname, Amount, PostalCode, LastName, AccountNumber)
) piv;
```
