É necesário particionar o dado pelo campo que se quer que o acumulado seja executado. É importante ordenar da maneira certa também para acumular na ordem correta. 




```sql 

SELECT [Nº principal do imobilizado],
       [Exercício],
       [Período contábil da depreciação],
       [Depreciação normal lançada do ano],
       CAST(SUM([Depreciação normal lançada do ano]) OVER (PARTITION BY [Nº principal do imobilizado]
                                                           ORDER BY [Exercício], [Período contábil da depreciação] ROWS UNBOUNDED PRECEDING) AS DECIMAL(8, 2)) AS Acumulado
FROM [operacional].[vw_saphanadb_ValoresImobilizadoPeríodo]

```