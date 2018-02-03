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

```

2. Qual o comando SQL para juntar duas condições diferentes? Por exemplo, SELECT * FROM TABELA WHERE campo > 1000 campo < 5000. Faça o teste e veja o resultado.  

```sql

```

3. Vimos que todo texto é passado através de aspas simples ('). Posso passar aspas duplas (") no lugar?  

```sql

```

4. Selecione todas as compras cuja data seja maior-ou-igual que 15/12/2012 e menor do que 15/12/2014.  

```sql

```

5. Selecione todas as compras cujo valor esteja entre R$15,00 e R$35,00 e a observação comece com a palavra 'Lanchonete'.  

```sql

```

6. Selecione todas as compras que já foram recebidas.  

```sql

```

7. Selecione todas as compras que ainda não foram recebidas.  

```sql

```

8. Vimos que para guardar o valor VERDADEIRO para a coluna recebida , devemos passar o valor 1.  
```
Para FALSO, devemos passar o valor 0. E quanto as palavras já conhecidas para verdadeiro e falso:
TRUE e FALSE . Elas funcionam? Ou seja:
INSERT INTO compras (valor, data, observacoes, recebida) VALUES (100.0, '2015-09-08', 'COMIDA', TRUE)
;
```
Funciona? Faça o teste.  

```sql

```

9. Selecione todas as compras com valor maior que 5.000,00 ou que já foram recebidas.  

```sql

```

10. Selecione todas as compras que o valor esteja entre 1.000,00 e 3.000,00 ou seja maior que 5.000,00.  


```sql

```
