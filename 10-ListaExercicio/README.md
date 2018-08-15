# lista 10

1. Exiba todos os tipos de matrícula que existem na tabela. Use DISTINCT para que não haja repetição.

2. Exiba todos os cursos e a sua quantidade de matrículas. Mas filtre por matrículas dos tipos PF ou PJ.

3. Traga todas as perguntas e a quantidade de respostas de cada uma. Mas dessa vez, somente dos cursos com ID 1 e 3.


---

- Diferença entre DISTINCT e GROUP By: https://pt.stackoverflow.com/a/229072/2167

1. Exiba todos os tipos de matrícula que existem na tabela. Use DISTINCT para que não haja repetição.

```sql
SELECT DISTINCT m.tipo
FROM matricula m;
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

2. Exiba todos os cursos e a sua quantidade de matrículas. Mas filtre por matrículas dos tipos PF ou PJ.

```sql
SELECT c.nome, COUNT(m.id) AS Matriculas, m.tipo
FROM matricula m
JOIN curso c ON m.curso_id = c.id
WHERE m.tipo = 'PAGA_PJ' OR m.tipo = 'PAGA_PF'
GROUP BY c.nome, m.tipo;

SELECT c.nome, COUNT(m.id) AS Matriculas, m.tipo
FROM matricula m
JOIN curso c ON m.curso_id = c.id
WHERE m.tipo IN ('PAGA_PJ', 'PAGA_PF')
GROUP BY c.nome, m.tipo;

+------------------------------------+------------+---------+
| nome                               | Matriculas | tipo    |
+------------------------------------+------------+---------+
| C# e orientação a objetos          |          1 | PAGA_PF |
| C# e orientação a objetos          |          1 | PAGA_PJ |
| Desenvolvimento mobile com Android |          2 | PAGA_PJ |
| Desenvolvimento web com VRaptor    |          1 | PAGA_PF |
| Desenvolvimento web com VRaptor    |          1 | PAGA_PJ |
| Scrum e métodos ágeis              |          1 | PAGA_PF |
| Scrum e métodos ágeis              |          1 | PAGA_PJ |
| SQL e banco de dados               |          1 | PAGA_PF |
| SQL e banco de dados               |          1 | PAGA_PJ |
+------------------------------------+------------+---------+
9 rows in set (0.00 sec)


```

3. Traga todas as perguntas e a quantidade de respostas de cada uma. Mas dessa vez, somente dos cursos com ID 1 e 3.

```sql
SELECT e.pergunta, e.resposta_oficial, r.resposta_dada, c.id, c.nome
FROM resposta r
JOIN exercicio e ON r.exercicio_id = e.id
JOIN secao s ON e.secao_id = s.id
JOIN curso c ON s.curso_id = c.id
WHERE c.id IN (1,3);
+----------------------------+--------------------------------------------------------------------------+--------------------------------+----+-----------------------+
| pergunta                   | resposta_oficial                                                         | resposta_dada                  | id | nome                  |
+----------------------------+--------------------------------------------------------------------------+--------------------------------+----+-----------------------+
| O que é um select?         | Uma consulta que devolve resultados                                      | uma selecao                    |  1 | SQL e banco de dados  |
| Como funciona um select?   | select campos from tabela                                                | ixi, nao sei                   |  1 | SQL e banco de dados  |
| O que é um update?         | serve pra alterar dados                                                  | alterar dados                  |  1 | SQL e banco de dados  |
| Perigos do update?         | Nao pode esquecer de colocar where                                       | eskecer o where e alterar tudo |  1 | SQL e banco de dados  |
| O que é um delete?         | deleta uma linha do banco de dados                                       | apagar coisas                  |  1 | SQL e banco de dados  |
| Cuidados com ele?          | nao pode esquecer do where                                               | tb nao pode eskecer o where    |  1 | SQL e banco de dados  |
| o que eh um insert?        | serve para inserir um dado no banco                                      | inserir dados                  |  1 | SQL e banco de dados  |
| O que é um select?         | Uma consulta que devolve resultados                                      | buscar dados                   |  1 | SQL e banco de dados  |
| Como funciona um select?   | select campos from tabela                                                | select campos from tabela      |  1 | SQL e banco de dados  |
| Perigos do update?         | Nao pode esquecer de colocar where                                       | ixi, nao sei                   |  1 | SQL e banco de dados  |
| o que eh iteracao          | tempo que vc tem pra agregar valor                                       | tempo pra fazer algo           |  3 | Scrum e métodos ágeis |
| qual tamanho bom?          | de 2 a 4 semanas, segundo o scrum guide antigo                           | 1 a 4 semanas                  |  3 | Scrum e métodos ágeis |
| o que sao retrospectiva?   | reunioes onde a ideia eh melhorar o processo                             | melhoria do processo           |  3 | Scrum e métodos ágeis |
| quando devemos fazer?      | geralmente a cada iteracao                                               | todo dia                       |  3 | Scrum e métodos ágeis |
| o que eh a reuniao diaria? | uma pequena reuniao para informar a equipe sobre o andamento da iteracao | reuniao de status              |  3 | Scrum e métodos ágeis |
| quando fazemos?            | uma vez por dia, em um horario fixo e pre definido                       | todo dia                       |  3 | Scrum e métodos ágeis |
| o que eh kanban?           | um metodo agil tb                                                        | o quadro branco                |  3 | Scrum e métodos ágeis |
| o que eh xp?               | outro metodo agil                                                        | um metodo agil                 |  3 | Scrum e métodos ágeis |
| tem outros?                | lean, crystal, fdd                                                       | tem varios outros              |  3 | Scrum e métodos ágeis |
+----------------------------+--------------------------------------------------------------------------+--------------------------------+----+-----------------------+
19 rows in set (0.00 sec)


```

