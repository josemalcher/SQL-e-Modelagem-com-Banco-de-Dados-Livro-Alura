# SQL e Modelagem com Banco de Dados - Livro Alura

Link: 

http://blog.alura.com.br/liberada-a-apostila-gratuita-de-sql-do-alura/

https://www.alura.com.br/apostilas

---

## <a name="indice">Índice</a>

- [1 Objetivos do curso](#parte1)   
- [2 Meu problema](#parte2)   
- [3 Atualizando e excluindo dados](#parte3)   
- [4 Alterando e restringindo o formato de nossas tabelas](#parte4)   
- [5 Agrupando dados e fazendo consultas mais inteligentes](#parte5)   
- [6 Juntando dados de várias tabelas](#parte6)   
- [7 Alunos sem matrícula e o Exists](#parte7)   
- [8 Agrupando dados com GROUP BY](#parte8)   
- [9 Filtrando agregações e o HAVING](#parte9)   
- [10 Múltiplos valores na condição e o IN](#parte10)   
- [11 Sub-queries](#parte11)   
- [12 Entendendo o LEFT JOIN](#parte12)   
- [13 Muitos alunos e o LIMIT](#parte13)   



---

## <a name="parte1">1 Objetivos do curso</a>

Tirar dúvidas em: http://www.guj.com.br

[Voltar ao Índice](#indice)

---

## <a name="parte2">2 Meu problema</a>

http://dev.mysql.com/downloads/mysql/

```sql
C:\Users\josemalcher\Documents\01-SERVs\xampp_php7.2.1\mysql\bin
λ mysql -uroot -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 13
Server version: 10.1.30-MariaDB mariadb.org binary distribution

Copyright (c) 2000, 2017, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
```

O -u indica o usuário root , e o -p é porque digitaremos a senha. Use a senha que definiu durante a instalação do MySQL, note que por padrão a senha pode ser em branco e, nesse caso, basta pressionar enter.

### 2.2 COMEÇANDO UM CADERNO NOVO: CRIANDO O BANCO

```sql
MariaDB [(none)]> create database caelum;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create database alura;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create database ControleDeGastos;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> USE ControleDeGastos;
Database changed
MariaDB [ControleDeGastos]>

```
### 2.4 A TABELA DE COMPRAS

```sql
MariaDB [ControleDeGastos]> CREATE TABLE compras(valor DECIMAL(18,2), data DATE, observacoes VARCHAR(255), recebida TINYINT);
Query OK, 0 rows affected (0.20 sec)
```

### 2.5 CONFERINDO A EXISTÊNCIA DE UMA TABELA

```sql
MariaDB [ControleDeGastos]> desc compras;
+-------------+---------------+------+-----+---------+-------+
| Field       | Type          | Null | Key | Default | Extra |
+-------------+---------------+------+-----+---------+-------+
| valor       | decimal(18,2) | YES  |     | NULL    |       |
| data        | date          | YES  |     | NULL    |       |
| observacoes | varchar(255)  | YES  |     | NULL    |       |
| recebida    | tinyint(4)    | YES  |     | NULL    |       |
+-------------+---------------+------+-----+---------+-------+
4 rows in set (0.31 sec)

```

### 2.6 INSERINDO REGISTROS NO BANCO DE DADOS

```sql
MariaDB [ControleDeGastos]> INSERT INTO compras VALUES(20,'2018-02-03','Lanchonete', 1);
Query OK, 1 row affected (0.02 sec)
```

### 2.7 SELECÃO SIMPLES

```sql
MariaDB [ControleDeGastos]> SELECT * FROM compras;
+-------+------------+-------------+----------+
| valor | data       | observacoes | recebida |
+-------+------------+-------------+----------+
| 20.00 | 2018-02-03 | Lanchonete  |        1 |
+-------+------------+-------------+----------+
1 row in set (0.00 sec)
```

### 2.8 A FORMATAÇÃO DE NÚMEROS DECIMAIS

```sql
MariaDB [ControleDeGastos]> INSERT INTO compras VALUES(987.3, '2018-02-03', 'Guarda-roupa', 0);
Query OK, 1 row affected (0.29 sec)

MariaDB [ControleDeGastos]> SELECT * FROM compras;
+--------+------------+--------------+----------+
| valor  | data       | observacoes  | recebida |
+--------+------------+--------------+----------+
|  20.00 | 2018-02-03 | Lanchonete   |        1 |
| 987.30 | 2018-02-03 | Guarda-roupa |        0 |
+--------+------------+--------------+----------+
2 rows in set (0.00 sec)
```

#### inserir os dados em uma ordem diferente

```sql
MariaDB [ControleDeGastos]> INSERT INTO compras(data,observacoes,valor,recebida) VALUES('2018-01-03','Celular Moto X ', 1200.99, 0 );
Query OK, 1 row affected (0.01 sec)

MariaDB [ControleDeGastos]> SELECT * FROM compras;
+---------+------------+-----------------+----------+
| valor   | data       | observacoes     | recebida |
+---------+------------+-----------------+----------+
|   20.00 | 2018-02-03 | Lanchonete      |        1 |
|  987.30 | 2018-02-03 | Guarda-roupa    |        0 |
| 1200.99 | 2018-01-03 | Celular Moto X  |        0 |
+---------+------------+-----------------+----------+
3 rows in set (0.00 sec)

```

### 2.9 A CHAVE PRIMÁRIA

É isso que queremos, um campo que seja nossa chave primária ( PRIMARY KEY ), que é um número inteiro ( INT ), e que seja automaticamente incrementado ( AUTO_INCREMENT ). Só falta definir o nome dele, que como identifica nossa compra, usamos a abreviação id . Esse é um padrão  amplamente utilizado. Portanto queremos um campo id INT AUTO_INCREMENT PRIMARY KEY .

#### alterar nossa tabela ( ALTER_TABLE )

```sql
MariaDB [ControleDeGastos]> ALTER TABLE compras ADD COLUMN id INT AUTO_INCREMENT PRIMARY KEY;
Query OK, 0 rows affected (0.09 sec)
Records: 0  Duplicates: 0  Warnings: 0

MariaDB [ControleDeGastos]> SELECT * FROM compras;
+---------+------------+-----------------+----------+----+
| valor   | data       | observacoes     | recebida | id |
+---------+------------+-----------------+----------+----+
|   20.00 | 2018-02-03 | Lanchonete      |        1 |  1 |
|  987.30 | 2018-02-03 | Guarda-roupa    |        0 |  2 |
| 1200.99 | 2018-01-03 | Celular Moto X  |        0 |  3 |
+---------+------------+-----------------+----------+----+
3 rows in set (0.00 sec)
```

### 2.10 RECRIANDO A TABELA DO ZERO

#### Apagando a Tabela anterior compras
```sql
MariaDB [ControleDeGastos]> DROP TABLE compras;
Query OK, 0 rows affected (0.20 sec)

MariaDB [ControleDeGastos]> SELECT * FROM compras;
ERROR 1146 (42S02): Table 'controledegastos.compras' doesn't exist
```

#### Recriando compras
```sql
MariaDB [ControleDeGastos]> CREATE TABLE compras(
    -> id INT AUTO_INCREMENT PRIMARY KEY,
    -> valor DECIMAL(18,2),
    -> data DATE,
    -> observacoes VARCHAR(255),
    -> recebida TINYINT);
Query OK, 0 rows affected (0.04 sec)

MariaDB [ControleDeGastos]> SELECT * FROM compras;
Empty set (0.00 sec)

MariaDB [ControleDeGastos]> DESC compras;
+----------+---------------+------+-----+---------+----------------+
| Field    | Type          | Null | Key | Default | Extra          |
+----------+---------------+------+-----+---------+----------------+
| id       | int(11)       | NO   | PRI | NULL    | auto_increment |
| valor    | decimal(18,2) | YES  |     | NULL    |                |
| data     | date          | YES  |     | NULL    |                |
| obs      | varchar(255)  | YES  |     | NULL    |                |
| recebida | tinyint(4)    | YES  |     | NULL    |                |
+----------+---------------+------+-----+---------+----------------+
5 rows in set (0.01 sec)
```
#### inserindo dados

```sql
MariaDB [ControleDeGastos]> INSERT INTO compras(valor,data,obs,recebida) VALUES(20.99, '2018-01-03','Capa de celular', 1);
Query OK, 1 row affected (0.00 sec)

MariaDB [ControleDeGastos]> INSERT INTO compras(valor,data,obs,recebida) VALUES(159.99, '2018-01-02','Celular Moto X Play', 1);
Query OK, 1 row affected (0.00 sec)

MariaDB [ControleDeGastos]> INSERT INTO compras(valor,data,obs,recebida) VALUES(1590.99, '2018-01-01','Contas Cartão', 1);
Query OK, 1 row affected (0.00 sec)

MariaDB [ControleDeGastos]> SELECT * FROM compras;
+----+---------+------------+---------------------+----------+
| id | valor   | data       | obs                 | recebida |
+----+---------+------------+---------------------+----------+
|  1 |   20.99 | 2018-01-03 | Capa de celular     |        1 |
|  2 |  159.99 | 2018-01-02 | Celular Moto X Play |        1 |
|  3 | 1590.99 | 2018-01-01 | Contas Cartão       |        1 |
+----+---------+------------+---------------------+----------+
3 rows in set (0.00 sec)
```

### 2.11 CONSULTAS COM FILTROS

Imprtar dados: 
```
    mysql -uroot -p ControleDeGastos < compras.sql
```
```sql
MariaDB [ControleDeGastos]> select * from compras;
+----+----------+------------+------------------------------+----------+
| id | valor    | data       | observacoes                  | recebida |
+----+----------+------------+------------------------------+----------+
|  1 |   200.00 | 2012-02-19 | Material escolar             |        1 |
|  2 |  3500.00 | 2012-05-21 | Televisao                    |        0 |
|  3 |  1576.40 | 2012-04-30 | Material de construcao       |        1 |
|  4 |   163.45 | 2012-12-15 | Pizza pra familia            |        1 |
|  5 |  4780.00 | 2013-01-23 | Sala de estar                |        1 |
|  6 |   392.15 | 2013-03-03 | Quartos                      |        1 |
|  7 |  1203.00 | 2013-03-18 | Quartos                      |        1 |
|  8 |   402.90 | 2013-03-21 | Copa                         |        1 |
|  9 |    54.98 | 2013-04-12 | Lanchonete                   |        0 |
| 10 |    12.34 | 2013-05-23 | Lanchonete                   |        0 |
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
| 31 |   253.70 | 2014-12-20 | Natal - presentes            |        0 |
| 32 |   370.15 | 2014-12-25 | Compras de natal             |        0 |
| 33 |    32.09 | 2015-07-02 | Lanchonete                   |        1 |
| 34 |   954.12 | 2015-11-03 | Show da Ivete Sangalo        |        1 |
| 35 |    98.70 | 2015-02-07 | Lanchonete                   |        1 |
| 36 |   213.50 | 2015-09-25 | Roupas                       |        0 |
| 37 |  1245.20 | 2015-10-17 | Roupas                       |        0 |
| 38 |    23.78 | 2015-12-18 | Lanchonete do Z?®            |        1 |
| 39 |   576.12 | 2015-09-13 | Sapatos                      |        1 |
| 40 |    12.34 | 2015-07-19 | Canetas                      |        0 |
| 41 |    87.43 | 2015-05-10 | Gravata                      |        0 |
| 42 |   887.66 | 2015-02-02 | Presente para o filhao       |        1 |
+----+----------+------------+------------------------------+----------+
42 rows in set (0.00 sec)

MariaDB [ControleDeGastos]> SELECT * FROM compras WHERE valor < 500;
+----+--------+------------+------------------------------+----------+
| id | valor  | data       | observacoes                  | recebida |
+----+--------+------------+------------------------------+----------+
|  1 | 200.00 | 2012-02-19 | Material escolar             |        1 |
|  4 | 163.45 | 2012-12-15 | Pizza pra familia            |        1 |
|  6 | 392.15 | 2013-03-03 | Quartos                      |        1 |
|  8 | 402.90 | 2013-03-21 | Copa                         |        1 |
|  9 |  54.98 | 2013-04-12 | Lanchonete                   |        0 |
| 10 |  12.34 | 2013-05-23 | Lanchonete                   |        0 |
| 11 |  78.65 | 2013-12-04 | Lanchonete                   |        0 |
| 12 |  12.39 | 2013-01-06 | Sorvete no parque            |        0 |
| 13 |  98.12 | 2013-07-09 | Hopi Hari                    |        1 |
| 16 | 223.09 | 2013-12-17 | Compras de natal             |        1 |
| 19 |  12.00 | 2014-02-19 | Salgado no aeroporto         |        1 |
| 28 | 434.00 | 2014-04-01 | Rodeio interior de Sao Paulo |        0 |
| 29 | 115.90 | 2014-06-12 | Dia dos namorados            |        0 |
| 30 |  98.00 | 2014-10-12 | Dia das crian?ºas            |        0 |
| 31 | 253.70 | 2014-12-20 | Natal - presentes            |        0 |
| 32 | 370.15 | 2014-12-25 | Compras de natal             |        0 |
| 33 |  32.09 | 2015-07-02 | Lanchonete                   |        1 |
| 35 |  98.70 | 2015-02-07 | Lanchonete                   |        1 |
| 36 | 213.50 | 2015-09-25 | Roupas                       |        0 |
| 38 |  23.78 | 2015-12-18 | Lanchonete do Z?®            |        1 |
| 40 |  12.34 | 2015-07-19 | Canetas                      |        0 |
| 41 |  87.43 | 2015-05-10 | Gravata                      |        0 |
+----+--------+------------+------------------------------+----------+
22 rows in set (0.00 sec)

```

Todas compras mais caras e que não foram entregues ao mesmo tempo
```sql
MariaDB [ControleDeGastos]> SELECT * FROM compras WHERE valor > 1500 AND recebida = 0;
+----+---------+------------+-------------------+----------+
| id | valor   | data       | observacoes       | recebida |
+----+---------+------------+-------------------+----------+
|  2 | 3500.00 | 2012-05-21 | Televisao         |        0 |
| 22 | 1501.00 | 2014-06-22 | Presente da sogra |        0 |
| 23 | 1709.00 | 2014-08-25 | Parcela da casa   |        0 |
+----+---------+------------+-------------------+----------+
3 rows in set (0.00 sec)
```

Valores que sejam menores que 500 ou maiores que 1500
```sql
MariaDB [ControleDeGastos]> SELECT * FROM compras WHERE valor < 20 OR valor > 1500;
+----+----------+------------+------------------------+----------+
| id | valor    | data       | observacoes            | recebida |
+----+----------+------------+------------------------+----------+
|  2 |  3500.00 | 2012-05-21 | Televisao              |        0 |
|  3 |  1576.40 | 2012-04-30 | Material de construcao |        1 |
|  5 |  4780.00 | 2013-01-23 | Sala de estar          |        1 |
| 10 |    12.34 | 2013-05-23 | Lanchonete             |        0 |
| 12 |    12.39 | 2013-01-06 | Sorvete no parque      |        0 |
| 14 |  2498.00 | 2013-01-12 | Compras de janeiro     |        1 |
| 15 |  3212.40 | 2013-11-13 | Compras do mes         |        1 |
| 19 |    12.00 | 2014-02-19 | Salgado no aeroporto   |        1 |
| 21 | 10937.12 | 2014-04-30 | Carnaval em Cancun     |        1 |
| 22 |  1501.00 | 2014-06-22 | Presente da sogra      |        0 |
| 23 |  1709.00 | 2014-08-25 | Parcela da casa        |        0 |
| 40 |    12.34 | 2015-07-19 | Canetas                |        0 |
+----+----------+------------+------------------------+----------+
12 rows in set (0.00 sec)
```

Valor Específico
```sql
MariaDB [ControleDeGastos]> SELECT * FROM compras WHERE valor = 3500;
+----+---------+------------+-------------+----------+
| id | valor   | data       | observacoes | recebida |
+----+---------+------------+-------------+----------+
|  2 | 3500.00 | 2012-05-21 | Televisao   |        0 |
+----+---------+------------+-------------+----------+
1 row in set (0.00 sec)
```

Quais foram em Lanchonete:
```sql
MariaDB [ControleDeGastos]> SELECT * FROM compras WHERE observacoes = 'lanchonete';
+----+-------+------------+-------------+----------+
| id | valor | data       | observacoes | recebida |
+----+-------+------------+-------------+----------+
|  9 | 54.98 | 2013-04-12 | Lanchonete  |        0 |
| 10 | 12.34 | 2013-05-23 | Lanchonete  |        0 |
| 11 | 78.65 | 2013-12-04 | Lanchonete  |        0 |
| 33 | 32.09 | 2015-07-02 | Lanchonete  |        1 |
| 35 | 98.70 | 2015-02-07 | Lanchonete  |        1 |
+----+-------+------------+-------------+----------+
5 rows in set (0.00 sec)
```

Verificando se existe um pedaço do texto que queremos na coluna desejada:

```sql
MariaDB [ControleDeGastos]> SELECT * FROM compras WHERE observacoes LIKE 'parcela%';
+----+---------+------------+------------------+----------+
| id | valor   | data       | observacoes      | recebida |
+----+---------+------------+------------------+----------+
| 23 | 1709.00 | 2014-08-25 | Parcela da casa  |        0 |
| 24 |  567.09 | 2014-09-25 | Parcela do carro |        0 |
+----+---------+------------+------------------+----------+
2 rows in set (0.00 sec)
```

Saber todas as compras com observações em que o "de" estivesse no meio do texto
```sql
MariaDB [ControleDeGastos]> SELECT * FROM compras WHERE observacoes LIKE '%de%';
+----+---------+------------+------------------------------+----------+
| id | valor   | data       | observacoes                  | recebida |
+----+---------+------------+------------------------------+----------+
|  3 | 1576.40 | 2012-04-30 | Material de construcao       |        1 |
|  5 | 4780.00 | 2013-01-23 | Sala de estar                |        1 |
| 14 | 2498.00 | 2013-01-12 | Compras de janeiro           |        1 |
| 16 |  223.09 | 2013-12-17 | Compras de natal             |        1 |
| 28 |  434.00 | 2014-04-01 | Rodeio interior de Sao Paulo |        0 |
| 32 |  370.15 | 2014-12-25 | Compras de natal             |        0 |
+----+---------+------------+------------------------------+----------+
6 rows in set (0.00 sec)
```

### 2.14 EXERCÍCIOS

https://github.com/josemalcher/SQL-e-Modelagem-com-Banco-de-Dados-Livro-Alura/tree/master/2-ListaExercicios




[Voltar ao Índice](#indice)

---

## <a name="parte3">ATUALIZANDO E EXCLUINDO DADOS</a>

#### SQL, quando queremos filtrar um intervalo, podemos utilizar o operador BETWEEN:

```sql
SELECT valor,observacoes 
FROM compras 
WHERE valor >= 1000 AND valor <= 2000;
+---------+------------------------+
| valor   | observacoes            |
+---------+------------------------+
| 1576.40 | Material de construcao |
| 1203.00 | Quartos                |
| 1501.00 | Presente da sogra      |
| 1709.00 | Parcela da casa        |
| 1245.20 | Roupas                 |
+---------+------------------------+
5 rows in set (0.05 sec)

-- USANDO BETWEEN

SELECT valor, observacoes 
FROM compras 
WHERE valor BETWEEN 1000 AND 2000;
+---------+------------------------+
| valor   | observacoes            |
+---------+------------------------+
| 1576.40 | Material de construcao |
| 1203.00 | Quartos                |
| 1501.00 | Presente da sogra      |
| 1709.00 | Parcela da casa        |
| 1245.20 | Roupas                 |
+---------+------------------------+
5 rows in set (0.00 sec)

-- mais um filtro pegando o intervalo de 01/01/2013 e 31/12/2013

SELECT valor, observacoes 
FROM compras 
WHERE valor BETWEEN 1000 AND 2000 
AND data BETWEEN '2013-01-01' AND '2013-12-01';
+---------+-------------+
| valor   | observacoes |
+---------+-------------+
| 1203.00 | Quartos     |
+---------+-------------+
1 row in set (0.00 sec)

```

### 3.1 UTILIZANDO O UPDATE

```sql
 SELECT id,valor, observacoes FROM compras WHERE valor BETWEEN 1000 AND 2000 AND data BETWEEN '2013-01-01' AND '2013-12-01';
+----+---------+-------------+
| id | valor   | observacoes |
+----+---------+-------------+
|  7 | 1203.00 | Quartos     |
+----+---------+-------------+

UPDATE compras 
SET valor = 1500 
WHERE id = 7;


SELECT id,valor, observacoes FROM compras WHERE valor BETWEEN 1000 AND 2000 AND data BETWEEN '2013-01-01' AND '2013-12-01';
+----+---------+-------------+
| id | valor   | observacoes |
+----+---------+-------------+
|  7 | 1500.00 | Quartos     |
+----+---------+-------------+

UPDATE compras 
SET observacoes = 'Reforma de Quartos' 
WHERE id = 7;


SELECT id,valor, observacoes FROM compras WHERE valor BETWEEN 1000 AND 2000 AND data BETWEEN '2013-01-01' AND '2013-12-01';
+----+---------+--------------------+
| id | valor   | observacoes        |
+----+---------+--------------------+
|  7 | 1500.00 | Reforma de Quartos |
+----+---------+--------------------+

```

### 3.2 ATUALIZANDO VÁRIAS COLUNAS AO MESMO TEMPO

```sql
 UPDATE compras 
 SET valor = 1555 , observacoes = 'Reforma dos quartos CARA' 
 WHERE id = 7; 


MariaDB [ControleDeGastos]> SELECT id,valor, observacoes FROM compras WHERE valor BETWEEN 1000 AND 2000 AND data BETWEEN '2013-01-01' AND '2013-12-01';
+----+---------+--------------------------+
| id | valor   | observacoes              |
+----+---------+--------------------------+
|  7 | 1555.00 | Reforma dos quartos CARA |
+----+---------+--------------------------+
```

### 3.3 UTILIZANDO UMA COLUNA COMO REFERÊNCIA PARA OUTRA COLUNA

```sql
 SELECT id, valor FROM compras WHERE id > 7 AND id <= 10;
+----+--------+
| id | valor  |
+----+--------+
|  8 | 402.90 |
|  9 |  54.98 |
| 10 |  12.34 |
+----+--------+

UPDATE compras 
SET valor = valor * 1.1 
WHERE id >= 7 AND id <= 10;
Query OK, 4 rows affected, 2 warnings (0.14 sec)
Rows matched: 4  Changed: 4  Warnings: 2

MariaDB [ControleDeGastos]> SELECT id, valor FROM compras WHERE id > 7 AND id <= 10;
+----+--------+
| id | valor  |
+----+--------+
|  8 | 443.19 |
|  9 |  60.48 |
| 10 |  13.57 |
+----+--------+

```
Se eu tenho uma tabela de produtos com os campos precoLiquido eu posso atualizar o precoBruto quando o imposto mudar para 15%:
```sql
    UPDATE produtos SET precoBruto = precoLiquido * 1.15;
```

### 3.4 UTILIZANDO O DELETE
```sql
DELETE FROM compras WHERE id = 7;

SELECT id, valor FROM compras WHERE id = 7;
Empty set (0.00 sec)
```

### 3.5 CUIDADOS COM O DELETE E UPDATE
A boa prática para executar essas instruções é sempre escrever a instrução WHERE antes, ou seja, definir primeiro qual será a condição para executar essas instruções:

```sql
-- Então adicionamos o DELETE / UPDATE :
DELETE FROM compras WHERE id = 11;
UPDATE compras SET valor = 1500 WHERE id = 11;
```

### Lista 3 Exercício

https://github.com/josemalcher/SQL-e-Modelagem-com-Banco-de-Dados-Livro-Alura/tree/master/3-ListaExercicios


[Voltar ao Índice](#indice)

---

## <a name="parte4">ALTERANDO E RESTRINGINDO O FORMATO DE NOSSAS TABELAS</a>

```sql
DESC compras;
+-------------+---------------+------+-----+---------+----------------+
| Field       | Type          | Null | Key | Default | Extra          |
+-------------+---------------+------+-----+---------+----------------+
| id          | int(11)       | NO   | PRI | NULL    | auto_increment |
| valor       | decimal(18,2) | YES  |     | NULL    |                |
| data        | date          | YES  |     | NULL    |                |
| observacoes | varchar(255)  | YES  |     | NULL    |                |
| recebida    | tinyint(4)    | YES  |     | NULL    |                |
+-------------+---------------+------+-----+---------+----------------+
5 rows in set (0.03 sec)

 INSERT INTO compras (valor,data,recebdica,observacoes) 
 VALUES(120,'2016-01-01', 1, NULL);

SELECT * FROM compras WHERE data = '2016-01-01';
+----+--------+------------+-------------+----------+
| id | valor  | data       | observacoes | recebida |
+----+--------+------------+-------------+----------+
| 45 | 120.00 | 2016-01-01 | NULL        |        1 |
+----+--------+------------+-------------+----------+

```
### 4.1 RESTRINGINDO OS NULOS

Podemos criar restrições, Constraints, que tem a capacidade de determinar as regras que as colunas de nossas tabelas terão.

```sql
 SELECT * FROM compras WHERE observacoes IS NULL;
+----+--------+------------+-------------+----------+
| id | valor  | data       | observacoes | recebida |
+----+--------+------------+-------------+----------+
| 45 | 120.00 | 2016-01-01 | NULL        |        1 |
+----+--------+------------+-------------+----------+
1 row in set (0.00 sec)

-- excluir todas as compras que tenham as observações nulas

DELETE FROM compras WHERE observacoes IS NULL;
Query OK, 1 row affected (0.05 sec)

SELECT * FROM compras WHERE observacoes IS NULL;
Empty set (0.00 sec)
```

### 4.2 ADICIONANDO CONSTRAINTS

Definir Constraints no momento da criação da tabela:
```sql
CREATE TABLE compras(
    -> id INT AUTO_INCREMENT PRIMARY KEY,
    -> valor DECIMAL(18,2) NOT NULL,
    -> data DATE NOT NULL,
    -> observacoes VARCHAR(255) NOT NULL,
    -> recebida TINYINT) NOT NULL;
```

### Ajustando a tabela
```sql

MariaDB [controledegastos]> DESC compras;
+-------------+---------------+------+-----+---------+----------------+
| Field       | Type          | Null | Key | Default | Extra          |
+-------------+---------------+------+-----+---------+----------------+
| id          | int(11)       | NO   | PRI | NULL    | auto_increment |
| valor       | decimal(18,2) | YES  |     | NULL    |                |
| data        | date          | YES  |     | NULL    |                |
| observacoes | varchar(255)  | YES  |     | NULL    |                |
| recebida    | tinyint(4)    | YES  |     | NULL    |                |
+-------------+---------------+------+-----+---------+----------------+
5 rows in set (0.02 sec)

MariaDB [controledegastos]> ALTER TABLE compras MODIFY COLUMN observacoes VARCHAR(255) NOT NULL;
Query OK, 42 rows affected (0.16 sec)
Records: 42  Duplicates: 0  Warnings: 0

MariaDB [controledegastos]> DESC compras;
+-------------+---------------+------+-----+---------+----------------+
| Field       | Type          | Null | Key | Default | Extra          |
+-------------+---------------+------+-----+---------+----------------+
| id          | int(11)       | NO   | PRI | NULL    | auto_increment |
| valor       | decimal(18,2) | YES  |     | NULL    |                |
| data        | date          | YES  |     | NULL    |                |
| observacoes | varchar(255)  | NO   |     | NULL    |                |
| recebida    | tinyint(4)    | YES  |     | NULL    |                |
+-------------+---------------+------+-----+---------+----------------+
5 rows in set (0.01 sec)

INSERT INTO compras (valor,data,recebida,observacoes) VALUES(120,'2016-01-01', 1, NULL);
ERROR 1048 (23000): Column 'observacoes' cannot be null

```

### 4.3 VALORES DEFAULT

Adicionar valores padrões, no inglês Default, em uma coluna utilizando a instrução DEFAULT:
```sql
 ALTER TABLE compras MODIFY COLUMN recebida tinyint(1) DEFAULT 0;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

MariaDB [controledegastos]> DESC compras;
+-------------+---------------+------+-----+---------+----------------+
| Field       | Type          | Null | Key | Default | Extra          |
+-------------+---------------+------+-----+---------+----------------+
| id          | int(11)       | NO   | PRI | NULL    | auto_increment |
| valor       | decimal(18,2) | YES  |     | NULL    |                |
| data        | date          | YES  |     | NULL    |                |
| observacoes | varchar(255)  | NO   |     | NULL    |                |
| recebida    | tinyint(1)    | YES  |     | 0       |                |
+-------------+---------------+------+-----+---------+----------------+

 INSERT INTO compras(valor, data, observacoes) 
 VALUES(150,'2018-02-06','compras de teste');
Query OK, 1 row affected (0.38 sec)

MariaDB [controledegastos]> SELECT * FROM compras WHERE id > 40;
+----+--------+------------+------------------------+----------+
| id | valor  | data       | observacoes            | recebida |
+----+--------+------------+------------------------+----------+
| 41 |  87.43 | 2015-05-10 | Gravata                |        0 |
| 42 | 887.66 | 2015-02-02 | Presente para o filhao |        1 |
| 43 | 100.00 | 2015-09-02 | COMIDA                 |        1 |
| 46 | 150.00 | 2018-02-06 | compras de teste       |        0 |
+----+--------+------------+------------------------+----------+
4 rows in set (0.00 sec)

```

### Lista 4 Exercícios

https://github.com/josemalcher/SQL-e-Modelagem-com-Banco-de-Dados-Livro-Alura/tree/master/4-ListaExercicios

[Voltar ao Índice](#indice)

---

## <a name="parte5">AGRUPANDO DADOS E FAZENDO CONSULTAS MAIS INTELIGENTES</a>

```sql
SELECT SUM(valor) FROM compras;
+------------+
| SUM(valor) |
+------------+
|   42094.44 |
+------------+
1 row in set (0.00 sec)
```

Total de todas as compras recebidas:
```sql
 SELECT SUM(valor) FROM compras WHERE recebida = TRUE;
+------------+
| SUM(valor) |
+------------+
|   33841.59 |
+------------+
1 row in set (0.00 sec)

 SELECT SUM(valor) FROM compras WHERE recebida = FALSE;
+------------+
| SUM(valor) |
+------------+
|    8252.85 |
+------------+
1 row in set (0.00 sec)

 SELECT COUNT(*) FROM compras WHERE recebida = TRUE;
+----------+
| COUNT(*) |
+----------+
|       29 |
+----------+
1 row in set (0.00 sec)

SELECT recebida, SUM(valor) FROM compras GROUP BY recebida;
+----------+------------+
| recebida | SUM(valor) |
+----------+------------+
|        0 |    8252.85 |
|        1 |   33841.59 |
+----------+------------+
2 rows in set (0.00 sec)

SELECT recebida, SUM(valor) AS 'soma' FROM compras GROUP by recebida;
+----------+----------+
| recebida | soma     |
+----------+----------+
|        0 |  8252.85 |
|        1 | 33841.59 |
+----------+----------+
2 rows in set (0.00 sec)

-- aplicar filtros em queries que utilizam funções de agregação

 SELECT recebida, SUM(valor) AS soma 
 FROM compras 
 WHERE valor < 1000 
 GROUP by recebida;
+----------+---------+
| recebida | soma    |
+----------+---------+
|        0 | 1808.65 |
|        1 | 9366.67 |
+----------+---------+
2 rows in set (0.00 sec)

```

Suponhamos uma query mais robusta, onde podemos verificar em qual mês e ano a compra foi entregue ou não e o valor da soma. Podemos retornar a informação de ano utilizando a função YEAR() e a informação de mês utilizando a função MONTH():

```sql
 SELECT MONTH(data) AS mes, YEAR(data) AS ano, recebida, SUM(valor) AS soma 
 FROM compras 
 GROUP BY recebida, mes, ano;
+------+------+----------+----------+
| mes  | ano  | recebida | soma     |
+------+------+----------+----------+
|    1 | 2013 |        0 |     2.39 |
|    2 | 2018 |        0 |   150.00 |
|    4 | 2013 |        0 |    50.48 |
|    5 | 2012 |        0 |  3490.00 |
|    5 | 2013 |        0 |     3.57 |
|    5 | 2015 |        0 |    87.43 |
|    7 | 2015 |        0 |    12.34 |
|    8 | 2014 |        0 |  1709.00 |
|    9 | 2014 |        0 |   567.09 |
|    9 | 2015 |        0 |   213.50 |
|   10 | 2014 |        0 |    98.00 |
|   10 | 2015 |        0 |  1245.20 |
|   12 | 2014 |        0 |   623.85 |
|    1 | 2013 |        1 |  8016.90 |
|    1 | 2014 |        1 |   827.50 |
|    2 | 2012 |        1 |   190.00 |
|    2 | 2014 |        1 |   921.11 |
|    2 | 2015 |        1 |   986.36 |
|    3 | 2013 |        1 |   815.34 |
|    4 | 2012 |        1 |  1566.40 |
|    4 | 2014 |        1 | 12139.30 |
|    5 | 2014 |        1 |   678.43 |
|    6 | 2014 |        1 |  1616.90 |
|    7 | 2013 |        1 |    98.12 |
|    7 | 2015 |        1 |    32.09 |
|    9 | 2015 |        1 |   676.12 |
|   10 | 2014 |        1 |   631.53 |
|   11 | 2013 |        1 |  3212.40 |
|   11 | 2015 |        1 |   954.12 |
|   12 | 2012 |        1 |   153.45 |
|   12 | 2013 |        1 |   301.74 |
|   12 | 2015 |        1 |    23.78 |
+------+------+----------+----------+
32 rows in set (0.00 sec)
```

### 5.1 ORDENANDO OS RESULTADOS

```sql
 SELECT MONTH(data) AS mes, YEAR(data) AS ano, recebida, SUM(valor) AS soma FROM compras GROUP BY recebida, mes, ano ORDER BY mes;
+------+------+----------+----------+
| mes  | ano  | recebida | soma     |
+------+------+----------+----------+
|    1 | 2013 |        0 |     2.39 |
|    1 | 2013 |        1 |  8016.90 |
|    1 | 2014 |        1 |   827.50 |
|    2 | 2018 |        0 |   150.00 |
|    2 | 2012 |        1 |   190.00 |
|    2 | 2015 |        1 |   986.36 |
|    2 | 2014 |        1 |   921.11 |
|    3 | 2013 |        1 |   815.34 |
|    4 | 2013 |        0 |    50.48 |
|    4 | 2014 |        1 | 12139.30 |
|    4 | 2012 |        1 |  1566.40 |
|    5 | 2014 |        1 |   678.43 |
|    5 | 2015 |        0 |    87.43 |
|    5 | 2013 |        0 |     3.57 |
|    5 | 2012 |        0 |  3490.00 |
|    6 | 2014 |        1 |  1616.90 |
|    7 | 2015 |        1 |    32.09 |
|    7 | 2013 |        1 |    98.12 |
|    7 | 2015 |        0 |    12.34 |
|    8 | 2014 |        0 |  1709.00 |
|    9 | 2015 |        0 |   213.50 |
|    9 | 2014 |        0 |   567.09 |
|    9 | 2015 |        1 |   676.12 |
|   10 | 2015 |        0 |  1245.20 |
|   10 | 2014 |        1 |   631.53 |
|   10 | 2014 |        0 |    98.00 |
|   11 | 2015 |        1 |   954.12 |
|   11 | 2013 |        1 |  3212.40 |
|   12 | 2013 |        1 |   301.74 |
|   12 | 2012 |        1 |   153.45 |
|   12 | 2015 |        1 |    23.78 |
|   12 | 2014 |        0 |   623.85 |
+------+------+----------+----------+
32 rows in set (0.00 sec)

 SELECT MONTH(data) AS mes, YEAR(data) AS ano, recebida, SUM(valor) AS soma FROM compras GROUP BY recebida, mes, ano ORDER BY ano, mes;
+------+------+----------+----------+
| mes  | ano  | recebida | soma     |
+------+------+----------+----------+
|    2 | 2012 |        1 |   190.00 |
|    4 | 2012 |        1 |  1566.40 |
|    5 | 2012 |        0 |  3490.00 |
|   12 | 2012 |        1 |   153.45 |
|    1 | 2013 |        0 |     2.39 |
|    1 | 2013 |        1 |  8016.90 |
|    3 | 2013 |        1 |   815.34 |
|    4 | 2013 |        0 |    50.48 |
|    5 | 2013 |        0 |     3.57 |
|    7 | 2013 |        1 |    98.12 |
|   11 | 2013 |        1 |  3212.40 |
|   12 | 2013 |        1 |   301.74 |
|    1 | 2014 |        1 |   827.50 |
|    2 | 2014 |        1 |   921.11 |
|    4 | 2014 |        1 | 12139.30 |
|    5 | 2014 |        1 |   678.43 |
|    6 | 2014 |        1 |  1616.90 |
|    8 | 2014 |        0 |  1709.00 |
|    9 | 2014 |        0 |   567.09 |
|   10 | 2014 |        1 |   631.53 |
|   10 | 2014 |        0 |    98.00 |
|   12 | 2014 |        0 |   623.85 |
|    2 | 2015 |        1 |   986.36 |
|    5 | 2015 |        0 |    87.43 |
|    7 | 2015 |        1 |    32.09 |
|    7 | 2015 |        0 |    12.34 |
|    9 | 2015 |        0 |   213.50 |
|    9 | 2015 |        1 |   676.12 |
|   10 | 2015 |        0 |  1245.20 |
|   11 | 2015 |        1 |   954.12 |
|   12 | 2015 |        1 |    23.78 |
|    2 | 2018 |        0 |   150.00 |
+------+------+----------+----------+
32 rows in set (0.00 sec)

-- AVG() que retorna a média de uma coluna:

 SELECT MONTH(data) AS mes, YEAR(data) AS ano, recebida, AVG(valor) AS soma FROM compras GROUP BY recebida, mes, ano ORDER BY mes, ano;
+------+------+----------+-------------+
| mes  | ano  | recebida | soma        |
+------+------+----------+-------------+
|    1 | 2013 |        1 | 2672.300000 |
|    1 | 2013 |        0 |    2.390000 |
|    1 | 2014 |        1 |  827.500000 |
|    2 | 2012 |        1 |  190.000000 |
|    2 | 2014 |        1 |  460.555000 |
|    2 | 2015 |        1 |  493.180000 |
|    2 | 2018 |        0 |  150.000000 |
|    3 | 2013 |        1 |  407.670000 |
|    4 | 2012 |        1 | 1566.400000 |
|    4 | 2013 |        0 |   50.480000 |
|    4 | 2014 |        1 | 4046.433333 |
|    5 | 2012 |        0 | 3490.000000 |
|    5 | 2013 |        0 |    3.570000 |
|    5 | 2014 |        1 |  678.430000 |
|    5 | 2015 |        0 |   87.430000 |
|    6 | 2014 |        1 |  808.450000 |
|    7 | 2013 |        1 |   98.120000 |
|    7 | 2015 |        1 |   32.090000 |
|    7 | 2015 |        0 |   12.340000 |
|    8 | 2014 |        0 | 1709.000000 |
|    9 | 2014 |        0 |  567.090000 |
|    9 | 2015 |        1 |  338.060000 |
|    9 | 2015 |        0 |  213.500000 |
|   10 | 2014 |        1 |  631.530000 |
|   10 | 2014 |        0 |   98.000000 |
|   10 | 2015 |        0 | 1245.200000 |
|   11 | 2013 |        1 | 3212.400000 |
|   11 | 2015 |        1 |  954.120000 |
|   12 | 2012 |        1 |  153.450000 |
|   12 | 2013 |        1 |  150.870000 |
|   12 | 2014 |        0 |  311.925000 |
|   12 | 2015 |        1 |   23.780000 |
+------+------+----------+-------------+
32 rows in set (0.00 sec)

``` 

### Lista 5 Exercícios

https://github.com/josemalcher/SQL-e-Modelagem-com-Banco-de-Dados-Livro-Alura/tree/master/5-ListaExercicios



[Voltar ao Índice](#indice)

---

## <a name="parte6">JUNTANDO DADOS DE VÁRIAS TABELAS</a>

```sql
-- Adicionando uma coluna comprador
ALTER TABLE compras ADD COLUMN comprador VARCHAR(255);
Query OK, 0 rows affected (0.34 sec)
Records: 0  Duplicates: 0  Warnings: 0

DESC compras;
+-------------+--------------+------+-----+---------+----------------+
| Field       | Type         | Null | Key | Default | Extra          |
+-------------+--------------+------+-----+---------+----------------+
| id          | int(11)      | NO   | PRI | NULL    | auto_increment |
| valor       | varchar(255) | NO   |     | NULL    |                |
| data        | date         | YES  |     | NULL    |                |
| observacoes | varchar(255) | NO   |     | NULL    |                |
| recebida    | tinyint(1)   | YES  |     | 0       |                |
| comprador   | varchar(255) | YES  |     | NULL    |                |
+-------------+--------------+------+-----+---------+----------------+
6 rows in set (0.01 sec)
```

Adicionando compradores
```sql
UPDATE compras SET comprador = 'Jose Malcher Jr' WHERE id = 1;
Query OK, 1 row affected (0.11 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [controledegastos]> UPDATE compras SET comprador = 'Luiza Helena' WHERE id = 2;
Query OK, 1 row affected (0.07 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [controledegastos]> UPDATE compras SET comprador = 'Cintia Helena' WHERE id = 3;
Query OK, 1 row affected (0.02 sec)
Rows matched: 1  Changed: 1  Warnings: 0
--etc...
 SELECT * FROM compras LIMIT 13;
+----+---------+------------+------------------------+----------+-----------------+
| id | valor   | data       | observacoes            | recebida | comprador       |
+----+---------+------------+------------------------+----------+-----------------+
|  1 | 190.00  | 2012-02-19 | Material escolar       |        1 | Jose Malcher Jr |
|  2 | 3490.00 | 2012-05-21 | Televisao              |        0 | Luiza Helena    |
|  3 | 1566.40 | 2012-04-30 | Material de construcao |        1 | Cintia Helena   |
|  4 | 153.45  | 2012-12-15 | Pizza pra familia      |        1 | Ana Carolina    |
|  5 | 4770.00 | 2013-01-23 | Sala de estar          |        1 | Jose Malcher Jr |
|  6 | 382.15  | 2013-03-03 | Quartos                |        1 | NULL            |
|  8 | 433.19  | 2013-03-21 | Copa                   |        1 | NULL            |
|  9 | 50.48   | 2013-04-12 | Lanchonete             |        0 | NULL            |
| 10 | 3.57    | 2013-05-23 | Lanchonete             |        0 | Jose Malcher Jr |
| 11 | 78.65   | 2013-12-04 | entregue antes de 2014 |        1 | Jose Malcher Jr |
| 12 | 2.39    | 2013-01-06 | Sorvete no parque      |        0 | Ana Carolina    |
| 13 | 98.12   | 2013-07-09 | entregue antes de 2014 |        1 | Ana Carolina    |
| 14 | 2488.00 | 2013-01-12 | Compras de janeiro     |        1 | Luiza Helena    |
+----+---------+------------+------------------------+----------+-----------------+
13 rows in set (0.00 sec)

--ajuste
 UPDATE compras SET comprador = 'Ana Carolina' WHERE id = 14 AND comprador = 'Luiza Helena';
Query OK, 1 row affected (0.04 sec)
Rows matched: 1  Changed: 1  Warnings: 0

MariaDB [controledegastos]> SELECT * FROM compras LIMIT 13;
+----+---------+------------+------------------------+----------+-----------------+
| id | valor   | data       | observacoes            | recebida | comprador       |
+----+---------+------------+------------------------+----------+-----------------+
|  1 | 190.00  | 2012-02-19 | Material escolar       |        1 | Jose Malcher Jr |
|  2 | 3490.00 | 2012-05-21 | Televisao              |        0 | Luiza Helena    |
|  3 | 1566.40 | 2012-04-30 | Material de construcao |        1 | Cintia Helena   |
|  4 | 153.45  | 2012-12-15 | Pizza pra familia      |        1 | Ana Carolina    |
|  5 | 4770.00 | 2013-01-23 | Sala de estar          |        1 | Jose Malcher Jr |
|  6 | 382.15  | 2013-03-03 | Quartos                |        1 | NULL            |
|  8 | 433.19  | 2013-03-21 | Copa                   |        1 | NULL            |
|  9 | 50.48   | 2013-04-12 | Lanchonete             |        0 | NULL            |
| 10 | 3.57    | 2013-05-23 | Lanchonete             |        0 | Jose Malcher Jr |
| 11 | 78.65   | 2013-12-04 | entregue antes de 2014 |        1 | Jose Malcher Jr |
| 12 | 2.39    | 2013-01-06 | Sorvete no parque      |        0 | Ana Carolina    |
| 13 | 98.12   | 2013-07-09 | entregue antes de 2014 |        1 | Ana Carolina    |
| 14 | 2488.00 | 2013-01-12 | Compras de janeiro     |        1 | Ana Carolina    |
+----+---------+------------+------------------------+----------+-----------------+
13 rows in set (0.00 sec)

```

Adicionando coluna TELEFONE
```sql
ALTER TABLE compras ADD COLUMN telefone VARCHAR(30);
Query OK, 0 rows affected (0.19 sec)
Records: 0  Duplicates: 0  Warnings: 0

MariaDB [controledegastos]> DESC compras;
+-------------+--------------+------+-----+---------+----------------+
| Field       | Type         | Null | Key | Default | Extra          |
+-------------+--------------+------+-----+---------+----------------+
| id          | int(11)      | NO   | PRI | NULL    | auto_increment |
| valor       | varchar(255) | NO   |     | NULL    |                |
| data        | date         | YES  |     | NULL    |                |
| observacoes | varchar(255) | NO   |     | NULL    |                |
| recebida    | tinyint(1)   | YES  |     | 0       |                |
| comprador   | varchar(255) | YES  |     | NULL    |                |
| telefone    | varchar(30)  | YES  |     | NULL    |                |
+-------------+--------------+------+-----+---------+----------------+
7 rows in set (0.03 sec)
```

Adicionando Telefones:
```sql
 UPDATE compras SET telefone = '43 94444-0000' WHERE comprador = 'Ana Carolina';
Query OK, 4 rows affected (0.03 sec)
Rows matched: 4  Changed: 4  Warnings: 0

MariaDB [controledegastos]> SELECT * FROM compras LIMIT 13;
+----+---------+------------+------------------------+----------+-----------------+---------------+
| id | valor   | data       | observacoes            | recebida | comprador       | telefone      |
+----+---------+------------+------------------------+----------+-----------------+---------------+
|  1 | 190.00  | 2012-02-19 | Material escolar       |        1 | Jose Malcher Jr | 91 98080-0000 |
|  2 | 3490.00 | 2012-05-21 | Televisao              |        0 | Luiza Helena    | NULL          |
|  3 | 1566.40 | 2012-04-30 | Material de construcao |        1 | Cintia Helena   | NULL          |
|  4 | 153.45  | 2012-12-15 | Pizza pra familia      |        1 | Ana Carolina    | 43 94444-0000 |
|  5 | 4770.00 | 2013-01-23 | Sala de estar          |        1 | Jose Malcher Jr | 91 98080-0000 |
|  6 | 382.15  | 2013-03-03 | Quartos                |        1 | NULL            | NULL          |
|  8 | 433.19  | 2013-03-21 | Copa                   |        1 | NULL            | NULL          |
|  9 | 50.48   | 2013-04-12 | Lanchonete             |        0 | NULL            | NULL          |
| 10 | 3.57    | 2013-05-23 | Lanchonete             |        0 | Jose Malcher Jr | 91 98080-0000 |
| 11 | 78.65   | 2013-12-04 | entregue antes de 2014 |        1 | Jose Malcher Jr | 91 98080-0000 |
| 12 | 2.39    | 2013-01-06 | Sorvete no parque      |        0 | Ana Carolina    | 43 94444-0000 |
| 13 | 98.12   | 2013-07-09 | entregue antes de 2014 |        1 | Ana Carolina    | 43 94444-0000 |
| 14 | 2488.00 | 2013-01-12 | Compras de janeiro     |        1 | Ana Carolina    | 43 94444-0000 |
+----+---------+------------+------------------------+----------+-----------------+---------------+
13 rows in set (0.00 sec)
```

### 6.1 NORMALIZANDO NOSSO MODELO

Duas entidades, em uma unica tabela: a compra e o comprador. Quando nos deparamos com esses tipos de problemas criamos novas tabelas.

```sql
 CREATE TABLE compradores(id INT PRIMARY KEY AUTO_INCREMENT NOT NULL, 
                         nome VARCHAR(255), 
                         endereco VARCHAR(255), 
                         telefone VARCHAR(255));
Query OK, 0 rows affected (0.04 sec)

SHOW TABLES;
+----------------------------+
| Tables_in_controledegastos |
+----------------------------+
| compradores                |
| compras                    |
+----------------------------+
2 rows in set (0.00 sec)

DESC compradores;
+----------+--------------+------+-----+---------+----------------+
| Field    | Type         | Null | Key | Default | Extra          |
+----------+--------------+------+-----+---------+----------------+
| id       | int(11)      | NO   | PRI | NULL    | auto_increment |
| nome     | varchar(255) | YES  |     | NULL    |                |
| endereco | varchar(255) | YES  |     | NULL    |                |
| telefone | varchar(255) | YES  |     | NULL    |                |
+----------+--------------+------+-----+---------+----------------+
4 rows in set (0.01 sec)

```

Inserindo compradores:
```sql
INSERT INTO compradores(nome, endereco, telefone) VALUES('Jose Malcher Jr', 'Rua tal tal tal', '91 98080-00000');
Query OK, 1 row affected (0.02 sec)

INSERT INTO compradores(nome, endereco, telefone) VALUES('Ana Carolina', 'Rua FLoripa', '43 333320-00000');
Query OK, 1 row affected (0.02 sec)

INSERT INTO compradores(nome, endereco, telefone) VALUES('Luiza Helena', 'Rua Belém', '91 919920-00000');
Query OK, 1 row affected (0.02 sec)

MariaDB [controledegastos]> SELECT * FROM compradores;
+----+-----------------+-----------------+-----------------+
| id | nome            | endereco        | telefone        |
+----+-----------------+-----------------+-----------------+
|  1 | Jose Malcher Jr | Rua tal tal tal | 91 98080-00000  |
|  2 | Ana Carolina    | Rua FLoripa     | 43 333320-00000 |
|  3 | Luiza Helena    | Rua Belém       | 91 919920-00000 |
+----+-----------------+-----------------+-----------------+
3 rows in set (0.00 sec)
```

Alterando a tabela COMPRAS
```sql
DESC compras;
+-------------+--------------+------+-----+---------+----------------+
| Field       | Type         | Null | Key | Default | Extra          |
+-------------+--------------+------+-----+---------+----------------+
| id          | int(11)      | NO   | PRI | NULL    | auto_increment |
| valor       | varchar(255) | NO   |     | NULL    |                |
| data        | date         | YES  |     | NULL    |                |
| observacoes | varchar(255) | NO   |     | NULL    |                |
| recebida    | tinyint(1)   | YES  |     | 0       |                |
| comprador   | varchar(255) | YES  |     | NULL    |                |
| telefone    | varchar(30)  | YES  |     | NULL    |                |
+-------------+--------------+------+-----+---------+----------------+
7 rows in set (0.01 sec)

ALTER TABLE compras DROP COLUMN comprador;
Query OK, 0 rows affected (0.16 sec)
Records: 0  Duplicates: 0  Warnings: 0

ALTER TABLE compras DROP COLUMN telefone;
Query OK, 0 rows affected (0.06 sec)
Records: 0  Duplicates: 0  Warnings: 0

DESC compras;
+-------------+--------------+------+-----+---------+----------------+
| Field       | Type         | Null | Key | Default | Extra          |
+-------------+--------------+------+-----+---------+----------------+
| id          | int(11)      | NO   | PRI | NULL    | auto_increment |
| valor       | varchar(255) | NO   |     | NULL    |                |
| data        | date         | YES  |     | NULL    |                |
| observacoes | varchar(255) | NO   |     | NULL    |                |
| recebida    | tinyint(1)   | YES  |     | 0       |                |
+-------------+--------------+------+-----+---------+----------------+
5 rows in set (0.02 sec)

ALTER TABLE compras ADD COLUMN id_comprador int;
Query OK, 0 rows affected (0.10 sec)
Records: 0  Duplicates: 0  Warnings: 0

DESC compras;
+--------------+--------------+------+-----+---------+----------------+
| Field        | Type         | Null | Key | Default | Extra          |
+--------------+--------------+------+-----+---------+----------------+
| id           | int(11)      | NO   | PRI | NULL    | auto_increment |
| valor        | varchar(255) | NO   |     | NULL    |                |
| data         | date         | YES  |     | NULL    |                |
| observacoes  | varchar(255) | NO   |     | NULL    |                |
| recebida     | tinyint(1)   | YES  |     | 0       |                |
| id_comprador | int(11)      | YES  |     | NULL    |                |
+--------------+--------------+------+-----+---------+----------------+
6 rows in set (0.01 sec)

```

Adicionando os id compradores em compras:
```sql
 SELECT count(*) as 'Total de compras' FROM compras;
+------------------+
| Total de compras |
+------------------+
|               43 |
+------------------+
1 row in set (0.00 sec)

-- ETC....
UPDATE compras SET id_comprador = 3 WHERE valor >= 80.0 AND valor <= 120.0 ;


```

### 6.2 ONE TO MANY/MANY TO ONE
O que fizemos foi deixar claro que um comprador pode ter muitas compras (um para muitos), ou ainda que muitas compras podem vir do mesmo comprador (many to one), só depende do ponto de vista. Chamamos então de uma relação One to Many (ou Many to One).


### 6.3 FOREIGN KEY

```sql
 SELECT * FROM compras JOIN compradores ON compras.id_comprador = compradores.id;


-- sem JOIN 
 SELECT * FROM compras, compradores WHERE compras.id_comprador = compradores.id;


 ```
A instrução JOIN espera uma tabela que precisa ser juntada: FROM compras JOIN compradores. Nesse caso estamos juntando a tabela compras com a tabela compradores. Para passarmos o critério de junção, utilizamos a instrução ON : ON compras.id_compradores = compradores.id . Nesse momento estamos informando a FOREIGN KEY da tabela compras (compras.id_compradores) e qual é a chave (compradores.id) da tabela compradores que referencia essa FOREIGN KEY.

#### Quando adicionamos uma FOREIGN KEY em uma tabela, estamos adicionando uma Constraints:
```sql

ALTER TABLE compras ADD CONSTRAINT fk_compradores 
FOREIGN KEY (id_comprador) REFERENCES compradores(id);
Query OK, 43 rows affected (0.15 sec)
Records: 43  Duplicates: 0  Warnings: 0

-- Agora ao tentar adicionar uma ID inexistente na outra tabela: ERRO

INSERT INTO compras(valor,data,observacoes, id_comprador) VALUES (2000,'2018-02-02', 'Play 4', 100);
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`controledegastos`.`compras`, CONSTRAINT `fk_compradores` FOREIGN KEY (`id_comprador`) REFERENCES `compradores` (`id`))
MariaDB [controledegastos]>

```

### 6.4 DETERMINANDO VALORES FIXOS NA TABELA

```sql
ALTER TABLE compras ADD COLUMN forma_pagamento ENUM('BOLETO','CREDITO');
Query OK, 0 rows affected (0.12 sec)
Records: 0  Duplicates: 0  Warnings: 0

MariaDB [controledegastos]> DESC compras;
+-----------------+--------------------------+------+-----+---------+----------------+
| Field           | Type                     | Null | Key | Default | Extra          |
+-----------------+--------------------------+------+-----+---------+----------------+
| id              | int(11)                  | NO   | PRI | NULL    | auto_increment |
| valor           | varchar(255)             | NO   |     | NULL    |                |
| data            | date                     | YES  |     | NULL    |                |
| observacoes     | varchar(255)             | NO   |     | NULL    |                |
| recebida        | tinyint(1)               | YES  |     | 0       |                |
| id_comprador    | int(11)                  | YES  | MUL | NULL    |                |
| forma_pagamento | enum('BOLETO','CREDITO') | YES  |     | NULL    |                |
+-----------------+--------------------------+------+-----+---------+----------------+
7 rows in set (0.01 sec)

-- ENUM que permite que informemos quais serão os dados que ele pode aceitar:

INSERT INTO compras (valor, data, observacoes, id_comprador, forma_pagamento) 
VALUES (400, '2016-01-06', 'SSD 128GB', 1, 'BOLETO');
Query OK, 1 row affected (0.01 sec)

MariaDB [controledegastos]> SELECT * FROM compras WHERE id > 45;
+----+--------+------------+------------------+----------+--------------+-----------------+
| id | valor  | data       | observacoes      | recebida | id_comprador | forma_pagamento |
+----+--------+------------+------------------+----------+--------------+-----------------+
| 46 | 150.00 | 2018-02-06 | compras de teste |        0 |            2 | NULL            |
| 48 | 400    | 2016-01-06 | SSD 128GB        |        0 |            1 | BOLETO          |
+----+--------+------------+------------------+----------+--------------+-----------------+
2 rows in set (0.00 sec)

INSERT INTO compras (valor, data, observacoes, id_comprador, forma_pagamento) 
VALUES (1400, '2018-01-06', 'CELULAR', 1, 'DINHEIRO');
Query OK, 1 row affected, 1 warning (0.00 sec)

MariaDB [controledegastos]> SELECT * FROM compras WHERE id > 45;
+----+--------+------------+------------------+----------+--------------+-----------------+
| id | valor  | data       | observacoes      | recebida | id_comprador | forma_pagamento |
+----+--------+------------+------------------+----------+--------------+-----------------+
| 46 | 150.00 | 2018-02-06 | compras de teste |        0 |            2 | NULL            |
| 48 | 400    | 2016-01-06 | SSD 128GB        |        0 |            1 | BOLETO          |
| 49 | 1400   | 2018-01-06 | CELULAR          |        0 |            1 |                 |
+----+--------+------------+------------------+----------+--------------+-----------------+
3 rows in set (0.00 sec)


```

### 6.5 SERVER SQL MODES

https://dev.mysql.com/doc/refman/5.7/en/sql-mode.html

O MySQL impediu que fosse adicionado um valor diferente de "BOLETO" ou "CREDITO", mas o que realmente precisamos é que ele simplesmente não deixe adicionar uma compra que não tenha pelo menos uma dessas formas de pagamento. Além das configurações das tabelas, podemos também configurar o próprio servidor do MySQL. O servidor do MySQL opera em diferentes SQL modes e dentre esses modos, existe o strict mode que tem a finalidade de tratar valores inválidos que configuramos em nossas tabelas para instruções de INSERT e UPDATE , como por exemplo, o nosso ENUM . Para habilitar o strict mode precisamos alterar o SQL mode da nossa sessão. Nesse caso usaremos o modo "STRICT_ALL_TABLES":

```sql

-- Apenas de sessão
SET SESSION sql_mode = 'STRICT_ALL_TABLES';
Query OK, 0 rows affected (0.01 sec)

-- verificar 
 
 SELECT @@SESSION.sql_mode;
+--------------------+
| @@SESSION.sql_mode |
+--------------------+
| STRICT_ALL_TABLES  |
+--------------------+
1 row in set (0.00 sec)

-- configuramos o SQL mode do MySQL para impedir a inserção de valores inválidos. (apagamos o ultimo valor inválido)

-- Tentando inserir dados inválidos novamente!

INSERT INTO compras (valor, data, observacoes, id_comprador, forma_pagamento) 
VALUES (1400, '2018-01-06', 'CELULAR', 1, 'DINHEIRO');
ERROR 1265 (01000): Data truncated for column 'forma_pagamento' at row 1


-- fazendo a configuração global do SQL mode

SET GLOBAL sql_mode = 'STRICT_ALL_TABLES';
Query OK, 0 rows affected (0.00 sec)

SELECT @@GLOBAL.sql_mode;
+-------------------+
| @@GLOBAL.sql_mode |
+-------------------+
| STRICT_ALL_TABLES |
+-------------------+
1 row in set (0.00 sec)

```

OBS.: O ENUM é uma boa solução quando queremos restringir valores específicos e já esperados em um coluna, porém não faz parte do padrão ANSI que é o padrão para a escrita de instruções SQL , ou seja, é um recurso exclusivo do MySQL e cada banco de dados possui a sua própria implementação para essa mesma funcionadalidade.

### Lista 6 Exercícios

https://github.com/josemalcher/SQL-e-Modelagem-com-Banco-de-Dados-Livro-Alura/tree/master/6-ListaExercicios


[Voltar ao Índice](#indice)

---

## <a name="parte7">ALUNOS SEM MATRÍCULA E O EXISTS</a>

SQL -> https://github.com/josemalcher/SQL-e-Modelagem-com-Banco-de-Dados-Livro-Alura/tree/master/MaterialDoLivro

```sql

-- Recomendo importanção via Ferramenta MySQL Workbench !!

C:\Users\josemalcher\Documents\01-SERVs\xampp_php7.2.1\mysql\bin
λ mysql -uroot -p escola < escola.sql

MariaDB [escola]> SHOW TABLES;
+------------------+
| Tables_in_escola |
+------------------+
| aluno            |
| curso            |
| exercicio        |
| matricula        |
| nota             |
| resposta         |
| secao            |
+------------------+
7 rows in set (0.00 sec)

```

Verificando as informações das tabelas:

```sql
 DESC aluno;
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
| nome  | varchar(255) | NO   |     |         |                |
| email | varchar(255) | NO   |     |         |                |
+-------+--------------+------+-----+---------+----------------+
3 rows in set (0.08 sec)

DESC curso;
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
| nome  | varchar(255) | NO   |     |         |                |
+-------+--------------+------+-----+---------+----------------+
2 rows in set (0.02 sec)

DESC matricula;
+----------+-------------+------+-----+---------+----------------+
| Field    | Type        | Null | Key | Default | Extra          |
+----------+-------------+------+-----+---------+----------------+
| id       | int(11)     | NO   | PRI | NULL    | auto_increment |
| aluno_id | int(11)     | NO   |     | NULL    |                |
| curso_id | int(11)     | NO   |     | NULL    |                |
| data     | datetime    | NO   |     | NULL    |                |
| tipo     | varchar(20) | NO   |     |         |                |
+----------+-------------+------+-----+---------+----------------+
5 rows in set (0.01 sec)
```

```sql
 SELECT a.nome, c.nome FROM aluno a, matricula m, curso c 
 WHERE a.id = m.aluno_id 
   AND c.id = m.curso_id;
+-----------------+------------------------------------+
| nome            | nome                               |
+-----------------+------------------------------------+
| João da Silva   | SQL e banco de dados               |
| Frederico José  | SQL e banco de dados               |
| Alberto Santos  | Scrum e métodos ágeis              |
| Renata Alonso   | C# e orientação a objetos          |
| Paulo José      | SQL e banco de dados               |
| Manoel Santos   | Scrum e métodos ágeis              |
| Renata Ferreira | Desenvolvimento web com VRaptor    |
| Paula Soares    | Desenvolvimento mobile com Android |
| Renata Alonso   | Desenvolvimento mobile com Android |
| Manoel Santos   | SQL e banco de dados               |
| João da Silva   | C# e orientação a objetos          |
| Frederico José  | C# e orientação a objetos          |
| Alberto Santos  | C# e orientação a objetos          |
| Frederico José  | Desenvolvimento web com VRaptor    |
+-----------------+------------------------------------+
14 rows in set (0.00 sec)

SELECT a.nome, c.nome 
FROM aluno a 
JOIN matricula m ON m.aluno_id = a.id 
JOIN     curso c ON m.curso_id = c.id;

+-----------------+------------------------------------+
| nome            | nome                               |
+-----------------+------------------------------------+
| João da Silva   | SQL e banco de dados               |
| Frederico José  | SQL e banco de dados               |
| Alberto Santos  | Scrum e métodos ágeis              |
| Renata Alonso   | C# e orientação a objetos          |
| Paulo José      | SQL e banco de dados               |
| Manoel Santos   | Scrum e métodos ágeis              |
| Renata Ferreira | Desenvolvimento web com VRaptor    |
| Paula Soares    | Desenvolvimento mobile com Android |
| Renata Alonso   | Desenvolvimento mobile com Android |
| Manoel Santos   | SQL e banco de dados               |
| João da Silva   | C# e orientação a objetos          |
| Frederico José  | C# e orientação a objetos          |
| Alberto Santos  | C# e orientação a objetos          |
| Frederico José  | Desenvolvimento web com VRaptor    |
+-----------------+------------------------------------+
14 rows in set (0.00 sec)

```

### 7.1 SUBQUERIES

A função EXISTS() para verificar se existe algum registro de acordo com uma determinada query:

```sql
> SELECT a.nome FROM aluno a WHERE EXISTS(SELECT m.id FROM matricula m WHERE m.aluno_id = a.id);
+-----------------+
| nome            |
+-----------------+
| João da Silva   |
| Frederico José  |
| Alberto Santos  |
| Renata Alonso   |
| Paulo José      |
| Manoel Santos   |
| Renata Ferreira |
| Paula Soares    |
+-----------------+
8 rows in set (0.00 sec)

```
Quando utilizamos o EXISTS() indicamos que queremos o retorno de todos os alunos nomes dos alunos ( a.nome ) que estão na tabela aluno , porém, queremos apenas se existir uma matrícula para esse aluno EXISTS(SELECT m.id FROM matricula m WHERE m.aluno_id = a.id).

Retornando os alunos NÃO matriculados
```sql
SELECT a.nome FROM aluno a WHERE NOT EXISTS(SELECT m.id FROM matricula m WHERE m.aluno_id = a.id);
+------------------+
| nome             |
+------------------+
| Paulo da Silva   |
| Carlos Cunha     |
| Jose da Silva    |
| Danilo Cunha     |
| Zilmira José     |
| Cristaldo Santos |
| Osmir Ferreira   |
| Claudio Soares   |
+------------------+
8 rows in set (0.00 sec)
```

Pegar todos os exercícios que não foram respondidos utilizando novamente o NOT EXISTS: 

```sql
DESC exercicio;
+------------------+--------------+------+-----+---------+----------------+
| Field            | Type         | Null | Key | Default | Extra          |
+------------------+--------------+------+-----+---------+----------------+
| id               | int(11)      | NO   | PRI | NULL    | auto_increment |
| secao_id         | int(11)      | NO   |     | NULL    |                |
| pergunta         | varchar(255) | NO   |     | NULL    |                |
| resposta_oficial | varchar(255) | NO   |     | NULL    |                |
+------------------+--------------+------+-----+---------+----------------+
4 rows in set (0.02 sec)

DESC resposta;
+---------------+--------------+------+-----+---------+----------------+
| Field         | Type         | Null | Key | Default | Extra          |
+---------------+--------------+------+-----+---------+----------------+
| id            | int(11)      | NO   | PRI | NULL    | auto_increment |
| exercicio_id  | int(11)      | YES  |     | NULL    |                |
| aluno_id      | int(11)      | YES  |     | NULL    |                |
| resposta_dada | varchar(255) | YES  |     | NULL    |                |
+---------------+--------------+------+-----+---------+----------------+
4 rows in set (0.04 sec)

SELECT r.id FROM resposta r, exercicio e WHERE r.exercicio_id = e.id;
+----+
| id |
+----+
|  1 |
|  2 |
|  3 |
|  4 |
|  5 |
|  6 |
|  7 |
|  8 |
|  9 |
| 10 |
| 11 |
| 12 |
| 13 |
| 14 |
| 15 |
| 16 |
| 17 |
| 18 |
| 19 |
| 20 |
| 21 |
| 22 |
| 23 |
| 24 |
| 25 |
| 26 |
| 27 |
+----+
27 rows in set (0.00 sec)

MariaDB [escola]> SELECT * FROM exercicio e WHERE NOT EXISTS(SELECT r.id FROM resposta r WHERE r.exercicio_id = e.id);
+----+----------+------------------------------+------------------------------------------------------+
| id | secao_id | pergunta                     | resposta_oficial                                     |
+----+----------+------------------------------+------------------------------------------------------+
|  8 |        4 | como funciona?               | insert into (coluna1, coluna2) values (v1, v2)       |
|  9 |        5 | Como funciona a web?         | requisicao e resposta                                |
| 10 |        5 | Que linguagens posso ajudar? | varias, java, php, c#, etc                           |
| 11 |        6 | O que eh MVC?                | model view controller                                |
| 12 |        6 | Frameworks que usam?         | vraptor, spring mvc, struts, etc                     |
| 14 |        8 | O que é um interceptor?      | eh como se fosse um filtro que eh executado antes    |
| 15 |        8 | quando usar?                 | tratamento de excecoes, conexao com o banco de dados |
+----+----------+------------------------------+------------------------------------------------------+
7 rows in set (0.00 sec)
```

retornar todos os cursos que não possuem matrícula:

```sql
 SELECT c.nome FROM curso c WHERE NOT EXISTS(SELECT m.id FROM matricula m WHERE m.curso_id = c.id);
+--------------------------------+
| nome                           |
+--------------------------------+
| Java e orientação a objetos    |
| Desenvolvimento mobile com iOS |
| Ruby on Rails                  |
| PHP e MySql                    |
+--------------------------------+
4 rows in set (0.00 sec)
```

### Exercício

https://github.com/josemalcher/SQL-e-Modelagem-com-Banco-de-Dados-Livro-Alura/tree/master/7-ListaExercicios


[Voltar ao Índice](#indice)

---

## <a name="parte8">AGRUPANDO DADOS COM GROUP BY</a>

A instuição solicitou a média de todos os cursos para fazer uma comparação de notas para verificar se
todos os cursos possuem a mesma média, quais cursos tem menores notas e quais possuem as maiores
notas. Vamos verificar a estrutura de algumas tabelas da nossa base de dados, começaremos pela tabela
curso:

```sql
MariaDB [escola]> desc curso;
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
| nome  | varchar(255) | NO   |     |         |                |
+-------+--------------+------+-----+---------+----------------+
2 rows in set (0.01 sec)

```

```sql
MariaDB [escola]> desc secao;
+------------+--------------+------+-----+---------+----------------+
| Field      | Type         | Null | Key | Default | Extra          |
+------------+--------------+------+-----+---------+----------------+
| id         | int(11)      | NO   | PRI | NULL    | auto_increment |
| curso_id   | int(11)      | NO   |     | NULL    |                |
| titulo     | varchar(255) | NO   |     |         |                |
| explicacao | varchar(255) | NO   |     | NULL    |                |
| numero     | int(11)      | NO   |     | NULL    |                |
+------------+--------------+------+-----+---------+----------------+
5 rows in set (0.04 sec)

```

```sql
MariaDB [escola]> desc exercicio;
+------------------+--------------+------+-----+---------+----------------+
| Field            | Type         | Null | Key | Default | Extra          |
+------------------+--------------+------+-----+---------+----------------+
| id               | int(11)      | NO   | PRI | NULL    | auto_increment |
| secao_id         | int(11)      | NO   |     | NULL    |                |
| pergunta         | varchar(255) | NO   |     | NULL    |                |
| resposta_oficial | varchar(255) | NO   |     | NULL    |                |
+------------------+--------------+------+-----+---------+----------------+
4 rows in set (0.11 sec)
```

```sql
MariaDB [escola]> desc resposta;
+---------------+--------------+------+-----+---------+----------------+
| Field         | Type         | Null | Key | Default | Extra          |
+---------------+--------------+------+-----+---------+----------------+
| id            | int(11)      | NO   | PRI | NULL    | auto_increment |
| exercicio_id  | int(11)      | YES  |     | NULL    |                |
| aluno_id      | int(11)      | YES  |     | NULL    |                |
| resposta_dada | varchar(255) | YES  |     | NULL    |                |
+---------------+--------------+------+-----+---------+----------------+
4 rows in set (0.10 sec)

```

```sql
MariaDB [escola]> desc nota;
+-------------+---------------+------+-----+---------+----------------+
| Field       | Type          | Null | Key | Default | Extra          |
+-------------+---------------+------+-----+---------+----------------+
| id          | int(11)       | NO   | PRI | NULL    | auto_increment |
| resposta_id | int(11)       | YES  |     | NULL    |                |
| nota        | decimal(18,2) | YES  |     | NULL    |                |
+-------------+---------------+------+-----+---------+----------------+
3 rows in set (0.29 sec)

```

```sql
MariaDB [escola]> SELECT n.nota FROM nota n;
+-------+
| nota  |
+-------+
|  8.00 |
|  0.00 |
|  7.00 |
|  6.00 |
|  9.00 |
| 10.00 |
|  4.00 |
|  4.00 |
|  7.00 |
|  8.00 |
|  6.00 |
|  7.00 |
|  4.00 |
|  9.00 |
|  3.00 |
|  5.00 |
|  5.00 |
|  5.00 |
|  6.00 |
|  8.00 |
|  8.00 |
|  9.00 |
| 10.00 |
|  2.00 |
|  0.00 |
|  1.00 |
|  4.00 |
+-------+
27 rows in set (0.02 sec)

```

```sql
MariaDB [escola]> SELECT n.nota FROM nota n
    -> JOIN resposta r ON n.resposta_id = r.id;
+-------+
| nota  |
+-------+
|  8.00 |
|  0.00 |
|  7.00 |
|  6.00 |
|  9.00 |
| 10.00 |
|  4.00 |
|  4.00 |
|  7.00 |
|  8.00 |
|  6.00 |
|  7.00 |
|  4.00 |
|  9.00 |
|  3.00 |
|  5.00 |
|  5.00 |
|  5.00 |
|  6.00 |
|  8.00 |
|  8.00 |
|  9.00 |
| 10.00 |
|  2.00 |
|  0.00 |
|  1.00 |
|  4.00 |
+-------+
27 rows in set (0.03 sec)

```

```sql
MariaDB [escola]> SELECT n.nota FROM nota n
    -> JOIN resposta r ON n.resposta_id = r.id
    -> JOIN exercicio e ON r.exercicio_id = e.id;
+-------+
| nota  |
+-------+
|  8.00 |
|  0.00 |
|  7.00 |
|  6.00 |
|  9.00 |
| 10.00 |
|  4.00 |
|  4.00 |
|  7.00 |
|  8.00 |
|  6.00 |
|  7.00 |
|  4.00 |
|  9.00 |
|  3.00 |
|  5.00 |
|  5.00 |
|  5.00 |
|  6.00 |
|  8.00 |
|  8.00 |
|  9.00 |
| 10.00 |
|  2.00 |
|  0.00 |
|  1.00 |
|  4.00 |
+-------+
27 rows in set (0.00 sec)
```

```sql
MariaDB [escola]> SELECT n.nota
    -> FROM nota n
    -> JOIN resposta r ON n.resposta_id = r.id
    -> JOIN exercicio e ON r.exercicio_id = e.id
    -> JOIN secao s ON e.secao_id = s.id;
+-------+
| nota  |
+-------+
|  8.00 |
|  0.00 |
|  7.00 |
|  6.00 |
|  9.00 |
| 10.00 |
|  4.00 |
|  4.00 |
|  7.00 |
|  8.00 |
|  6.00 |
|  7.00 |
|  4.00 |
|  9.00 |
|  3.00 |
|  5.00 |
|  5.00 |
|  5.00 |
|  6.00 |
|  8.00 |
|  8.00 |
|  9.00 |
| 10.00 |
|  2.00 |
|  0.00 |
|  1.00 |
|  4.00 |
+-------+
27 rows in set (0.00 sec)

```

```sql
MariaDB [escola]> SELECT n.nota
    ->FROM nota n
    ->JOIN resposta r ON n.resposta_id = r.id
    ->JOIN exercicio e ON r.exercicio_id = e.id
    ->JOIN secao s ON e.secao_id = s.id
    ->JOIN curso c ON s.curso_id = c.id;
+-------+
| nota  |
+-------+
|  8.00 |
|  0.00 |
|  7.00 |
|  6.00 |
|  9.00 |
| 10.00 |
|  4.00 |
|  4.00 |
|  7.00 |
|  8.00 |
|  6.00 |
|  7.00 |
|  4.00 |
|  9.00 |
|  3.00 |
|  5.00 |
|  5.00 |
|  5.00 |
|  6.00 |
|  8.00 |
|  8.00 |
|  9.00 |
| 10.00 |
|  2.00 |
|  0.00 |
|  1.00 |
|  4.00 |
+-------+
27 rows in set (0.00 sec)

```

```sql
MariaDB [escola]> SELECT AVG(n.nota)
    -> FROM nota n
    ->        JOIN resposta r ON n.resposta_id = r.id
    ->        JOIN exercicio e ON r.exercicio_id = e.id
    ->        JOIN secao s ON e.secao_id = s.id
    ->        JOIN curso c ON s.curso_id = c.id;
+-------------+
| AVG(n.nota) |
+-------------+
|    5.740741 |
+-------------+
1 row in set (0.00 sec)

```

```sql
MariaDB [escola]> SELECT c.nome,
    ->        AVG(n.nota)
    -> FROM nota n
    ->        JOIN resposta r ON n.resposta_id = r.id
    ->        JOIN exercicio e ON r.exercicio_id = e.id
    ->        JOIN secao s ON e.secao_id = s.id
    ->        JOIN curso c ON s.curso_id = c.id;
+----------------------+-------------+
| nome                 | AVG(n.nota) |
+----------------------+-------------+
| SQL e banco de dados |    5.740741 |
+----------------------+-------------+
1 row in set (0.00 sec)
```

```sql
MariaDB [escola]> SELECT c.nome,
    ->        AVG(n.nota)
    -> FROM nota n
    ->        JOIN resposta r ON n.resposta_id = r.id
    ->        JOIN exercicio e ON r.exercicio_id = e.id
    ->        JOIN secao s ON e.secao_id = s.id
    ->        JOIN curso c ON s.curso_id = c.id
    -> GROUP BY c.nome;
+---------------------------------+-------------+
| nome                            | AVG(n.nota) |
+---------------------------------+-------------+
| C# e orientação a objetos       |    4.857143 |
| Desenvolvimento web com VRaptor |    8.000000 |
| Scrum e métodos ágeis           |    5.777778 |
| SQL e banco de dados            |    6.100000 |
+---------------------------------+-------------+
4 rows in set (0.00 sec)

```

```sql
MariaDB [escola]> SELECT COUNT(*) FROM exercicio;
+----------+
| COUNT(*) |
+----------+
|       31 |
+----------+
1 row in set (0.00 sec)
```

```sql
MariaDB [escola]> SELECT c.nome, COUNT(*) AS contagem FROM exercicio e
    -> JOIN secao s ON e.secao_id = s.id
    -> JOIN curso c ON s.curso_id = c.id
    -> GROUP BY c.nome;
+---------------------------------+----------+
| nome                            | contagem |
+---------------------------------+----------+
| C# e orientação a objetos       |        7 |
| Desenvolvimento web com VRaptor |        7 |
| Scrum e métodos ágeis           |        9 |
| SQL e banco de dados            |        8 |
+---------------------------------+----------+
4 rows in set (0.00 sec)

```

Todo final de semestre nós precisamos enviar um relatório para o MEC informando quantos alunos
estão matriculados em cada curso da instituição. Faremos novamente a nossa query por partes, vamos
retornar primeiro todos os cursos:

```sql
MariaDB [escola]> SELECT c.nome, COUNT(a.id) AS quantidade
    -> FROM curso c
    -> JOIN matricula m ON m.curso_id = c.id
    -> JOIN aluno a ON m.aluno_id = a.id
    -> GROUP BY c.nome;
+------------------------------------+------------+
| nome                               | quantidade |
+------------------------------------+------------+
| C# e orientação a objetos          |          4 |
| Desenvolvimento mobile com Android |          2 |
| Desenvolvimento web com VRaptor    |          2 |
| Scrum e métodos ágeis              |          2 |
| SQL e banco de dados               |          4 |
+------------------------------------+------------+
5 rows in set (0.00 sec)
```

Exercício 08

- https://github.com/josemalcher/SQL-e-Modelagem-com-Banco-de-Dados-Livro-Alura/tree/master/8-ListaExercicios 



[Voltar ao Índice](#indice)

---

## <a name="parte9">FILTRANDO AGREGAÇÕES E O HAVING</a>

Todo o fim de semestre, a instituição de ensino precisa montar os boletins dos alunos. Então vamos
montar a query que retornará todas as informações para montar o boletim. Começaremos retornando
todas as notas dos alunos:

```sql
MariaDB [escola]> SELECT n.nota FROM nota n
    -> JOIN resposta r ON r.id = n.resposta_id
    -> JOIN exercicio e ON e.id = r.exercicio_id
    -> JOIN secao s ON s.id = e.secao_id
    -> JOIN curso c ON c.id = s.curso_id
    -> JOIN aluno a ON a.id = r.aluno_id;
+-------+
| nota  |
+-------+
|  8.00 |
|  0.00 |
|  7.00 |
|  6.00 |
|  9.00 |
| 10.00 |
|  4.00 |
|  4.00 |
|  7.00 |
|  8.00 |
|  6.00 |
|  7.00 |
|  4.00 |
|  9.00 |
|  3.00 |
|  5.00 |
|  5.00 |
|  5.00 |
|  6.00 |
|  8.00 |
|  8.00 |
|  9.00 |
| 10.00 |
|  2.00 |
|  0.00 |
|  1.00 |
|  4.00 |
+-------+
27 rows in set (0.00 sec)

```

```sql
MariaDB [escola]> SELECT a.nome,c.nome,AVG( n.nota)
    -> FROM nota n
    -> JOIN resposta r ON r.id = n.resposta_id
    -> JOIN exercicio e ON e.id = r.exercicio_id
    -> JOIN secao s ON s.id = e.secao_id
    -> JOIN curso c ON c.id = s.curso_id
    -> JOIN aluno a ON a.id = r.aluno_id;
+---------------+----------------------+--------------+
| nome          | nome                 | AVG( n.nota) |
+---------------+----------------------+--------------+
| João da Silva | SQL e banco de dados |     5.740741 |
+---------------+----------------------+--------------+
1 row in set (0.00 sec)

```

Lembre-se que estamos lidando com uma função de agregação, ou seja, se não informarmos a forma
que ela precisa agrupar as colunas, ela retornará apenas uma linha! Porém, precisamos sempre pensar
em qual tipo de agrupamento é necessário, nesse caso queremos que mostre a média de acada aluno,
então agruparemos pelos alunos, porém também que queremos que a cada curso que o alunos fez mostre
a sua :

```sql
MariaDB [escola]> SELECT a.nome,c.nome,AVG( n.nota)
    -> FROM nota n
    -> JOIN resposta r ON r.id = n.resposta_id
    -> JOIN exercicio e ON e.id = r.exercicio_id
    -> JOIN secao s ON s.id = e.secao_id
    -> JOIN curso c ON c.id = s.curso_id
    -> JOIN aluno a ON a.id = r.aluno_id
    -> GROUP BY a.nome, c.nome;
+----------------+---------------------------------+--------------+
| nome           | nome                            | AVG( n.nota) |
+----------------+---------------------------------+--------------+
| Alberto Santos | Scrum e métodos ágeis           |     5.777778 |
| Frederico José | Desenvolvimento web com VRaptor |     8.000000 |
| Frederico José | SQL e banco de dados            |     5.666667 |
| João da Silva  | SQL e banco de dados            |     6.285714 |
| Renata Alonso  | C# e orientação a objetos       |     4.857143 |
+----------------+---------------------------------+--------------+
5 rows in set (0.00 sec)
```

#### 9.1 CONDIÇÕES COM HAVING

Retornamos todas as médias dos alunos, porém a instituição precisa de um relatório separado para
todos os alunos que reprovaram, ou seja, que tiraram nota baixa, nesse caso médias menores que 5. De
acordo com o que vimos até agora bastaria adicionarmos um WHERE:

```sql
MariaDB [escola]> SELECT a.nome,c.nome,AVG( n.nota)
    -> FROM nota n
    -> JOIN resposta r ON r.id = n.resposta_id
    -> JOIN exercicio e ON e.id = r.exercicio_id
    -> JOIN secao s ON s.id = e.secao_id
    -> JOIN curso c ON c.id = s.curso_id
    -> JOIN aluno a ON a.id = r.aluno_id
    -> WHERE AVG(n.nota) < 5
    -> GROUP BY a.nome, c.nome;

ERROR 1111 (HY000): Invalid use of group function
```

Nesse caso, estamos tentando adicionar condições para uma função de agregação, porém, quando
queremos adicionar condições para funções de agregação precisamos utilizar o HAVING ao invés de
WHERE :

```sql
MariaDB [escola]> SELECT a.nome,c.nome,AVG( n.nota)
    -> FROM nota n
    -> JOIN resposta r ON r.id = n.resposta_id
    -> JOIN exercicio e ON e.id = r.exercicio_id
    -> JOIN secao s ON s.id = e.secao_id
    -> JOIN curso c ON c.id = s.curso_id
    -> JOIN aluno a ON a.id = r.aluno_id
    -> GROUP BY a.nome, c.nome
    -> HAVING AVG(n.nota) < 5;
+---------------+---------------------------+--------------+
| nome          | nome                      | AVG( n.nota) |
+---------------+---------------------------+--------------+
| Renata Alonso | C# e orientação a objetos |     4.857143 |
+---------------+---------------------------+--------------+
1 row in set (0.00 sec)

```


A instuição enviou mais uma solicitação de um relatório informando quais cursos tem poucos
alunos para tomar uma decisão se vai manter os cursos ou se irá cancelá-los. Então vamos novamente
fazer a nossa query por passos, primeiro vamos começar selecionando os cursos:

```sql
MariaDB [escola]> SELECT c.nome, COUNT(a.id) FROM curso c
    -> JOIN matricula m ON m.curso_id = c.id
    -> JOIN aluno a ON m.aluno_id = a.id
    -> GROUP BY c.nome;
+------------------------------------+-------------+
| nome                               | COUNT(a.id) |
+------------------------------------+-------------+
| C# e orientação a objetos          |           4 |
| Desenvolvimento mobile com Android |           2 |
| Desenvolvimento web com VRaptor    |           2 |
| Scrum e métodos ágeis              |           2 |
| SQL e banco de dados               |           4 |
+------------------------------------+-------------+
5 rows in set (0.00 sec)

```

```sql
MariaDB [escola]> SELECT c.nome, COUNT(a.id) FROM curso c
    -> JOIN matricula m ON m.curso_id = c.id
    -> JOIN aluno a ON m.aluno_id = a.id
    -> GROUP BY c.nome
    -> HAVING COUNT(a.id) < 10;
+------------------------------------+-------------+
| nome                               | COUNT(a.id) |
+------------------------------------+-------------+
| C# e orientação a objetos          |           4 |
| Desenvolvimento mobile com Android |           2 |
| Desenvolvimento web com VRaptor    |           2 |
| Scrum e métodos ágeis              |           2 |
| SQL e banco de dados               |           4 |
+------------------------------------+-------------+
5 rows in set (0.00 sec)

```

#### resumindo

Sabemos que para adicionarmos filtros apenas para colunas utilizamos a instrução WHERE e
indicamos todas as peculiaridades necessárias, porém quando precisamos adicionar filtros para funções
de agregação, como por exemplo o AVG() , precisamos utilizar a instrução HAVING . Além disso, é
sempre bom lembrar que, quando estamos desenvolvendo queries grandes, é recomendado que faça
passa-a-passo queries menores, ou seja, resolva os menores problemas juntando cada tabela por vez e
teste para verificar se está funcionando, pois isso ajuda a verificar aonde está o problema da query.

Lista de Exercícios

- https://github.com/josemalcher/SQL-e-Modelagem-com-Banco-de-Dados-Livro-Alura/tree/master/9-ListaExercicio




[Voltar ao Índice](#indice)

---

## <a name="parte10">MÚLTIPLOS VALORES NA CONDIÇÃO E O IN</a>

O setor de financeiro dessa instituição, solicitou um relatório informando todas as formas de pagamento
cadastradas no banco de dados para verificar se está de acordo com o que eles trabalham. Na base de
dados, se verificarmos a tabela matricula :

```sql
MariaDB [escola]> SELECT  m.tipo FROM matricula m;
+-------------+
| tipo        |
+-------------+
| PAGA_PF     |
| PAGA_PJ     |
| PAGA_PF     |
| PAGA_CHEQUE |
| PAGA_BOLETO |
| PAGA_PJ     |
| PAGA_PF     |
| PAGA_PJ     |
| PAGA_PJ     |
| PAGA_CHEQUE |
| PAGA_BOLETO |
| PAGA_PJ     |
| PAGA_PF     |
| PAGA_PJ     |
+-------------+
14 rows in set (0.00 sec)

```
Veja que foram retornados tipos de pagamento iguais, porém precisamos enviar um relatório apenas
com os tipos de pagamento distintos. Para retornamos os valores distintos de uma coluna podemos
utilizar a instrução DISTINCT :

```sql
MariaDB [escola]> SELECT DISTINCT m.tipo FROM matricula m;
+-------------+
| tipo        |
+-------------+
| PAGA_PF     |
| PAGA_PJ     |
| PAGA_CHEQUE |
| PAGA_BOLETO |
+-------------+
4 rows in set (0.00 sec)

```

Conseguimos retornar o relatório das formas de pagamento, porém o setor financeiro ainda precisa
saber de mais informações. Agora foi solicitado que enviasse um relatório com os cursos e a quantidade
de alunos que possuem o tipo de pagamento PJ. 

Sabemos que o relatório é sobre a quantidade de matrículas que foram pagas como PJ, precisamos
contar, ou seja, usaremos a função COUNT() . Vamos começar a contar a quantidade de matrículas:

```sql
MariaDB [escola]> SELECT COUNT(m.id) FROM matricula m;
+-------------+
| COUNT(m.id) |
+-------------+
|          14 |
+-------------+
1 row in set (0.00 sec)

```

```sql
MariaDB [escola]> SELECT c.nome, COUNT(m.id) FROM matricula m
    -> JOIN curso c ON m.curso_id = c.id;
+----------------------+-------------+
| nome                 | COUNT(m.id) |
+----------------------+-------------+
| SQL e banco de dados |          14 |
+----------------------+-------------+
1 row in set (0.00 sec)

```

Observe que foi retornado apenas uma linha! Isso significa que a função COUNT() também é uma
função de agregação, ou seja, se queremos adicionar mais colunas na nossa query, precisamos agrupá-las.
Então vamos agrupar o nome do curso:

```sql
MariaDB [escola]> SELECT c.nome, COUNT(m.id) FROM matricula m
    -> JOIN curso c ON m.curso_id = c.id
    -> GROUP BY c.nome;
+------------------------------------+-------------+
| nome                               | COUNT(m.id) |
+------------------------------------+-------------+
| C# e orientação a objetos          |           4 |
| Desenvolvimento mobile com Android |           2 |
| Desenvolvimento web com VRaptor    |           2 |
| Scrum e métodos ágeis              |           2 |
| SQL e banco de dados               |           4 |
+------------------------------------+-------------+
5 rows in set (0.00 sec)
```

Conseguimos retornar todas os cursos e a quantidade de matrículas, porém precisamos filtrar por
tipo de pagamento PJ. Então vamos adicionar um WHERE :

```sql
MariaDB [escola]> SELECT c.nome, COUNT(m.id) FROM matricula m
    -> JOIN curso c ON m.curso_id = c.id
    -> WHERE m.tipo = 'PAGA_PJ'
    -> GROUP BY c.nome;
+------------------------------------+-------------+
| nome                               | COUNT(m.id) |
+------------------------------------+-------------+
| C# e orientação a objetos          |           1 |
| Desenvolvimento mobile com Android |           2 |
| Desenvolvimento web com VRaptor    |           1 |
| Scrum e métodos ágeis              |           1 |
| SQL e banco de dados               |           1 |
+------------------------------------+-------------+
5 rows in set (0.00 sec)
```

#### FILTROS UTILIZANDO O IN

O setor financeiro da instituição precisa de mais detalhes sobre os tipos de pagamento de cada curso,
eles precisam de um relatório similar ao que fizemos, porém para todos que sejam pagamento PJ e PF.
Para diferenciar o tipo de pagamento, precisaremos adicionar a coluna do m.tipo :

```sql
MariaDB [escola]> SELECT c.nome, COUNT(m.id), m.tipo
    -> FROM matricula m
    -> JOIN curso c ON m.curso_id = c.id
    -> WHERE m.tipo = 'PAGA_PJ' OR m.tipo = 'PAGA_PF'
    -> GROUP BY c.nome, m.tipo;
+------------------------------------+-------------+---------+
| nome                               | COUNT(m.id) | tipo    |
+------------------------------------+-------------+---------+
| C# e orientação a objetos          |           1 | PAGA_PF |
| C# e orientação a objetos          |           1 | PAGA_PJ |
| Desenvolvimento mobile com Android |           2 | PAGA_PJ |
| Desenvolvimento web com VRaptor    |           1 | PAGA_PF |
| Desenvolvimento web com VRaptor    |           1 | PAGA_PJ |
| Scrum e métodos ágeis              |           1 | PAGA_PF |
| Scrum e métodos ágeis              |           1 | PAGA_PJ |
| SQL e banco de dados               |           1 | PAGA_PF |
| SQL e banco de dados               |           1 | PAGA_PJ |
+------------------------------------+-------------+---------+
9 rows in set (0.00 sec)

```

Em SQL, existe a instrução IN que permite especificarmos mais de um
valor que precisamos filtrar ao mesmo tempo para uma determinada coluna:

```sql
MariaDB [escola]> SELECT c.nome, COUNT(m.id), m.tipo
    -> FROM matricula m
    -> JOIN curso c ON m.curso_id = c.id
    -> WHERE m.tipo IN ('PAGA_PJ', 'PAGA_PF', 'PAGA_CHEQUE', 'PAGA_BOLETO')
    -> GROUP BY c.nome, m.tipo;
+------------------------------------+-------------+-------------+
| nome                               | COUNT(m.id) | tipo        |
+------------------------------------+-------------+-------------+
| C# e orientação a objetos          |           1 | PAGA_BOLETO |
| C# e orientação a objetos          |           1 | PAGA_CHEQUE |
| C# e orientação a objetos          |           1 | PAGA_PF     |
| C# e orientação a objetos          |           1 | PAGA_PJ     |
| Desenvolvimento mobile com Android |           2 | PAGA_PJ     |
| Desenvolvimento web com VRaptor    |           1 | PAGA_PF     |
| Desenvolvimento web com VRaptor    |           1 | PAGA_PJ     |
| Scrum e métodos ágeis              |           1 | PAGA_PF     |
| Scrum e métodos ágeis              |           1 | PAGA_PJ     |
| SQL e banco de dados               |           1 | PAGA_BOLETO |
| SQL e banco de dados               |           1 | PAGA_CHEQUE |
| SQL e banco de dados               |           1 | PAGA_PF     |
| SQL e banco de dados               |           1 | PAGA_PJ     |
+------------------------------------+-------------+-------------+
13 rows in set (0.00 sec)
```


A instituição nomeou 3 alunos como os mais destacados nos últimos cursos realizamos e gostaria de
saber quais foram todos os cursos que eles fizeram. Os 3 alunos que se destacaram foram: João da Silva,
Alberto Santos e a Renata Alonso. Vamos verificar quais são os id s desses alunos:

```sql
MariaDB [escola]> SELECT a.nome , c.nome
    -> FROM curso c
    -> JOIN matricula m ON m.curso_id = c.id
    -> JOIN aluno a ON m.aluno_id = a.id
    -> WHERE a.id in (1,3,4)
    -> ORDER BY a.nome;
+----------------+------------------------------------+
| nome           | nome                               |
+----------------+------------------------------------+
| Alberto Santos | Scrum e métodos ágeis              |
| Alberto Santos | C# e orientação a objetos          |
| João da Silva  | SQL e banco de dados               |
| João da Silva  | C# e orientação a objetos          |
| Renata Alonso  | C# e orientação a objetos          |
| Renata Alonso  | Desenvolvimento mobile com Android |
+----------------+------------------------------------+
6 rows in set (0.00 sec)


```

Na instituição, serão lançados alguns cursos novos de .NET e o pessoal do comercial precisa divulgar
esses cursos para os ex-alunos, porém apenas para os ex-alunos que já fizeram os cursos de C# e de SQL.
Inicialmente vamos verificar os id s desses cursos:

```sql
MariaDB [escola]> SELECT a.nome, c.nome
    -> FROM aluno a
    -> JOIN matricula m ON m.aluno_id = a.id
    -> JOIN curso c ON m.curso_id = c.id
    -> WHERE c.id IN (1,4)
    -> ORDER BY c.nome;
+----------------+---------------------------+
| nome           | nome                      |
+----------------+---------------------------+
| Renata Alonso  | C# e orientação a objetos |
| João da Silva  | C# e orientação a objetos |
| Frederico José | C# e orientação a objetos |
| Alberto Santos | C# e orientação a objetos |
| João da Silva  | SQL e banco de dados      |
| Frederico José | SQL e banco de dados      |
| Paulo José     | SQL e banco de dados      |
| Manoel Santos  | SQL e banco de dados      |
+----------------+---------------------------+
8 rows in set (0.00 sec)

```
Agora sabemos que apenas os alunos Frederico José e João da Silva, são os ex-alunos aptos para
realizar os novos cursos de .NET.

#### resumo

Nesse capítulo vimos que quando precisamos saber todos os valores de uma determinada coluna
podemos utilizar a instrução DISTINCT para retornar todos os valores distintos, ou seja, sem nenhuma
repetição. Vimos também que quando precisamos realizar vários filtros para uma mesma coluna,
podemos utilizar a instrução IN passando por parâmetro todos os valores que esperamos que seja
retornado, ao invés de ficar preenchendo a nossa query com vários OR s.


#### lista de exercício

- https://github.com/josemalcher/SQL-e-Modelagem-com-Banco-de-Dados-Livro-Alura/tree/master/10-ListaExercicio



[Voltar ao Índice](#indice)

---

## <a name="parte11">SUB-QUERIES</a>

A instituição precisa de um relatório mais robusto, com as seguintes informações: Precisa do nome do
aluno e curso, a média do aluno em relação ao curso e a diferença entre a média do aluno e a média geral
do curso.

```sql
MariaDB [escola]> SELECT a.nome, c.nome, n.nota, AVG(n.nota)
    -> FROM nota n
    -> JOIN resposta r ON n.resposta_id = r.id
    -> JOIN exercicio e ON r.exercicio_id = r.exercicio_id
    -> JOIN secao s ON e.secao_id = s.id
    -> JOIN curso c ON s.curso_id = c.id
    -> JOIN aluno a ON r.aluno_id = a.id
    -> GROUP BY a.nome, c.nome;
+----------------+---------------------------------+------+-------------+
| nome           | nome                            | nota | AVG(n.nota) |
+----------------+---------------------------------+------+-------------+
| Alberto Santos | C# e orientação a objetos       | 7.00 |    5.777778 |
| Alberto Santos | Desenvolvimento web com VRaptor | 7.00 |    5.777778 |
| Alberto Santos | Scrum e métodos ágeis           | 7.00 |    5.777778 |
| Alberto Santos | SQL e banco de dados            | 7.00 |    5.777778 |
| Frederico José | C# e orientação a objetos       | 4.00 |    6.250000 |
| Frederico José | Desenvolvimento web com VRaptor | 4.00 |    6.250000 |
| Frederico José | Scrum e métodos ágeis           | 4.00 |    6.250000 |
| Frederico José | SQL e banco de dados            | 4.00 |    6.250000 |
| João da Silva  | C# e orientação a objetos       | 8.00 |    6.285714 |
| João da Silva  | Desenvolvimento web com VRaptor | 8.00 |    6.285714 |
| João da Silva  | Scrum e métodos ágeis           | 8.00 |    6.285714 |
| João da Silva  | SQL e banco de dados            | 8.00 |    6.285714 |
| Renata Alonso  | C# e orientação a objetos       | 8.00 |    4.857143 |
| Renata Alonso  | Desenvolvimento web com VRaptor | 8.00 |    4.857143 |
| Renata Alonso  | Scrum e métodos ágeis           | 8.00 |    4.857143 |
| Renata Alonso  | SQL e banco de dados            | 8.00 |    4.857143 |
+----------------+---------------------------------+------+-------------+
16 rows in set (0.00 sec)

```

Agora nós temos a média do aluno e seu respectivo curso, mas ainda falta a coluna da diferença que
calcula a diferença entre a média do aluno em um determinado curso e subtrai pela média geral. Porém
ainda não temos a média geral, então como podemos pegar a média geral? Vamos verificar a tabela
nota :

```sql
MariaDB [escola]> SELECT a.nome, c.nome, n.nota, AVG(n.nota) AS media_aluno,
    ->     AVG(n.nota) - (SELECT AVG(n.nota) FROM nota n) AS diferenca
    -> FROM nota n
    -> JOIN resposta r ON n.resposta_id = r.id
    -> JOIN exercicio e ON r.exercicio_id = r.exercicio_id
    -> JOIN secao s ON e.secao_id = s.id
    -> JOIN curso c ON s.curso_id = c.id
    -> JOIN aluno a ON r.aluno_id = a.id
    -> GROUP BY a.nome, c.nome;
+----------------+---------------------------------+------+-------------+-----------+
| nome           | nome                            | nota | media_aluno | diferenca |
+----------------+---------------------------------+------+-------------+-----------+
| Alberto Santos | C# e orientação a objetos       | 7.00 |    5.777778 |  0.037037 |
| Alberto Santos | Desenvolvimento web com VRaptor | 7.00 |    5.777778 |  0.037037 |
| Alberto Santos | Scrum e métodos ágeis           | 7.00 |    5.777778 |  0.037037 |
| Alberto Santos | SQL e banco de dados            | 7.00 |    5.777778 |  0.037037 |
| Frederico José | C# e orientação a objetos       | 4.00 |    6.250000 |  0.509259 |
| Frederico José | Desenvolvimento web com VRaptor | 4.00 |    6.250000 |  0.509259 |
| Frederico José | Scrum e métodos ágeis           | 4.00 |    6.250000 |  0.509259 |
| Frederico José | SQL e banco de dados            | 4.00 |    6.250000 |  0.509259 |
| João da Silva  | C# e orientação a objetos       | 8.00 |    6.285714 |  0.544974 |
| João da Silva  | Desenvolvimento web com VRaptor | 8.00 |    6.285714 |  0.544974 |
| João da Silva  | Scrum e métodos ágeis           | 8.00 |    6.285714 |  0.544974 |
| João da Silva  | SQL e banco de dados            | 8.00 |    6.285714 |  0.544974 |
| Renata Alonso  | C# e orientação a objetos       | 8.00 |    4.857143 | -0.883598 |
| Renata Alonso  | Desenvolvimento web com VRaptor | 8.00 |    4.857143 | -0.883598 |
| Renata Alonso  | Scrum e métodos ágeis           | 8.00 |    4.857143 | -0.883598 |
| Renata Alonso  | SQL e banco de dados            | 8.00 |    4.857143 | -0.883598 |
+----------------+---------------------------------+------+-------------+-----------+
16 rows in set (0.00 sec)

```

CORREÇÕES + MEDIA GERAL:

```sql
SELECT a.nome,
    ->        c.nome,
    ->        AVG(n.nota) AS media_aluno,
    ->        (SELECT AVG(n.nota) FROM nota n) AS media_geral,
    ->        AVG(n.nota) - (SELECT AVG(n.nota) FROM nota n) AS diferenca
    -> FROM nota n
    -> JOIN resposta r ON n.resposta_id = r.id
    -> JOIN exercicio e ON r.exercicio_id = e.id
    -> JOIN secao s ON e.secao_id = s.id
    -> JOIN curso c ON s.curso_id = c.id
    -> JOIN aluno a ON r.aluno_id = a.id
    -> GROUP BY a.nome, c.nome;
+----------------+---------------------------------+-------------+-------------+-----------+
| nome           | nome                            | media_aluno | media_geral | diferenca |
+----------------+---------------------------------+-------------+-------------+-----------+
| Alberto Santos | Scrum e métodos ágeis           |    5.777778 |    5.740741 |  0.037037 |
| Frederico José | Desenvolvimento web com VRaptor |    8.000000 |    5.740741 |  2.259259 |
| Frederico José | SQL e banco de dados            |    5.666667 |    5.740741 | -0.074074 |
| João da Silva  | SQL e banco de dados            |    6.285714 |    5.740741 |  0.544974 |
| Renata Alonso  | C# e orientação a objetos       |    4.857143 |    5.740741 | -0.883598 |
+----------------+---------------------------------+-------------+-------------+-----------+
5 rows in set (0.00 sec)


```

Conseguimos exibir o relatório como esperado, porém existe um pequeno detalhe. Note que o
resultado da subquery (SELECT AVG(n.nota) FROM nota n) foi de apenas uma linha e é justamente por
esse motivo que conseguimos efetuar operações aritméticas como, nesse caso, a subtração. Se o resultado
fosse mais de uma linha, não seria possível realizar operações.


A instituição precisa de um relatório do aproveitamento dos alunos nos cursos, ou seja, precisamos
saber se eles estão respondendo todos os exercícios, então iremos buscar o número de respostas que cada
respondeu aluno individualmente.

```sql
MariaDB [escola]> SELECT a.nome,
    ->        (SELECT COUNT(r.id) FROM resposta r WHERE r.aluno_id = a.id) AS quantidade_respostas
    -> FROM aluno a;
+------------------+----------------------+
| nome             | quantidade_respostas |
+------------------+----------------------+
| João da Silva    |                    7 |
| Frederico José   |                    4 |
| Alberto Santos   |                    9 |
| Renata Alonso    |                    7 |
| Paulo da Silva   |                    0 |
| Carlos Cunha     |                    0 |
| Paulo José       |                    0 |
| Manoel Santos    |                    0 |
| Renata Ferreira  |                    0 |
| Paula Soares     |                    0 |
| Jose da Silva    |                    0 |
| Danilo Cunha     |                    0 |
| Zilmira José     |                    0 |
| Cristaldo Santos |                    0 |
| Osmir Ferreira   |                    0 |
| Claudio Soares   |                    0 |
+------------------+----------------------+
16 rows in set (0.00 sec)

```

A instituição precisa de uma relatório muito parecido com a query que acabamos de fazer, ela precisa
saber quantas matrículas um aluno tem, ou seja, ao ínves de resposta, informaremos as matrículas. Então
vamos apenas substituir as informações das respostas pelas informações da matrícula:

```sql
MariaDB [escola]> SELECT a.nome,
    ->        (SELECT COUNT(m.id) FROM matricula m WHERE m.aluno_id = a.id) AS quantidade_matricula
    -> FROM aluno a;
+------------------+----------------------+
| nome             | quantidade_matricula |
+------------------+----------------------+
| João da Silva    |                    2 |
| Frederico José   |                    3 |
| Alberto Santos   |                    2 |
| Renata Alonso    |                    2 |
| Paulo da Silva   |                    0 |
| Carlos Cunha     |                    0 |
| Paulo José       |                    1 |
| Manoel Santos    |                    2 |
| Renata Ferreira  |                    1 |
| Paula Soares     |                    1 |
| Jose da Silva    |                    0 |
| Danilo Cunha     |                    0 |
| Zilmira José     |                    0 |
| Cristaldo Santos |                    0 |
| Osmir Ferreira   |                    0 |
| Claudio Soares   |                    0 |
+------------------+----------------------+
16 rows in set (0.00 sec)


```


Conseguimos pegar a quantidade de resposta e matricula de um determinado aluno, porém fizemos
isso separadamente, porém agora precisamos juntar essas informações para montar em um único
relatório que mostre, o nome do aluno, a quantidade de respostas e a quantidade de matrículas. Então
vamos partir do princípio, ou seja, fazer a query que retorna todos os alunos:

```sql
MariaDB [escola]> SELECT a.nome,
    ->        (SELECT COUNT(m.id) FROM matricula m WHERE m.aluno_id = a.id) AS quantidade_matricula,
    ->        (SELECT COUNT(r.id) FROM resposta r WHERE r.aluno_id = a.id) AS quantidade_respostas
    -> FROM aluno a;
+------------------+----------------------+----------------------+
| nome             | quantidade_matricula | quantidade_respostas |
+------------------+----------------------+----------------------+
| João da Silva    |                    2 |                    7 |
| Frederico José   |                    3 |                    4 |
| Alberto Santos   |                    2 |                    9 |
| Renata Alonso    |                    2 |                    7 |
| Paulo da Silva   |                    0 |                    0 |
| Carlos Cunha     |                    0 |                    0 |
| Paulo José       |                    1 |                    0 |
| Manoel Santos    |                    2 |                    0 |
| Renata Ferreira  |                    1 |                    0 |
| Paula Soares     |                    1 |                    0 |
| Jose da Silva    |                    0 |                    0 |
| Danilo Cunha     |                    0 |                    0 |
| Zilmira José     |                    0 |                    0 |
| Cristaldo Santos |                    0 |                    0 |
| Osmir Ferreira   |                    0 |                    0 |
| Claudio Soares   |                    0 |                    0 |
+------------------+----------------------+----------------------+
16 rows in set (0.00 sec)

```




[Voltar ao Índice](#indice)

---

## <a name="parte12">ENTENDENDO O LEFT JOIN</a>

Os instrutores da instituição pediram um relatório com os alunos que são mais participativos na sala de
aula, ou seja, queremos retornar os alunos que responderam mais exercícios. Consequentemente
encontraremos também os alunos que não estão participando muito, então já aproveitamos e
conversamos com eles para entender o que está acontecendo.

```sql
MariaDB [escola]> SELECT a.nome, COUNT(r.id) AS respostas
    -> FROM aluno a
    -> JOIN resposta r ON r.aluno_id = a.id
    -> GROUP BY a.nome;
+----------------+-----------+
| nome           | respostas |
+----------------+-----------+
| Alberto Santos |         9 |
| Frederico José |         4 |
| João da Silva  |         7 |
| Renata Alonso  |         7 |
+----------------+-----------+
4 rows in set (0.00 sec)

```

Alunos sem resposta desapareceram? Vamos procurar todos os alunos que não possuem nenhuma
resposta. Isto é selecionar os alunos que não existe, resposta deste aluno:

```sql
MariaDB [escola]> SELECT a.nome
    -> FROM aluno a
    -> WHERE NOT EXISTS(SELECT r.id FROM resposta r WHERE r.aluno_id = a.id);
+------------------+
| nome             |
+------------------+
| Paulo da Silva   |
| Carlos Cunha     |
| Paulo José       |
| Manoel Santos    |
| Renata Ferreira  |
| Paula Soares     |
| Jose da Silva    |
| Danilo Cunha     |
| Zilmira José     |
| Cristaldo Santos |
| Osmir Ferreira   |
| Claudio Soares   |
+------------------+
12 rows in set (0.00 sec)
```

Se verificarmos os nomes, realmente, todos os alunos que não tem respostas não estão sendo
retornados naquela primeira query. Porém nós queremos também que retorne os alunos sem respostas...
Vamos tentar de uma outra maneira, vamos retornar o nome do aluno e a resposta que ele respondeu:

Agora vamos adicionar a coluna id da tabela aluno e aluno_id da tabela resposta :

```sql
MariaDB [escola]> SELECT a.nome, r.aluno_id ,r.resposta_dada
    -> FROM aluno a
    -> JOIN resposta r ON r.aluno_id = a.id;
+----------------+----------+-----------------------------------------------------------------------------------+
| nome           | aluno_id | resposta_dada                                                                     |
+----------------+----------+-----------------------------------------------------------------------------------+
| João da Silva  |        1 | uma selecao                                                                       |
| João da Silva  |        1 | ixi, nao sei                                                                      |
| João da Silva  |        1 | alterar dados                                                                     |
| João da Silva  |        1 | eskecer o where e alterar tudo                                                    |
| João da Silva  |        1 | apagar coisas                                                                     |
| João da Silva  |        1 | tb nao pode eskecer o where                                                       |
| João da Silva  |        1 | inserir dados                                                                     |
| Frederico José |        2 | buscar dados                                                                      |
| Frederico José |        2 | select campos from tabela                                                         |
| Frederico José |        2 | alterar coisas                                                                    |
| Frederico José |        2 | ixi, nao sei                                                                      |
| Alberto Santos |        3 | tempo pra fazer algo                                                              |
| Alberto Santos |        3 | 1 a 4 semanas                                                                     |
| Alberto Santos |        3 | melhoria do processo                                                              |
| Alberto Santos |        3 | todo dia                                                                          |
| Alberto Santos |        3 | reuniao de status                                                                 |
| Alberto Santos |        3 | todo dia                                                                          |
| Alberto Santos |        3 | o quadro branco                                                                   |
| Alberto Santos |        3 | um metodo agil                                                                    |
| Alberto Santos |        3 | tem varios outros                                                                 |
| Renata Alonso  |        4 | eh a internet                                                                     |
| Renata Alonso  |        4 | browser faz requisicao, servidor manda resposta                                   |
| Renata Alonso  |        4 | eh o servidor que lida com http                                                   |
| Renata Alonso  |        4 | nao sei                                                                           |
| Renata Alonso  |        4 | banco de dados!                                                                   |
| Renata Alonso  |        4 | eh colocar a app na internet                                                      |
| Renata Alonso  |        4 | depende da tecnologia, mas geralmente eh levar pra um servidor que ta na internet |
+----------------+----------+-----------------------------------------------------------------------------------+
27 rows in set (0.00 sec)

```

Perceba que separamos as informações dos alunos de um lado e a das respostas do outro lado.
Analisando esses dados podemos verificar que quando fizemos o JOIN entre a tabela aluno e
resposta estamos trazendo apenas todos os registros que possuem o id da tabela aluno e o
aluno_id da tabela resposta .

O que o SQL faz também é que todos os alunos cujo o id não esteja na coluna aluno_id não
serão retornados! Isto é, ele só trará para nós alguém que o JOIN tenha valor igual nas duas tabelas. Se
só está presente em uma das tabelas, ele ignora.

Em SQL, existe um JOIN diferente que permite o retorno de alunos que também não possuam o
id na tabela que está sendo associada. Queremos pegar todo mundo da tabela da esquerda,
independentemente de existir ou não um valor na tabela da direita. É um tal de join de esquerda, o LEFT
JOIN , ou seja, ele trará todos os registros da tabela da esquerda mesmo que não exista uma associação
na tabela da direita:

```sql
MariaDB [escola]> SELECT a.nome, r.aluno_id ,r.resposta_dada
    -> FROM aluno a
    -> LEFT JOIN resposta r ON r.aluno_id = a.id;
+------------------+----------+-----------------------------------------------------------------------------------+
| nome             | aluno_id | resposta_dada                                                                     |
+------------------+----------+-----------------------------------------------------------------------------------+
| João da Silva    |        1 | uma selecao                                                                       |
| João da Silva    |        1 | ixi, nao sei                                                                      |
| João da Silva    |        1 | alterar dados                                                                     |
| João da Silva    |        1 | eskecer o where e alterar tudo                                                    |
| João da Silva    |        1 | apagar coisas                                                                     |
| João da Silva    |        1 | tb nao pode eskecer o where                                                       |
| João da Silva    |        1 | inserir dados                                                                     |
| Frederico José   |        2 | buscar dados                                                                      |
| Frederico José   |        2 | select campos from tabela                                                         |
| Frederico José   |        2 | alterar coisas                                                                    |
| Frederico José   |        2 | ixi, nao sei                                                                      |
| Alberto Santos   |        3 | tempo pra fazer algo                                                              |
| Alberto Santos   |        3 | 1 a 4 semanas                                                                     |
| Alberto Santos   |        3 | melhoria do processo                                                              |
| Alberto Santos   |        3 | todo dia                                                                          |
| Alberto Santos   |        3 | reuniao de status                                                                 |
| Alberto Santos   |        3 | todo dia                                                                          |
| Alberto Santos   |        3 | o quadro branco                                                                   |
| Alberto Santos   |        3 | um metodo agil                                                                    |
| Alberto Santos   |        3 | tem varios outros                                                                 |
| Renata Alonso    |        4 | eh a internet                                                                     |
| Renata Alonso    |        4 | browser faz requisicao, servidor manda resposta                                   |
| Renata Alonso    |        4 | eh o servidor que lida com http                                                   |
| Renata Alonso    |        4 | nao sei                                                                           |
| Renata Alonso    |        4 | banco de dados!                                                                   |
| Renata Alonso    |        4 | eh colocar a app na internet                                                      |
| Renata Alonso    |        4 | depende da tecnologia, mas geralmente eh levar pra um servidor que ta na internet |
| Paulo da Silva   |     NULL | NULL                                                                              |
| Carlos Cunha     |     NULL | NULL                                                                              |
| Paulo José       |     NULL | NULL                                                                              |
| Manoel Santos    |     NULL | NULL                                                                              |
| Renata Ferreira  |     NULL | NULL                                                                              |
| Paula Soares     |     NULL | NULL                                                                              |
| Jose da Silva    |     NULL | NULL                                                                              |
| Danilo Cunha     |     NULL | NULL                                                                              |
| Zilmira José     |     NULL | NULL                                                                              |
| Cristaldo Santos |     NULL | NULL                                                                              |
| Osmir Ferreira   |     NULL | NULL                                                                              |
| Claudio Soares   |     NULL | NULL                                                                              |
+------------------+----------+-----------------------------------------------------------------------------------+
39 rows in set (0.00 sec)

```

Conseguimos retornar todos os registros. Então agora vamos tentar contar novamentes as respostas,
agrupando pelo nome:

```sql
MariaDB [escola]> SELECT a.nome,
    ->        COUNT(r.id) AS respostas
    -> FROM aluno a
    -> LEFT JOIN resposta r ON r.aluno_id = a.id
    -> GROUP BY a.nome;
+------------------+-----------+
| nome             | respostas |
+------------------+-----------+
| Alberto Santos   |         9 |
| Carlos Cunha     |         0 |
| Claudio Soares   |         0 |
| Cristaldo Santos |         0 |
| Danilo Cunha     |         0 |
| Frederico José   |         4 |
| João da Silva    |         7 |
| Jose da Silva    |         0 |
| Manoel Santos    |         0 |
| Osmir Ferreira   |         0 |
| Paula Soares     |         0 |
| Paulo da Silva   |         0 |
| Paulo José       |         0 |
| Renata Alonso    |         7 |
| Renata Ferreira  |         0 |
| Zilmira José     |         0 |
+------------------+-----------+
16 rows in set (0.00 sec)

```

### 12.1 RIGHT JOIN

Vamos supor que ao invés de retornar todos os alunos e suas respostas, mesmo que o aluno não
tenha nenhuma resposta, queremos faer o contrário, ou seja, retornar todos as respostas que foram
respondidas e as que não foram respondidas. Vamos verificar se existe alguma resposta que não foi
respondida por um aluno:

```sql
MariaDB [escola]> SELECT r.id
    -> FROM resposta r
    -> WHERE r.aluno_id IS NULL;
Empty set (0.00 sec)

```

Adicionando uma resposta sem aluno:

```sql
INSERT INTO resposta (resposta_dada) VALUE ('x vale 10.');
```

Agora existe uma resposta que não foi associada a um aluno. Da mesma forma que utilizamos um
JOIN diferente para pegar todos os dados da tabela da esquerda (LEFT) mesmo que não tenha
associação com a tabela que está sendo juntada, existe também o JOIN que fará o procedimento, porém
para a tabela da direita (RIGHT), que é o tal do RIGHT JOIN :

```sql
MariaDB [escola]> SELECT a.nome, r.resposta_dada
    -> FROM aluno a
    -> RIGHT JOIN resposta r ON r.aluno_id = a.id;
+----------------+-----------------------------------------------------------------------------------+
| nome           | resposta_dada                                                                     |
+----------------+-----------------------------------------------------------------------------------+
| João da Silva  | uma selecao                                                                       |
| João da Silva  | ixi, nao sei                                                                      |
| João da Silva  | alterar dados                                                                     |
| João da Silva  | eskecer o where e alterar tudo                                                    |
| João da Silva  | apagar coisas                                                                     |
| João da Silva  | tb nao pode eskecer o where                                                       |
| João da Silva  | inserir dados                                                                     |
| Frederico José | buscar dados                                                                      |
| Frederico José | select campos from tabela                                                         |
| Frederico José | alterar coisas                                                                    |
| Frederico José | ixi, nao sei                                                                      |
| Alberto Santos | tempo pra fazer algo                                                              |
| Alberto Santos | 1 a 4 semanas                                                                     |
| Alberto Santos | melhoria do processo                                                              |
| Alberto Santos | todo dia                                                                          |
| Alberto Santos | reuniao de status                                                                 |
| Alberto Santos | todo dia                                                                          |
| Alberto Santos | o quadro branco                                                                   |
| Alberto Santos | um metodo agil                                                                    |
| Alberto Santos | tem varios outros                                                                 |
| Renata Alonso  | eh a internet                                                                     |
| Renata Alonso  | browser faz requisicao, servidor manda resposta                                   |
| Renata Alonso  | eh o servidor que lida com http                                                   |
| Renata Alonso  | nao sei                                                                           |
| Renata Alonso  | banco de dados!                                                                   |
| Renata Alonso  | eh colocar a app na internet                                                      |
| Renata Alonso  | depende da tecnologia, mas geralmente eh levar pra um servidor que ta na internet |
| NULL           | x vale 10.                                                                        |
+----------------+-----------------------------------------------------------------------------------+
28 rows in set (0.00 sec)

```

Quando utilizamos ***apenas o JOIN*** significa que queremos retornar todos os registros que tenham
uma associação, ou seja, que exista tanto na tabela da esquerda quanto na tabela da direita, esse JOIN
também é conhecido como INNER JOIN . Vamos verificar o resultado utilizando o ***INNER JOIN*** :

```sql
MariaDB [escola]> SELECT a.nome, COUNT(r.id) AS respostas
    -> FROM aluno a
    ->        INNER JOIN resposta r ON r.aluno_id = a.id
    -> GROUP BY a.nome;
+----------------+-----------+
| nome           | respostas |
+----------------+-----------+
| Alberto Santos |         9 |
| Frederico José |         4 |
| João da Silva  |         7 |
| Renata Alonso  |         7 |
+----------------+-----------+
4 rows in set (0.00 sec)

```

### 12.2 JOIN OU SUBQUERY?

O resultado é o mesmo! Se o resultado é o mesmo, quando eu devo utilizar o JOIN ou as subqueries?
Aparentemente a subquery é mais enxuta e mais fácil de ser escrita, porém os SGBDs sempre terão um
desempenho melhor para JOIN em relação a subqueries, então prefira o uso de JOIN s ao invés de
subquery.

```sql
MariaDB [escola]> SELECT a.nome,
    ->        (SELECT COUNT(r.id) FROM resposta r WHERE r.aluno_id = a.id)  AS qtd_respostas,
    ->        (SELECT COUNT(m.id) FROM matricula m WHERE m.aluno_id = a.id) AS qtd_matriculas
    -> FROM aluno a;
+------------------+---------------+----------------+
| nome             | qtd_respostas | qtd_matriculas |
+------------------+---------------+----------------+
| João da Silva    |             7 |              2 |
| Frederico José   |             4 |              3 |
| Alberto Santos   |             9 |              2 |
| Renata Alonso    |             7 |              2 |
| Paulo da Silva   |             0 |              0 |
| Carlos Cunha     |             0 |              0 |
| Paulo José       |             0 |              1 |
| Manoel Santos    |             0 |              2 |
| Renata Ferreira  |             0 |              1 |
| Paula Soares     |             0 |              1 |
| Jose da Silva    |             0 |              0 |
| Danilo Cunha     |             0 |              0 |
| Zilmira José     |             0 |              0 |
| Cristaldo Santos |             0 |              0 |
| Osmir Ferreira   |             0 |              0 |
| Claudio Soares   |             0 |              0 |
+------------------+---------------+----------------+
16 rows in set (0.00 sec)

```


Repare que a nossa query associando um aluno 1, com a resposta 1 e matrícula 1, o mesmo aluno 1,
com resposta 1 e matrícula 11 e assim sucessivamente... Isso significa que essa query está multiplicando o
aluno(1) x respostas(7) x matrículas(2)... Com certeza essa contagem não funcionará! Precisamos de
resultados distintos, ou seja, iremos utilizar o DISTINCT para evitar esse problema. Agora podemos
contar as respostas e as matrículas:

```sql
MariaDB [escola]> SELECT a.nome, COUNT(DISTINCT r.id) AS qtd_respostas,
    -> COUNT(DISTINCT m.id) AS qtd_matriculas
    -> FROM aluno a
    -> LEFT JOIN resposta r ON r.aluno_id = a.id
    -> LEFT JOIN matricula m ON m.aluno_id = a.id
    -> GROUP BY a.nome;
+------------------+---------------+----------------+
| nome             | qtd_respostas | qtd_matriculas |
+------------------+---------------+----------------+
| Alberto Santos   |             9 |              2 |
| Carlos Cunha     |             0 |              0 |
| Claudio Soares   |             0 |              0 |
| Cristaldo Santos |             0 |              0 |
| Danilo Cunha     |             0 |              0 |
| Frederico José   |             4 |              3 |
| João da Silva    |             7 |              2 |
| Jose da Silva    |             0 |              0 |
| Manoel Santos    |             0 |              2 |
| Osmir Ferreira   |             0 |              0 |
| Paula Soares     |             0 |              1 |
| Paulo da Silva   |             0 |              0 |
| Paulo José       |             0 |              1 |
| Renata Alonso    |             7 |              2 |
| Renata Ferreira  |             0 |              1 |
| Zilmira José     |             0 |              0 |
+------------------+---------------+----------------+
16 rows in set (0.00 sec)

```

E se não adicionássemos a instrução DISTINCT ? O que aconteceria? Vamos testar:

```sql
MariaDB [escola]> SELECT a.nome, COUNT(r.id) AS qtd_respostas, COUNT(m.id) AS qtd_matriculas
    -> FROM aluno a
    ->        LEFT JOIN resposta r ON r.aluno_id = a.id
    ->        LEFT JOIN matricula m ON m.aluno_id = a.id
    -> GROUP BY a.nome;
+------------------+---------------+----------------+
| nome             | qtd_respostas | qtd_matriculas |
+------------------+---------------+----------------+
| Alberto Santos   |            18 |             18 |
| Carlos Cunha     |             0 |              0 |
| Claudio Soares   |             0 |              0 |
| Cristaldo Santos |             0 |              0 |
| Danilo Cunha     |             0 |              0 |
| Frederico José   |            12 |             12 |
| João da Silva    |            14 |             14 |
| Jose da Silva    |             0 |              0 |
| Manoel Santos    |             0 |              2 |
| Osmir Ferreira   |             0 |              0 |
| Paula Soares     |             0 |              1 |
| Paulo da Silva   |             0 |              0 |
| Paulo José       |             0 |              1 |
| Renata Alonso    |            14 |             14 |
| Renata Ferreira  |             0 |              1 |
| Zilmira José     |             0 |              0 |
+------------------+---------------+----------------+
16 rows in set (0.00 sec)
```


#### resumo

Neste capítulo aprendemos como utilizar os diferentes tipos de JOIN s, como por exemplo o LEFT
JOIN que retorna os registros da tabela a esquerda e o RIGHT JOIN que retorna os da direita mesmo
que não tenham associações. Vimos também que o JOIN também é conhecido como INNER JOIN que
retorna apenas os registros que estão associados. Além disso, vimos que algumas queries podem ser
resolvidas utilizando subqueries ou LEFT/RIGHT JOIN , porém é importante lembrar que os SGBDs
sempre terão melhor desempenho com o os JOIN s, por isso é recomendado que utilize os JOIN s.
Vamos para os exercícios?

[Voltar ao Índice](#indice)

---
## <a name="parte13">MUITOS ALUNOS E O LIMIT</a>

Precisamos de um relatório que retorne todos os alunos, algo como selecionar o nome de todos eles, com
um SELECT simples:

```sql
MariaDB [escola]> SELECT a.nome
    -> FROM aluno a
    -> LIMIT 5;
+----------------+
| nome           |
+----------------+
| João da Silva  |
| Frederico José |
| Alberto Santos |
| Renata Alonso  |
| Paulo da Silva |
+----------------+
5 rows in set (0.00 sec)
```

### 13.1 LIMITANDO E BUSCANDO A PARTIR DE UMA QUANTIDADE ESPECÍFICA

Agora vamos pegar os próximos 5 alunos, isto é, senhor MySQL, ignore os 5 primeiros, e depois
pegue para mim os próximos 5:

```sql
MariaDB [escola]> SELECT a.nome
    -> FROM aluno a
    -> LIMIT 5,5;
+-----------------+
| nome            |
+-----------------+
| Carlos Cunha    |
| Paulo José      |
| Manoel Santos   |
| Renata Ferreira |
| Paula Soares    |
+-----------------+
5 rows in set (0.00 sec)

```

LIMIT 5 ? LIMIT 5,5 ? Parece um pouco estranho, o que será que isso significa? Quando
utilizamos o LIMIT funciona da seguinte maneira: LIMIT
linha_inicial,qtd_de_linhas_para_avançar , ou seja, quando fizemos LIMIT 5 , informamos ao
MySQL que avançe 5 linhas apenas, pois por padrão ele iniciará pela primeira linha, chamada de linha
0 . Se fizéssemos LIMIT 0,5 , por exemplo:

```sql
MariaDB [escola]> SELECT a.nome FROM aluno a
    -> ORDER BY a.nome
    -> LIMIT 0,5;
+------------------+
| nome             |
+------------------+
| Alberto Santos   |
| Carlos Cunha     |
| Claudio Soares   |
| Cristaldo Santos |
| Danilo Cunha     |
+------------------+
5 rows in set (0.00 sec)

```


[Voltar ao Índice](#indice)

---
