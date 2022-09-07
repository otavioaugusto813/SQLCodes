[[ReadItLater]] [[Article]]

# [Efficiently convert rows to columns in sql server](https://stackoverflow.com/questions/15745042/efficiently-convert-rows-to-columns-in-sql-server)

There are several ways that you can transform data from multiple rows into columns.

## Using `PIVOT`

In SQL Server you can use the `PIVOT` function to transform the data from rows to columns:

``` sql
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

See [Demo](https://data.stackexchange.com/stackoverflow/query/497432).

### Pivot with unknown number of `columnnames`

If you have an unknown number of `columnnames` that you want to transpose, then you can use dynamic SQL:

``` sql
DECLARE @cols AS NVARCHAR(MAX),
    @query  AS NVARCHAR(MAX)

select @cols = STUFF((SELECT ',' + QUOTENAME(ColumnName) 
                    from yourtable
                    group by ColumnName, id
                    order by id
            FOR XML PATH(''), TYPE
            ).value('.', 'NVARCHAR(MAX)') 
        ,1,1,'')

set @query = N'SELECT ' + @cols + N' from 
             (
                select value, ColumnName
                from yourtable
            ) x
            pivot 
            (
                max(value)
                for ColumnName in (' + @cols + N')
            ) p '
 
```

See [Demo](https://data.stackexchange.com/stackoverflow/query/497433).

## Using an aggregate function

If you do not want to use the `PIVOT` function, then you can use an aggregate function with a `CASE` expression:

``` sql
select 
  max(case when columnname = 'FirstName' then value end) Firstname,
  max(case when columnname = 'Amount' then value end) Amount,
  max(case when columnname = 'PostalCode' then value end) PostalCode,
  max(case when columnname = 'LastName' then value end) LastName,
  max(case when columnname = 'AccountNumber' then value end) AccountNumber
from yourtable
```

See [Demo](https://data.stackexchange.com/stackoverflow/query/497434).

## Using multiple joins

This could also be completed using multiple joins, but you will need some column to associate each of the rows which you do not have in your sample data. But the basic syntax would be:

``` sql
select fn.value as FirstName,
  a.value as Amount,
  pc.value as PostalCode,
  ln.value as LastName,
  an.value as AccountNumber
from yourtable fn
left join yourtable a
  on fn.somecol = a.somecol
  and a.columnname = 'Amount'
left join yourtable pc
  on fn.somecol = pc.somecol
  and pc.columnname = 'PostalCode'
left join yourtable ln
  on fn.somecol = ln.somecol
  and ln.columnname = 'LastName'
left join yourtable an
  on fn.somecol = an.somecol
  and an.columnname = 'AccountNumber'
where fn.columnname = 'Firstname'
```