# 3 EXERCÍCIOS

1. Altere as compras, colocando a observação 'preparando o natal' para todas as que foram efetuadas no dia 20/12/2014.  
2. Altere o VALOR das compras feitas antes de 01/06/2013. Some R$10,00 ao valor atual.  
3. Atualize todas as compras feitas entre 01/07/2013 e 01/07/2014 para que elas tenham a observação 'entregue antes de 2014' e a coluna recebida com o valor TRUE.  
4. Em um comando WHERE é possível especificar um intervalo de valores. Para tanto, é preciso dizer qual o valor mínimo e o valor máximo que define o intervalo. Qual é o operador que é usado para isso?   
5. Qual operador você usa para remover linhas de compras de sua tabela?  
6. Exclua as compras realizadas entre 05 e 20 março de 2013.  
7. Existe um operador lógico chamado NOT . Esse operador pode ser usado para negar qualquer condição. Por exemplo, para selecionar qualquer registro com data diferente de 03/11/2014, pode ser
construído o seguinte WHERE: WHERE NOT DATA = '2011-11-03' Use o operador NOT e monte um SELECT que retorna todas as compras com valor diferente de R$ 108,00.  

--- 

1. Altere as compras, colocando a observação 'preparando o natal' para todas as que foram efetuadas no dia 20/12/2014.  

```sql
show databases;
+--------------------+
| Database           |
+--------------------+
| alura              |
| caelum             |
| controledegastos   |
| cursoci            |
| information_schema |
| mysql              |
| performance_schema |
| phpmyadmin         |
| test               |
+--------------------+

SELECT * FROM compras WHERE data='2014-12-20';
+----+--------+------------+-------------------+----------+
| id | valor  | data       | observacoes       | recebida |
+----+--------+------------+-------------------+----------+
| 31 | 253.70 | 2014-12-20 | Natal - presentes |        0 |
+----+--------+------------+-------------------+----------+
1 row in set (0.27 sec)

UPDATE compras SET observacoes = 'preparando o natal' where data = '2014-12-20';
Query OK, 1 row affected (0.04 sec)
Rows matched: 1  Changed: 1  Warnings: 0

SELECT * FROM compras WHERE data='2014-12-20';
+----+--------+------------+--------------------+----------+
| id | valor  | data       | observacoes        | recebida |
+----+--------+------------+--------------------+----------+
| 31 | 253.70 | 2014-12-20 | preparando o natal |        0 |
+----+--------+------------+--------------------+----------+
1 row in set (0.00 sec)

```

2. Altere o VALOR das compras feitas antes de 01/06/2013. Some R$10,00 ao valor atual.  

```sql
SELECT * FROM compras WHERE data < '2013-06-01';
+----+---------+------------+------------------------+----------+
| id | valor   | data       | observacoes            | recebida |
+----+---------+------------+------------------------+----------+
|  1 |  200.00 | 2012-02-19 | Material escolar       |        1 |
|  2 | 3500.00 | 2012-05-21 | Televisao              |        0 |
|  3 | 1576.40 | 2012-04-30 | Material de construcao |        1 |
|  4 |  163.45 | 2012-12-15 | Pizza pra familia      |        1 |
|  5 | 4780.00 | 2013-01-23 | Sala de estar          |        1 |
|  6 |  392.15 | 2013-03-03 | Quartos                |        1 |
|  8 |  443.19 | 2013-03-21 | Copa                   |        1 |
|  9 |   60.48 | 2013-04-12 | Lanchonete             |        0 |
| 10 |   13.57 | 2013-05-23 | Lanchonete             |        0 |
| 12 |   12.39 | 2013-01-06 | Sorvete no parque      |        0 |
| 14 | 2498.00 | 2013-01-12 | Compras de janeiro     |        1 |
| 17 |  768.90 | 2013-01-16 | Festa                  |        1 |
+----+---------+------------+------------------------+----------+
12 rows in set (0.00 sec)

UPDATE compras SET valor = valor - 10.00 WHERE data < '2013-06-01';
Query OK, 12 rows affected (0.03 sec)
Rows matched: 12  Changed: 12  Warnings: 0

SELECT * FROM compras WHERE data < '2013-06-01';
+----+---------+------------+------------------------+----------+
| id | valor   | data       | observacoes            | recebida |
+----+---------+------------+------------------------+----------+
|  1 |  190.00 | 2012-02-19 | Material escolar       |        1 |
|  2 | 3490.00 | 2012-05-21 | Televisao              |        0 |
|  3 | 1566.40 | 2012-04-30 | Material de construcao |        1 |
|  4 |  153.45 | 2012-12-15 | Pizza pra familia      |        1 |
|  5 | 4770.00 | 2013-01-23 | Sala de estar          |        1 |
|  6 |  382.15 | 2013-03-03 | Quartos                |        1 |
|  8 |  433.19 | 2013-03-21 | Copa                   |        1 |
|  9 |   50.48 | 2013-04-12 | Lanchonete             |        0 |
| 10 |    3.57 | 2013-05-23 | Lanchonete             |        0 |
| 12 |    2.39 | 2013-01-06 | Sorvete no parque      |        0 |
| 14 | 2488.00 | 2013-01-12 | Compras de janeiro     |        1 |
| 17 |  758.90 | 2013-01-16 | Festa                  |        1 |
+----+---------+------------+------------------------+----------+
12 rows in set (0.00 sec)
```


3. Atualize todas as compras feitas entre 01/07/2013 e 01/07/2014 para que elas tenham a observação 'entregue antes de 2014' e a coluna recebida com o valor TRUE.  

```sql
 SELECT * FROM compras WHERE data >= '2013-07-01' AND data <= '2014-07-01';
+----+----------+------------+------------------------------+----------+
| id | valor    | data       | observacoes                  | recebida |
+----+----------+------------+------------------------------+----------+
| 11 |    78.65 | 2013-12-04 | Lanchonete                   |        0 |
| 13 |    98.12 | 2013-07-09 | Hopi Hari                    |        1 |
| 15 |  3212.40 | 2013-11-13 | Compras do mes               |        1 |
| 16 |   223.09 | 2013-12-17 | Compras de natal             |        1 |
| 18 |   827.50 | 2014-01-09 | Festa                        |        1 |
| 19 |    12.00 | 2014-02-19 | Salgado no aeroporto         |        1 |
| 20 |   678.43 | 2014-05-21 | Passagem pra Bahia           |        1 |
| 21 | 10937.12 | 2014-04-30 | Carnaval em Cancun           |        1 |
| 22 |  1501.00 | 2014-06-22 | Presente da sogra            |        0 |
| 26 |   909.11 | 2014-02-11 | IPVA                         |        1 |
| 27 |   768.18 | 2014-04-10 | Gasolina viagem Porto Alegre |        1 |
| 28 |   434.00 | 2014-04-01 | Rodeio interior de Sao Paulo |        0 |
| 29 |   115.90 | 2014-06-12 | Dia dos namorados            |        0 |
+----+----------+------------+------------------------------+----------+

UPDATE compras SET observacoes = 'entregue antes de 2014', recebida = TRUE WHERE data >= '2013-07-01' AND data <= '2014-07-01';
Query OK, 13 rows affected (0.36 sec)
Rows matched: 13  Changed: 13  Warnings: 0

SELECT * FROM compras WHERE data >= '2013-07-01' AND data <= '2014-07-01';
+----+----------+------------+------------------------+----------+
| id | valor    | data       | observacoes            | recebida |
+----+----------+------------+------------------------+----------+
| 11 |    78.65 | 2013-12-04 | entregue antes de 2014 |        1 |
| 13 |    98.12 | 2013-07-09 | entregue antes de 2014 |        1 |
| 15 |  3212.40 | 2013-11-13 | entregue antes de 2014 |        1 |
| 16 |   223.09 | 2013-12-17 | entregue antes de 2014 |        1 |
| 18 |   827.50 | 2014-01-09 | entregue antes de 2014 |        1 |
| 19 |    12.00 | 2014-02-19 | entregue antes de 2014 |        1 |
| 20 |   678.43 | 2014-05-21 | entregue antes de 2014 |        1 |
| 21 | 10937.12 | 2014-04-30 | entregue antes de 2014 |        1 |
| 22 |  1501.00 | 2014-06-22 | entregue antes de 2014 |        1 |
| 26 |   909.11 | 2014-02-11 | entregue antes de 2014 |        1 |
| 27 |   768.18 | 2014-04-10 | entregue antes de 2014 |        1 |
| 28 |   434.00 | 2014-04-01 | entregue antes de 2014 |        1 |
| 29 |   115.90 | 2014-06-12 | entregue antes de 2014 |        1 |
+----+----------+------------+------------------------+----------+

```

4. Em um comando WHERE é possível especificar um intervalo de valores. Para tanto, é preciso dizer qual o valor mínimo e o valor máximo que define o intervalo. Qual é o operador que é usado para isso?   

```sql

 SELECT * FROM compras WHERE id <= 5 OR id >= 40 ;
+----+---------+------------+------------------------+----------+
| id | valor   | data       | observacoes            | recebida |
+----+---------+------------+------------------------+----------+
|  1 |  190.00 | 2012-02-19 | Material escolar       |        1 |
|  2 | 3490.00 | 2012-05-21 | Televisao              |        0 |
|  3 | 1566.40 | 2012-04-30 | Material de construcao |        1 |
|  4 |  153.45 | 2012-12-15 | Pizza pra familia      |        1 |
|  5 | 4770.00 | 2013-01-23 | Sala de estar          |        1 |
| 40 |   12.34 | 2015-07-19 | Canetas                |        0 |
| 41 |   87.43 | 2015-05-10 | Gravata                |        0 |
| 42 |  887.66 | 2015-02-02 | Presente para o filhao |        1 |
| 43 |  100.00 | 2015-09-02 | COMIDA                 |        1 |
| 44 |  100.00 | 2015-09-02 | COMIDA                 |        0 |
+----+---------+------------+------------------------+----------+

 SELECT * FROM compras WHERE id BETWEEN 35 AND 40 ;
+----+---------+------------+-------------------+----------+
| id | valor   | data       | observacoes       | recebida |
+----+---------+------------+-------------------+----------+
| 35 |   98.70 | 2015-02-07 | Lanchonete        |        1 |
| 36 |  213.50 | 2015-09-25 | Roupas            |        0 |
| 37 | 1245.20 | 2015-10-17 | Roupas            |        0 |
| 38 |   23.78 | 2015-12-18 | Lanchonete do Z?® |        1 |
| 39 |  576.12 | 2015-09-13 | Sapatos           |        1 |
| 40 |   12.34 | 2015-07-19 | Canetas           |        0 |
+----+---------+------------+-------------------+----------+


```

5. Qual operador você usa para remover linhas de compras de sua tabela?  

```sql
SELECT * FROM compras WHERE id >= 42;
+----+--------+------------+------------------------+----------+
| id | valor  | data       | observacoes            | recebida |
+----+--------+------------+------------------------+----------+
| 42 | 887.66 | 2015-02-02 | Presente para o filhao |        1 |
| 43 | 100.00 | 2015-09-02 | COMIDA                 |        1 |
| 44 | 100.00 | 2015-09-02 | COMIDA                 |        0 |
+----+--------+------------+------------------------+----------+
3 rows in set (0.00 sec)

DELETE FROM compras WHERE id = 44;
Query OK, 1 row affected (0.10 sec)

SELECT * FROM compras WHERE id >= 42;
+----+--------+------------+------------------------+----------+
| id | valor  | data       | observacoes            | recebida |
+----+--------+------------+------------------------+----------+
| 42 | 887.66 | 2015-02-02 | Presente para o filhao |        1 |
| 43 | 100.00 | 2015-09-02 | COMIDA                 |        1 |
+----+--------+------------+------------------------+----------+
```

6. Exclua as compras realizadas entre 05 e 20 março de 2013.  

```sql
DELETE FROM compras WHERE data <= '2013-03-20' AND data >= '2013-03-05';
Query OK, 0 rows affected (0.00 sec)
```

7. Existe um operador lógico chamado NOT . Esse operador pode ser usado para negar qualquer condição. Por exemplo, para selecionar qualquer registro com data diferente de 03/11/2014, pode ser construído o seguinte WHERE: WHERE NOT DATA = '2011-11-03' Use o operador NOT e monte um SELECT que retorna todas as compras com valor diferente de R$ 108,00.  

```sql
--ajustes...
SELECT * FROM compras WHERE NOT valor <= 1000.00;
+----+----------+------------+------------------------+----------+
| id | valor    | data       | observacoes            | recebida |
+----+----------+------------+------------------------+----------+
|  2 |  3490.00 | 2012-05-21 | Televisao              |        0 |
|  3 |  1566.40 | 2012-04-30 | Material de construcao |        1 |
|  5 |  4770.00 | 2013-01-23 | Sala de estar          |        1 |
| 14 |  2488.00 | 2013-01-12 | Compras de janeiro     |        1 |
| 15 |  3212.40 | 2013-11-13 | entregue antes de 2014 |        1 |
| 21 | 10937.12 | 2014-04-30 | entregue antes de 2014 |        1 |
| 22 |  1501.00 | 2014-06-22 | entregue antes de 2014 |        1 |
| 23 |  1709.00 | 2014-08-25 | Parcela da casa        |        0 |
| 37 |  1245.20 | 2015-10-17 | Roupas                 |        0 |
+----+----------+------------+------------------------+----------+
```
