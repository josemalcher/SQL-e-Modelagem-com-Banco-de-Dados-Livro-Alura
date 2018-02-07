# Lista 5 Exercicios

1. Calcule a média de todas as compras com datas inferiores a 12/05/2013.
2. Calcule a quantidade de compras com datas inferiores a 12/05/2013 e que já foram recebidas.
3. Calcule a soma de todas as compras, agrupadas se a compra recebida ou não.

---

1. Calcule a média de todas as compras com datas inferiores a 12/05/2013.

```sql
SELECT AVG(valor) AS 'Média Compras' FROM compras WHERE data <= '2013-05-12';
+-------------------+
| Média Compras     |
+-------------------+
| 1298.632727272727 |
+-------------------+
1 row in set (0.00 sec)
```

2. Calcule a quantidade de compras com datas inferiores a 12/05/2013 e que já foram recebidas.

```sql
SLECT * FROM compras WHERE data <= '2013-05-12' AND recebida = TRUE;
+----+---------+------------+------------------------+----------+
| id | valor   | data       | observacoes            | recebida |
+----+---------+------------+------------------------+----------+
|  1 | 190.00  | 2012-02-19 | Material escolar       |        1 |
|  3 | 1566.40 | 2012-04-30 | Material de construcao |        1 |
|  4 | 153.45  | 2012-12-15 | Pizza pra familia      |        1 |
|  5 | 4770.00 | 2013-01-23 | Sala de estar          |        1 |
|  6 | 382.15  | 2013-03-03 | Quartos                |        1 |
|  8 | 433.19  | 2013-03-21 | Copa                   |        1 |
| 14 | 2488.00 | 2013-01-12 | Compras de janeiro     |        1 |
| 17 | 758.90  | 2013-01-16 | Festa                  |        1 |
+----+---------+------------+------------------------+----------+
8 rows in set (0.00 sec)

SELECT SUM(valor) AS 'SOMA DAS COMPRAS RECEBIDAS' FROM compras WHERE data < '2013-05-12' AND recebida = TRUE;
+----------------------------+
| SOMA DAS COMPRAS RECEBIDAS |
+----------------------------+
|         10742.089999999998 |
+----------------------------+
1 row in set (0.01 sec)
```

3. Calcule a soma de todas as compras, agrupadas se a compra recebida ou não.


```sql
SELECT SUM(valor) AS 'SOMA DAS COMPRAS', recebida FROM compras WHERE data <= '2013-05-12' GROUP BY recebida;
+--------------------+----------+
| SOMA DAS COMPRAS   | recebida |
+--------------------+----------+
|            3542.87 |        0 |
| 10742.089999999998 |        1 |
+--------------------+----------+
2 rows in set (0.00 sec)
```
