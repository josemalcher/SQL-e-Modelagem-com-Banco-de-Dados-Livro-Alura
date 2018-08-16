# Lista 11

1. Exiba a média das notas por aluno, além de uma coluna com a diferença entre a média do aluno e a média geral. Use sub-queries para isso.

2. Qual é o problema de se usar sub-queries?

3. Exiba a quantidade de matrículas por curso. Além disso, exiba a divisão entre matrículas naquele curso e matrículas totais.

---

1. Exiba a média das notas por aluno, além de uma coluna com a diferença entre a média do aluno e a média geral. Use sub-queries para isso.

```sql
SELECT a.nome,
c.nome,
AVG(n.nota) AS 'Média Aluno',
(SELECT AVG(n.nota) FROM nota n) AS 'Média Geral',
AVG(n.nota) - (SELECT AVG(n.nota) FROM nota n) as Diferença
FROM nota n
JOIN resposta r ON n.resposta_id = r.id
JOIN exercicio e ON r.exercicio_id = e.id
JOIN secao s ON e.secao_id = s.id
JOIN curso c ON s.curso_id = c.id
JOIN aluno a ON r.aluno_id = a.id
GROUP BY a.nome, c.nome;
+----------------+---------------------------------+-------------+-------------+-----------+
| nome           | nome                            | Média Aluno | Média Geral | Diferença |
+----------------+---------------------------------+-------------+-------------+-----------+
| Alberto Santos | Scrum e métodos ágeis           |    5.777778 |    5.740741 |  0.037037 |
| Frederico José | Desenvolvimento web com VRaptor |    8.000000 |    5.740741 |  2.259259 |
| Frederico José | SQL e banco de dados            |    5.666667 |    5.740741 | -0.074074 |
| João da Silva  | SQL e banco de dados            |    6.285714 |    5.740741 |  0.544974 |
| Renata Alonso  | C# e orientação a objetos       |    4.857143 |    5.740741 | -0.883598 |
+----------------+---------------------------------+-------------+-------------+-----------+
5 rows in set (0.00 sec)


```

2. Qual é o problema de se usar sub-queries?

- https://pt.stackoverflow.com/questions/138334/subqueries-podem-diminuir-a-performance-mito-ou-verdade


3. Exiba a quantidade de matrículas por curso. Além disso, exiba a divisão entre matrículas naquele curso e matrículas totais.

```sql
select c.nome,
count(m.id),
count(m.id)/(select count(id) from matricula m)
from curso c
join matricula m on m.curso_id = c.id
group by c.nome;
+------------------------------------+-------------+-------------------------------------------------+
| nome                               | count(m.id) | count(m.id)/(select count(id) from matricula m) |
+------------------------------------+-------------+-------------------------------------------------+
| C# e orientação a objetos          |           4 |                                          0.2857 |
| Desenvolvimento mobile com Android |           2 |                                          0.1429 |
| Desenvolvimento web com VRaptor    |           2 |                                          0.1429 |
| Scrum e métodos ágeis              |           2 |                                          0.1429 |
| SQL e banco de dados               |           4 |                                          0.2857 |
+------------------------------------+-------------+-------------------------------------------------+
5 rows in set (0.00 sec)

# https://cursos.alura.com.br/forum/topico-consultas-na-resposta-da-aula-6-62068
```
