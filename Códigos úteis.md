# Declarando variáveis (MS - SQL)

```sql
DECLARE @ID_CLIENTE INT
BEGIN
SET @ID_CLIENTE  = (SELECT TOP 1 IDCLIENTE FROM CLIENTE ORDER BY NEWID())
END
```

# CASE (MySQL)

```sql
SELECT CASE             
            WHEN A + B > C AND B + C > A AND A + C > B THEN
                CASE 
                    WHEN A = B AND B = C THEN 'Equilateral'
                    WHEN A = B OR B = C OR A = C THEN 'Isosceles'
                    ELSE 'Scalene'
                END
            ELSE 'Not A Triangle'
        END
FROM TRIANGLES
```

# Subtração básica (MySQL)

```sql
SELECT COUNT(CITY) - COUNT(DISTINCT(CITY)) FROM STATION
```

# Linhas que começam ou terminam com vogal (MySQL)

Começando

```sql
SELECT DISTINCT(CITY) FROM STATION

WHERE CITY RLIKE '^[aeiouAEIOU]'

//SELECT DISTINCT(CITY) FROM STATION

//WHERE NOT CITY RLIKE '^[aeiouAEIOU]' -> NÃO COMEÇAM COM VOGAL
```

Terminando

```sql
SELECT DISTINCT(CITY) FROM STATION

WHERE CITY RLIKE '^[aeiouAEIOU]$'
```

Começando e terminando com vogal

```sql
SELECT DISTINCT(CITY) FROM STATION

WHERE CITY RLIKE '^[aeiouAEIOU].*[aeiouAEIOU]$'
```

Não começa nem termina

```sql
SELECT DISTINCT(CITY) from STATION
WHERE CITY NOT RLIKE '^[aeiouAEIOU]' AND CITY NOT RLIKE '[AEIOUaeiou]$'
```

# Selecionando o maior e o menor texto de uma coluna. Se tiver mais de um texto com o maior ou menor tamanho, pegar pela ordem alfabética

```sql
(SELECT CITY, LENGTH(CITY) FROM STATION
ORDER BY LENGTH(CITY) ASC, CITY
LIMIT 1) 

UNION 

(

SELECT CITY, LENGTH(CITY) FROM STATION
ORDER BY LENGTH(CITY) DESC, CITY
LIMIT 1
    )
```

# Ordenando por últimos caracteres

```sql
SELECT Name FROM STUDENTS
WHERE Marks > 75
ORDER BY RIGHT(Name, 3), ID
```

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
```

# Auto relacionamento (MS-SQL)

Retorna uma tabela que mostra os vendedores e os seus gerentes fazendo um INNER JOIN usando a chave do vendedor e a chave do superior na mesma tabela.

```sql
SELECT V.IDVENDEDOR, 
				V.NOME AS GERENTE,
				G.NOME AS VENDEDOR,
				G.ID_GERENTE,
FROM VENDEDOR V
INNER JOIN VENDEDOR G
ON V.IDVENDEDOR = G.ID_GERENTE
	
```

# Cursor, Loop e Variáveis + Procedure (MS-SQL)

```sql
CREATE PROCEDURE CAD_NOTAS AS 

DECLARE 

		C_NOTAS CURSOR FOR
		SELECT IDNOTA FROM NOTA_FISCAL
		WHERE IDNOTA NOT IN (SELECT ID_NOTA_FISCAL FROM ITEM_NOTA)

DECLARE

		@IDNOTA INT,
		@ID_PRODUTO INT
		@TOTAL DECIMAL(10,2)

OPEN C_NOTAS

...

WHILE @@FETCH_STATUS = 0
BEGIN
		SET @ID_PRODUTO = 0
		(SELECT TOP 1 IDPRODUTO FROM PRODUTO ORDER BY NEWID())

		SET @TOTAL = (SELECT VALOR FROM PRODUTO WHERE IDPRODUTO = @ID_PRODUTO)

		INSERT INTO ITEM_NOTA (ID_PRODUTO, ID_NOTA_FISCAL, QUANTIDADE, TOTAL)
		VALUES (@ID_PRODUTO, @IDNOTA, 1, @TOTAL)

FETCH NEXT FROM C_NOTAS
INTO @IDNOTA

END

CLOSE C_NOTAS
DEALLOCATE
GO
```

# Procedures (MS-SQL)

```sql
CREATE PROCEDURE CAD_NOTAS AS 
	// Código de Cursor, Loop e Variáveis
GO
EXEC CAD_NOTAS
GO
```

# Criando Constraints (MS-SQL)

```sql
ALTER TABLE NOTA_FISCAL ADD CONSTRAINT FK_NOTA_VENDEDOR
FOREIGN KEY(ID_VENDEDOR) REFERENCES VENDEDOR(IDVENDEDOR)

GO
```