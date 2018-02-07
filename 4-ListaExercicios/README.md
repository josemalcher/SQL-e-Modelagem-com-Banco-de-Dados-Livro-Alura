# Lista 4 Exercícios

1. Configure o valor padrão para a coluna recebida .
2. Configure a coluna observacoes para não aceitar valores nulos.
3. No nosso modelo atual, qual campo você deixaria com valores DEFAULT e quais não? Justifique sua decisão. Note que como estamos falando de modelagem, não existe uma regra absoluta, existe vantagens e desvantagens na decisão que tomar, tente citá-las.
4. NOT NULL e DEFAULT podem ser usados também no CREATE TABLE ? Crie uma tabela nova e adicione Constraints e valores DAFAULT .
5. Reescreva o CREATE TABLE do começo do curso, marcando observacoes como nulo e valor padrão do recebida como 1.

---

1. Configure o valor padrão para a coluna recebida .

```sql
 DESC compras;
+-------------+---------------+------+-----+---------+----------------+
| Field       | Type          | Null | Key | Default | Extra          |
+-------------+---------------+------+-----+---------+----------------+
| id          | int(11)       | NO   | PRI | NULL    | auto_increment |
| valor       | decimal(18,2) | YES  |     | NULL    |                |
| data        | date          | YES  |     | NULL    |                |
| observacoes | varchar(255)  | NO   |     | NULL    |                |
| recebida    | tinyint(1)    | YES  |     | 1       |                |
+-------------+---------------+------+-----+---------+----------------+
5 rows in set (0.04 sec)

ALTER TABLE compras 
MODIFY COLUMN recebida tinyint(1) DEFAULT 0;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

DESC compras;
+-------------+---------------+------+-----+---------+----------------+
| Field       | Type          | Null | Key | Default | Extra          |
+-------------+---------------+------+-----+---------+----------------+
| id          | int(11)       | NO   | PRI | NULL    | auto_increment |
| valor       | decimal(18,2) | YES  |     | NULL    |                |
| data        | date          | YES  |     | NULL    |                |
| observacoes | varchar(255)  | NO   |     | NULL    |                |
| recebida    | tinyint(1)    | YES  |     | 0       |                |
+-------------+---------------+------+-----+---------+----------------+
5 rows in set (0.02 sec)

```

2. Configure a coluna observacoes e valor para não aceitar valores nulos.

```sql
ALTER TABLE compras MODIFY COLUMN valor varchar(255) NOT NULL;
Query OK, 43 rows affected (0.22 sec)
Records: 43  Duplicates: 0  Warnings: 0

MariaDB [controledegastos]> DESC compras;
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
```

3. No nosso modelo atual, qual campo você deixaria com valores DEFAULT e quais não? Justifique sua decisão. Note que como estamos falando de modelagem, não existe uma regra absoluta, existe vantagens e desvantagens na decisão que tomar, tente citá-las.

```sql
-- Apenas o campo Recebida, pois o cadastro por padrão pode receber, logo no casdastro o valor 0(FALSE) de não recebido.
```

4. NOT NULL e DEFAULT podem ser usados também no CREATE TABLE ? Crie uma tabela nova e adicione Constraints e valores DAFAULT .

```sql
 CREATE TABLE compras2(id INT NOT NULL AUTO_INCREMENT PRIMARY KEY, 
                       valor DECIMAL(18,2) NOT NULL DEFAULT 0.0, 
                       data DATE NOT NULL, 
                       nomeProduto VARCHAR(255) NOT NULL, 
                       recebido TINYINT DEFAULT 0);

DESC compras2;
+-------------+---------------+------+-----+---------+----------------+
| Field       | Type          | Null | Key | Default | Extra          |
+-------------+---------------+------+-----+---------+----------------+
| id          | int(11)       | NO   | PRI | NULL    | auto_increment |
| valor       | decimal(18,2) | NO   |     | 0.00    |                |
| data        | date          | NO   |     | NULL    |                |
| nomeProduto | varchar(255)  | NO   |     | NULL    |                |
| recebido    | tinyint(4)    | YES  |     | 0       |                |
+-------------+---------------+------+-----+---------+----------------+
5 rows in set (0.01 sec)                       
```

5. Reescreva o CREATE TABLE do começo do curso, marcando observacoes como nulo e valor padrão do recebida como 1.

```sql
CREATE TABLE compras(id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
                     valor DECIMAL(18,2) DEFAULT 0.0 NOT NULL,
                     data DATE NOT NULL,
                     observacoes VARCHAR(255), 
                     recebida TINYINT DEFAULT 1 NOT NULL;

```

