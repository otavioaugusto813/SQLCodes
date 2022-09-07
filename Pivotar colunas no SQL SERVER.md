SELECT * FROM (
  SELECT
    [Nº do documento de compras]
	  ,[Nº item ao qual se aplicam as condições]
      ,[Taxa montante ou porcentagem]
      ,[Denominação]
   
  FROM [operacional].[vw_saphanadb_ElementosPreço]
) Teste

PIVOT (
  SUM([Taxa montante ou porcentagem])
  FOR [Denominação]
  IN (
    [Preço bruto],
    [Frete absoluto],
    [Suprimento APC]
  )
) AS PivotTable