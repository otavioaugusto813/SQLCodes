# Printar padrão (MS-SQL)

Aparentemente, só é possível realizar loops dentro de procedures no MySql.

```sql
declare @a INT
SET @a = 20
WHILE (@a>0)
    Begin
        Print replicate("* ", @a)
        Set @a = @a-1
    END;
