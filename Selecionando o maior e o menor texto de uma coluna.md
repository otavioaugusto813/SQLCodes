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
