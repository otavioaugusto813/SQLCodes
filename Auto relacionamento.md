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