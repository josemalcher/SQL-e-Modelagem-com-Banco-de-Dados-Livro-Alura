# 2.14 EXERCÍCIOS

1. Instale o servidor do MySQL em sua máquina. Qual o sistema operacional em que você fez sua instalação? Sentiu que algo podia ser simplificado no processo de instalação? Lembre-se que você pode realizar o download de ambos em http://MySQL.com/downloads/MySQL . Você pode optar por baixar a versão mais atual.
2. Logue no MySQL, e comece criando o banco de dados:
```sql
mysql -uroot -p
CREATE DATABASE ControleDeGastos;
USE ControleDeGastos;
```
Agora vamos criar a tabela. Ela precisa ter os seguintes campos: id inteiro, valor número com vírgula, data , observacoes e um booleano para marcar se a compra foi recebida . A tabela deve-se chamar "compras".
3. Clique aqui e faça o download do arquivo .sql, e importe no MySQL: https://s3.amazonaws.com/caelum-online-public/alura-sql/scripts/compras.sql
```
mysql -u root -p ControleDeGastos < compras.sql
```
Em seguida, execute o SELECT para garantir que todas as informaçoes foram adicionadas:
```sql
SELECT * FROM compras;
```
DICA: Salve o arquivo compras.sql em uma pasta de fácil acesso na linha de comando. Além disso, o arquivo deve estar no mesmo lugar onde você executará o comando.

1. Selecione valor e observacoes de todas as compras cuja data seja maior-ou-igual que 15/12/2012.  
2. Qual o comando SQL para juntar duas condições diferentes? Por exemplo, SELECT * FROM TABELA WHERE campo > 1000 campo < 5000. Faça o teste e veja o resultado.  
3. Vimos que todo texto é passado através de aspas simples ('). Posso passar aspas duplas (") no lugar?  
4. Selecione todas as compras cuja data seja maior-ou-igual que 15/12/2012 e menor do que 15/12/2014.  
5. Selecione todas as compras cujo valor esteja entre R$15,00 e R$35,00 e a observação comece com a palavra 'Lanchonete'.  
6. Selecione todas as compras que já foram recebidas.  
7. Selecione todas as compras que ainda não foram recebidas.  
8. Vimos que para guardar o valor VERDADEIRO para a coluna recebida , devemos passar o valor 1.  
```
Para FALSO, devemos passar o valor 0. E quanto as palavras já conhecidas para verdadeiro e falso:
TRUE e FALSE . Elas funcionam? Ou seja:
INSERT INTO compras (valor, data, observacoes, recebida) VALUES (100.0, '2015-09-08', 'COMIDA', TRUE)
;
```
Funciona? Faça o teste.  
9. Selecione todas as compras com valor maior que 5.000,00 ou que já foram recebidas.  
10. Selecione todas as compras que o valor esteja entre 1.000,00 e 3.000,00 ou seja maior que 5.000,00.  

---

1. Selecione valor e observacoes de todas as compras cuja data seja maior-ou-igual que 15/12/2012.  

```sql
SELECT valor,observacoes,data 
FROM compras 
WHERE valor >= '2012-12-15';
+----------+--------------------+------------+
| valor    | observacoes        | data       |
+----------+--------------------+------------+
|  3500.00 | Televisao          | 2012-05-21 |
|  4780.00 | Sala de estar      | 2013-01-23 |
|  2498.00 | Compras de janeiro | 2013-01-12 |
|  3212.40 | Compras do mes     | 2013-11-13 |
| 10937.12 | Carnaval em Cancun | 2014-04-30 |
+----------+--------------------+------------+
```

2. Qual o comando SQL para juntar duas condições diferentes? Por exemplo, SELECT * FROM TABELA WHERE campo > 1000 campo < 5000. Faça o teste e veja o resultado.  

```sql
 SELECT * FROM compras 
 WHERE observacoes 
 LIKE 'Compras%' 
 AND data LIKE '2013%';
+----+---------+------------+--------------------+----------+
| id | valor   | data       | observacoes        | recebida |
+----+---------+------------+--------------------+----------+
| 14 | 2498.00 | 2013-01-12 | Compras de janeiro |        1 |
| 15 | 3212.40 | 2013-11-13 | Compras do mes     |        1 |
| 16 |  223.09 | 2013-12-17 | Compras de natal   |        1 |
+----+---------+------------+--------------------+----------+
```

3. Vimos que todo texto é passado através de aspas simples ('). Posso passar aspas duplas (") no lugar?  

https://pt.stackoverflow.com/questions/109951/qual-a-diferen%C3%A7a-entre-aspa-simples-e-aspa-dupla-no-sql

```sql
-- sim
SELECT * FROM compras WHERE observacoes LIKE "Compras%" AND data LIKE "2013%";
+----+---------+------------+--------------------+----------+
| id | valor   | data       | observacoes        | recebida |
+----+---------+------------+--------------------+----------+
| 14 | 2498.00 | 2013-01-12 | Compras de janeiro |        1 |
| 15 | 3212.40 | 2013-11-13 | Compras do mes     |        1 |
| 16 |  223.09 | 2013-12-17 | Compras de natal   |        1 |
+----+---------+------------+--------------------+----------+
```

4. Selecione todas as compras cuja data seja maior-ou-igual que 15/12/2012 e menor do que 15/12/2014.  

```sql
 SELECT * FROM compras WHERE data >= '2012-12-15' AND data <= '2014-12-15';
+----+----------+------------+------------------------------+----------+
| id | valor    | data       | observacoes                  | recebida |
+----+----------+------------+------------------------------+----------+
|  4 |   163.45 | 2012-12-15 | Pizza pra familia            |        1 |
|  5 |  4780.00 | 2013-01-23 | Sala de estar                |        1 |
|  6 |   392.15 | 2013-03-03 | Quartos                      |        1 |
|  8 |   443.19 | 2013-03-21 | Copa                         |        1 |
|  9 |    60.48 | 2013-04-12 | Lanchonete                   |        0 |
| 10 |    13.57 | 2013-05-23 | Lanchonete                   |        0 |
| 11 |    78.65 | 2013-12-04 | Lanchonete                   |        0 |
| 12 |    12.39 | 2013-01-06 | Sorvete no parque            |        0 |
| 13 |    98.12 | 2013-07-09 | Hopi Hari                    |        1 |
| 14 |  2498.00 | 2013-01-12 | Compras de janeiro           |        1 |
| 15 |  3212.40 | 2013-11-13 | Compras do mes               |        1 |
| 16 |   223.09 | 2013-12-17 | Compras de natal             |        1 |
| 17 |   768.90 | 2013-01-16 | Festa                        |        1 |
| 18 |   827.50 | 2014-01-09 | Festa                        |        1 |
| 19 |    12.00 | 2014-02-19 | Salgado no aeroporto         |        1 |
| 20 |   678.43 | 2014-05-21 | Passagem pra Bahia           |        1 |
| 21 | 10937.12 | 2014-04-30 | Carnaval em Cancun           |        1 |
| 22 |  1501.00 | 2014-06-22 | Presente da sogra            |        0 |
| 23 |  1709.00 | 2014-08-25 | Parcela da casa              |        0 |
| 24 |   567.09 | 2014-09-25 | Parcela do carro             |        0 |
| 25 |   631.53 | 2014-10-12 | IPTU                         |        1 |
| 26 |   909.11 | 2014-02-11 | IPVA                         |        1 |
| 27 |   768.18 | 2014-04-10 | Gasolina viagem Porto Alegre |        1 |
| 28 |   434.00 | 2014-04-01 | Rodeio interior de Sao Paulo |        0 |
| 29 |   115.90 | 2014-06-12 | Dia dos namorados            |        0 |
| 30 |    98.00 | 2014-10-12 | Dia das crian?ºas            |        0 |
+----+----------+------------+------------------------------+----------+
26 rows in set (0.00 sec)

SELECT * 
FROM compras 
WHERE data BETWEEN '2012-12-15' AND '2014-12-15';
+----+----------+------------+------------------------------+----------+
| id | valor    | data       | observacoes                  | recebida |
+----+----------+------------+------------------------------+----------+
|  4 |   163.45 | 2012-12-15 | Pizza pra familia            |        1 |
|  5 |  4780.00 | 2013-01-23 | Sala de estar                |        1 |
|  6 |   392.15 | 2013-03-03 | Quartos                      |        1 |
|  8 |   443.19 | 2013-03-21 | Copa                         |        1 |
|  9 |    60.48 | 2013-04-12 | Lanchonete                   |        0 |
| 10 |    13.57 | 2013-05-23 | Lanchonete                   |        0 |
| 11 |    78.65 | 2013-12-04 | Lanchonete                   |        0 |
| 12 |    12.39 | 2013-01-06 | Sorvete no parque            |        0 |
| 13 |    98.12 | 2013-07-09 | Hopi Hari                    |        1 |
| 14 |  2498.00 | 2013-01-12 | Compras de janeiro           |        1 |
| 15 |  3212.40 | 2013-11-13 | Compras do mes               |        1 |
| 16 |   223.09 | 2013-12-17 | Compras de natal             |        1 |
| 17 |   768.90 | 2013-01-16 | Festa                        |        1 |
| 18 |   827.50 | 2014-01-09 | Festa                        |        1 |
| 19 |    12.00 | 2014-02-19 | Salgado no aeroporto         |        1 |
| 20 |   678.43 | 2014-05-21 | Passagem pra Bahia           |        1 |
| 21 | 10937.12 | 2014-04-30 | Carnaval em Cancun           |        1 |
| 22 |  1501.00 | 2014-06-22 | Presente da sogra            |        0 |
| 23 |  1709.00 | 2014-08-25 | Parcela da casa              |        0 |
| 24 |   567.09 | 2014-09-25 | Parcela do carro             |        0 |
| 25 |   631.53 | 2014-10-12 | IPTU                         |        1 |
| 26 |   909.11 | 2014-02-11 | IPVA                         |        1 |
| 27 |   768.18 | 2014-04-10 | Gasolina viagem Porto Alegre |        1 |
| 28 |   434.00 | 2014-04-01 | Rodeio interior de Sao Paulo |        0 |
| 29 |   115.90 | 2014-06-12 | Dia dos namorados            |        0 |
| 30 |    98.00 | 2014-10-12 | Dia das crian?ºas            |        0 |
+----+----------+------------+------------------------------+----------+
26 rows in set (0.00 sec)
```

5. Selecione todas as compras cujo valor esteja entre R$15,00 e R$35,00 e a observação comece com a palavra 'Lanchonete'.  

```sql
SELECT id,valor,observacoes 
FROM compras 
WHERE observacoes LIKE 'Lanchonete%' 
AND valor BETWEEN 15 AND 35;
+----+-------+-------------------+
| id | valor | observacoes       |
+----+-------+-------------------+
| 33 | 32.09 | Lanchonete        |
| 38 | 23.78 | Lanchonete do Z?® |
+----+-------+-------------------+
```

6. Selecione todas as compras que já foram recebidas.  

```sql
 SELECT * 
 FROM compras 
 WHERE recebida = 1;
+----+----------+------------+------------------------------+----------+
| id | valor    | data       | observacoes                  | recebida |
+----+----------+------------+------------------------------+----------+
|  1 |   200.00 | 2012-02-19 | Material escolar             |        1 |
|  3 |  1576.40 | 2012-04-30 | Material de construcao       |        1 |
|  4 |   163.45 | 2012-12-15 | Pizza pra familia            |        1 |
|  5 |  4780.00 | 2013-01-23 | Sala de estar                |        1 |
|  6 |   392.15 | 2013-03-03 | Quartos                      |        1 |
|  8 |   443.19 | 2013-03-21 | Copa                         |        1 |
| 13 |    98.12 | 2013-07-09 | Hopi Hari                    |        1 |
| 14 |  2498.00 | 2013-01-12 | Compras de janeiro           |        1 |
| 15 |  3212.40 | 2013-11-13 | Compras do mes               |        1 |
| 16 |   223.09 | 2013-12-17 | Compras de natal             |        1 |
| 17 |   768.90 | 2013-01-16 | Festa                        |        1 |
| 18 |   827.50 | 2014-01-09 | Festa                        |        1 |
| 19 |    12.00 | 2014-02-19 | Salgado no aeroporto         |        1 |
| 20 |   678.43 | 2014-05-21 | Passagem pra Bahia           |        1 |
| 21 | 10937.12 | 2014-04-30 | Carnaval em Cancun           |        1 |
| 25 |   631.53 | 2014-10-12 | IPTU                         |        1 |
| 26 |   909.11 | 2014-02-11 | IPVA                         |        1 |
| 27 |   768.18 | 2014-04-10 | Gasolina viagem Porto Alegre |        1 |
| 33 |    32.09 | 2015-07-02 | Lanchonete                   |        1 |
| 34 |   954.12 | 2015-11-03 | Show da Ivete Sangalo        |        1 |
| 35 |    98.70 | 2015-02-07 | Lanchonete                   |        1 |
| 38 |    23.78 | 2015-12-18 | Lanchonete do Z?®            |        1 |
| 39 |   576.12 | 2015-09-13 | Sapatos                      |        1 |
| 42 |   887.66 | 2015-02-02 | Presente para o filhao       |        1 |
+----+----------+------------+------------------------------+----------+
```

7. Selecione todas as compras que ainda não foram recebidas.  

```sql
SELECT * FROM compras WHERE recebida = 0;
+----+---------+------------+------------------------------+----------+
| id | valor   | data       | observacoes                  | recebida |
+----+---------+------------+------------------------------+----------+
|  2 | 3500.00 | 2012-05-21 | Televisao                    |        0 |
|  9 |   60.48 | 2013-04-12 | Lanchonete                   |        0 |
| 10 |   13.57 | 2013-05-23 | Lanchonete                   |        0 |
| 11 |   78.65 | 2013-12-04 | Lanchonete                   |        0 |
| 12 |   12.39 | 2013-01-06 | Sorvete no parque            |        0 |
| 22 | 1501.00 | 2014-06-22 | Presente da sogra            |        0 |
| 23 | 1709.00 | 2014-08-25 | Parcela da casa              |        0 |
| 24 |  567.09 | 2014-09-25 | Parcela do carro             |        0 |
| 28 |  434.00 | 2014-04-01 | Rodeio interior de Sao Paulo |        0 |
| 29 |  115.90 | 2014-06-12 | Dia dos namorados            |        0 |
| 30 |   98.00 | 2014-10-12 | Dia das crian?ºas            |        0 |
| 31 |  253.70 | 2014-12-20 | Natal - presentes            |        0 |
| 32 |  370.15 | 2014-12-25 | Compras de natal             |        0 |
| 36 |  213.50 | 2015-09-25 | Roupas                       |        0 |
| 37 | 1245.20 | 2015-10-17 | Roupas                       |        0 |
| 40 |   12.34 | 2015-07-19 | Canetas                      |        0 |
| 41 |   87.43 | 2015-05-10 | Gravata                      |        0 |
+----+---------+------------+------------------------------+----------+
```

8. Vimos que para guardar o valor VERDADEIRO para a coluna recebida , devemos passar o valor 1.  

Para FALSO, devemos passar o valor 0. E quanto as palavras já conhecidas para verdadeiro e falso:
TRUE e FALSE . Elas funcionam? Ou seja:
INSERT INTO compras (valor, data, observacoes, recebida) VALUES (100.0, '2015-09-08', 'COMIDA', TRUE)
;
Funciona? Faça o teste.  

```sql
MariaDB [ControleDeGastos]> INSERT INTO compras(valor,data,observacoes,recebida) VALUES(100.0, '2015-09-02', 'COMIDA', TRUE);
Query OK, 1 row affected (0.03 sec)

MariaDB [ControleDeGastos]> INSERT INTO compras(valor,data,observacoes,recebida) VALUES(100.0, '2015-09-02', 'COMIDA', FALSE);
Query OK, 1 row affected (0.00 sec)

 SELECT * FROM compras WHERE observacoes='COMIDA';
+----+--------+------------+-------------+----------+
| id | valor  | data       | observacoes | recebida |
+----+--------+------------+-------------+----------+
| 43 | 100.00 | 2015-09-02 | COMIDA      |        1 |
| 44 | 100.00 | 2015-09-02 | COMIDA      |        0 |
+----+--------+------------+-------------+----------+
```

9. Selecione todas as compras com valor maior que 5.000,00 ou que já foram recebidas.  

```sql
 SELECT * 
 FROM compras 
 WHERE recebida = TRUE;
+----+----------+------------+------------------------------+----------+
| id | valor    | data       | observacoes                  | recebida |
+----+----------+------------+------------------------------+----------+
|  1 |   200.00 | 2012-02-19 | Material escolar             |        1 |
|  3 |  1576.40 | 2012-04-30 | Material de construcao       |        1 |
|  4 |   163.45 | 2012-12-15 | Pizza pra familia            |        1 |
|  5 |  4780.00 | 2013-01-23 | Sala de estar                |        1 |
|  6 |   392.15 | 2013-03-03 | Quartos                      |        1 |
|  8 |   443.19 | 2013-03-21 | Copa                         |        1 |
| 13 |    98.12 | 2013-07-09 | Hopi Hari                    |        1 |
| 14 |  2498.00 | 2013-01-12 | Compras de janeiro           |        1 |
| 15 |  3212.40 | 2013-11-13 | Compras do mes               |        1 |
| 16 |   223.09 | 2013-12-17 | Compras de natal             |        1 |
| 17 |   768.90 | 2013-01-16 | Festa                        |        1 |
| 18 |   827.50 | 2014-01-09 | Festa                        |        1 |
| 19 |    12.00 | 2014-02-19 | Salgado no aeroporto         |        1 |
| 20 |   678.43 | 2014-05-21 | Passagem pra Bahia           |        1 |
| 21 | 10937.12 | 2014-04-30 | Carnaval em Cancun           |        1 |
| 25 |   631.53 | 2014-10-12 | IPTU                         |        1 |
| 26 |   909.11 | 2014-02-11 | IPVA                         |        1 |
| 27 |   768.18 | 2014-04-10 | Gasolina viagem Porto Alegre |        1 |
| 33 |    32.09 | 2015-07-02 | Lanchonete                   |        1 |
| 34 |   954.12 | 2015-11-03 | Show da Ivete Sangalo        |        1 |
| 35 |    98.70 | 2015-02-07 | Lanchonete                   |        1 |
| 38 |    23.78 | 2015-12-18 | Lanchonete do Z?®            |        1 |
| 39 |   576.12 | 2015-09-13 | Sapatos                      |        1 |
| 42 |   887.66 | 2015-02-02 | Presente para o filhao       |        1 |
| 43 |   100.00 | 2015-09-02 | COMIDA                       |        1 |
+----+----------+------------+------------------------------+----------+
25 rows in set (0.00 sec)
```

10. Selecione todas as compras que o valor esteja entre 1.000,00 e 3.000,00 ou seja maior que 5.000,00.  


```sql
SELECT * FROM compras 
WHERE valor >= 1000 
AND valor <= 3000 
OR valor > 5000;
+----+----------+------------+------------------------+----------+
| id | valor    | data       | observacoes            | recebida |
+----+----------+------------+------------------------+----------+
|  3 |  1576.40 | 2012-04-30 | Material de construcao |        1 |
| 14 |  2498.00 | 2013-01-12 | Compras de janeiro     |        1 |
| 21 | 10937.12 | 2014-04-30 | Carnaval em Cancun     |        1 |
| 22 |  1501.00 | 2014-06-22 | Presente da sogra      |        0 |
| 23 |  1709.00 | 2014-08-25 | Parcela da casa        |        0 |
| 37 |  1245.20 | 2015-10-17 | Roupas                 |        0 |
+----+----------+------------+------------------------+----------+
6 rows in set (0.00 sec)
```
