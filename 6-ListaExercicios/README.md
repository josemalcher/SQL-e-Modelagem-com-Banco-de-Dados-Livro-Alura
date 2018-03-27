# Lista 6 de Exercício

1. Crie a tabela compradores com id , nome , endereco e telefone .
2. Insira os compradores, Guilherme e João da Silva.
3. Adicione a coluna id_compradores na tabela compras e defina a chave estrangeira (FOREIGN KEY) referenciando o id da tabela compradores .
4. Atualize a tabela compras e insira o id dos compradores na coluna id_compradores .
5. Exiba o NOME do comprador e o VALOR de todas as compras feitas antes de 09/08/2014.
6. Exiba todas as compras do comprador que possui ID igual a 2.
7. Exiba todas as compras (mas sem os dados do comprador), cujo comprador tenha nome que começa com 'GUILHERME'.
8. Exiba o nome do comprador e a soma de todas as suas compras.
9. A tabela compras foi alterada para conter uma FOREIGN KEY referenciando a coluna id da tabela compradores . O objetivo é deixar claro para o banco de dados que compras.id_compradores está de alguma forma relacionado com a tabela compradores através da coluna compradores.id . Mesmo sem criar a FOREIGN KEY é possível relacionar tabelas através do comando JOIN.
10. Qual a vantagem em utilizar a FOREIGN KEY ?
11. Crie uma coluna chamada "forma_pagto" do tipo ENUM e defina os valores: 'BOLETO' e 'CREDITO'.
12. Ative o strict mode na sessão que está utilizando para impossibilitar valores inválidos. Utilize o modo "STRICT_ALL_TABLES". E verifique se o SQL mode foi alterado fazendo um SELECT na sessão.
13. Tente inserir uma compra com forma de pagamento diferente de 'BOLETO' ou 'CREDITO', por exemplo, 'DINHEIRO' e verifique se o MySQL recusa a inserção.
14. Adicione as formas de pagamento para todas as compras por meio da instrução UPDATE .
15. Faça a configuração global do MySQL para que ele sempre entre no strict mode.

---

1. Crie a tabela compradores com id , nome , endereco e telefone .

```sql
CREATE TABLE compradores (
  id       INT PRIMARY KEY AUTO_INCREMENT NOT NULL,
  nome     VARCHAR(255),
  endereco VARCHAR(255),
  telefone VARCHAR(255)
);

```

2. Insira os compradores, Guilherme e João da Silva.

```sql
INSERT INTO compradores (nome, endereco, telefone) VALUES ('GUILHERME', 'Rua tal tal tal', '91887722');
INSERT INTO compradores (nome, endereco, telefone) VALUES ("Jão da Silva", 'Rua almirante tal', '81882233');

```

3. Adicione a coluna id_compradores na tabela compras e defina a chave estrangeira (FOREIGN KEY) referenciando o id da tabela compradores .

```sql
ALTER TABLE compras ADD COLUMN id_comprador INT;
ALTER TABLE compras ADD CONSTRAINT fk_compradores;
FOREIGN KEY (id_comprador) REFERENCES compradores(id);
```

4. Atualize a tabela compras e insira o id dos compradores na coluna id_compradores .

```sql
UPDATE compras SET compras.id_comprador = 2 WHERE id = 2;
```

5. Exiba o NOME do comprador e o VALOR de todas as compras feitas antes de 09/08/2014.

```sql

SELECT
  nome,
  data
FROM compradores a, compras c
WHERE a.id = c.id_comprador AND c.data < '2014-08-09';

SELECT a.nome, c.data
FROM compradores a
JOIN compras c ON a.id = c.id_comprador
WHERE c.data < '2014-08-09';

```

6. Exiba todas as compras do comprador que possui ID igual a 2.

```sql
SELECT
  c.valor,
  c.observacoes,
  c.data,
  e.nome
FROM compras c
  JOIN compradores e ON e.id = c.id_comprador
WHERE e.id = 2;
```

7. Exiba todas as compras (mas sem os dados do comprador), cujo comprador tenha nome que começa com 'JOSE'.

```sql

SELECT a.id, a.observacoes,a.valor, a.data, c.nome
FROM compras a
JOIN compradores c ON c.id = a.id_comprador
WHERE c.nome LIKE '%jose%';
```

8. Exiba o nome do comprador e a soma de todas as suas compras.

```sql
SELECT e.nome, SUM(c.valor) as Total FROM compras c
JOIN compradores e ON e.id = c.id_comprador
GROUP BY e.id;
```

9. A tabela compras foi alterada para conter uma FOREIGN KEY referenciando a coluna id da tabela compradores . O objetivo é deixar claro para o banco de dados que compras.id_compradores está de alguma forma relacionado com a tabela compradores através da coluna compradores.id . Mesmo sem criar a FOREIGN KEY é possível relacionar tabelas através do comando JOIN.

```sql
//Sim!  
```

10. Qual a vantagem em utilizar a FOREIGN KEY ?

- https://pt.stackoverflow.com/questions/106084/qual-a-utilidade-de-usar-chaves-estrangeiras

"Além de ajudar a descrever o relacionamento nos modelos, as chaves estrangeiras são usados principalmente pra manter a integridade dos dados, ou seja imagine que você tem duas tabelas ligadas por uma chave estrangeira e tem dados na tabela B ligados a uma especifica linha na tabela A, então você, se você tentar deletar esta linha especifica o banco vai lhe impedir e vai enviar um erro."

```sql

```
11. Crie uma coluna chamada "forma_pagto" do tipo ENUM e defina os valores: 'BOLETO' e 'CREDITO'.

```sql
ALTER TABLE compras ADD COLUMN forma_pagamento ENUM('BOLETO','CREDITO');
```

12. Ative o strict mode na sessão que está utilizando para impossibilitar valores inválidos. Utilize o modo "STRICT_ALL_TABLES". E verifique se o SQL mode foi alterado fazendo um SELECT na sessão.

```sql
SET SESSION sql_mode = 'STRICT_ALL_TABLES';
```

13. Tente inserir uma compra com forma de pagamento diferente de 'BOLETO' ou 'CREDITO', por exemplo, 'DINHEIRO' e verifique se o MySQL recusa a inserção.

```sql
INSERT INTO compras (valor, data, observacoes, id_comprador, forma_pagamento)
VALUES (1400, '2018-01-06', 'CELULAR', 1, 'DINHEIRO');

-- ERROR 1265 (01000): Data truncated for column 'forma_pagamento' at row 1
```

14. Adicione as formas de pagamento para todas as compras por meio da instrução UPDATE .

```sql
```

15. Faça a configuração global do MySQL para que ele sempre entre no strict mode.

```sql
SET GLOBAL sql_mode = 'STRICT_ALL_TABLES';
```
