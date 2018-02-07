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
```

2. Insira os compradores, Guilherme e João da Silva.

```sql
```

3. Adicione a coluna id_compradores na tabela compras e defina a chave estrangeira (FOREIGN KEY) referenciando o id da tabela compradores .

```sql
```

4. Atualize a tabela compras e insira o id dos compradores na coluna id_compradores .

```sql
```

5. Exiba o NOME do comprador e o VALOR de todas as compras feitas antes de 09/08/2014.

```sql
```

6. Exiba todas as compras do comprador que possui ID igual a 2.

```sql
```

7. Exiba todas as compras (mas sem os dados do comprador), cujo comprador tenha nome que começa com 'GUILHERME'.

```sql
```

8. Exiba o nome do comprador e a soma de todas as suas compras.

```sql
```

9. A tabela compras foi alterada para conter uma FOREIGN KEY referenciando a coluna id da tabela compradores . O objetivo é deixar claro para o banco de dados que compras.id_compradores está de alguma forma relacionado com a tabela compradores através da coluna compradores.id . Mesmo sem criar a FOREIGN KEY é possível relacionar tabelas através do comando JOIN.

```sql
```

10. Qual a vantagem em utilizar a FOREIGN KEY ?


```sql
```
11. Crie uma coluna chamada "forma_pagto" do tipo ENUM e defina os valores: 'BOLETO' e 'CREDITO'.

```sql
```

12. Ative o strict mode na sessão que está utilizando para impossibilitar valores inválidos. Utilize o modo "STRICT_ALL_TABLES". E verifique se o SQL mode foi alterado fazendo um SELECT na sessão.

```sql
```

13. Tente inserir uma compra com forma de pagamento diferente de 'BOLETO' ou 'CREDITO', por exemplo, 'DINHEIRO' e verifique se o MySQL recusa a inserção.

```sql
```

14. Adicione as formas de pagamento para todas as compras por meio da instrução UPDATE .

```sql
```

15. Faça a configuração global do MySQL para que ele sempre entre no strict mode.

```sql
```
