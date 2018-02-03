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






[Voltar ao Índice](#indice)

---

## <a name="parte3"></a>


[Voltar ao Índice](#indice)

---

## <a name="parte4"></a>


[Voltar ao Índice](#indice)

---

## <a name="parte5"></a>


[Voltar ao Índice](#indice)

---

## <a name="parte6"></a>


[Voltar ao Índice](#indice)

---

## <a name="parte7"></a>


[Voltar ao Índice](#indice)

---

## <a name="parte8"></a>


[Voltar ao Índice](#indice)

---

## <a name="parte9"></a>


[Voltar ao Índice](#indice)

---

## <a name="parte10"></a>


[Voltar ao Índice](#indice)

---

## <a name="parte11"></a>


[Voltar ao Índice](#indice)

---

## <a name="parte12"></a>


[Voltar ao Índice](#indice)

---