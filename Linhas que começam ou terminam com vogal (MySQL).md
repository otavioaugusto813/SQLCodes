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