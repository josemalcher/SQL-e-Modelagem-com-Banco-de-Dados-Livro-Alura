# Lista 9

1. Qual é a principal diferença entre as instruções having e where do sql?


2. Devolva todos os alunos, cursos e a média de suas notas. Lembre-se de agrupar por aluno e por curso. Filtre também pela nota: só mostre alunos com nota média menor do que 5.


3. Exiba todos os cursos e a sua quantidade de matrículas. Mas, exiba somente cursos que tenham mais de 1 matrícula.


4. Exiba o nome do curso e a quantidade de seções que existe nele. Mostre só cursos com mais de 3 seções.

---

1. Qual é a principal diferença entre as instruções having e where do sql?

R - O WHERE funciona direta na linha, já o HAVING funciona em resultados de agregadores de linhas, o mais usado é com o GROUP BY.

2. Devolva todos os alunos, cursos e a média de suas notas. Lembre-se de agrupar por aluno e por curso. Filtre também pela nota: só mostre alunos com nota média menor do que 5.

```sql
SELECT a.nome, c.nome, AVG(n.nota) AS Media
FROM nota n
JOIN resposta r ON r.id = n.resposta_id
JOIN exercicio e ON r.exercicio_id = e.id
JOIN secao s ON e.secao_id = s.id
JOIN curso c ON s.curso_id = c.id
JOIN aluno a ON r.aluno_id = a.id
GROUP BY a.nome,c.nome
HAVING AVG(n.nota) < 5;
+---------------+---------------------------+----------+
| nome          | nome                      | Media    |
+---------------+---------------------------+----------+
| Renata Alonso | C# e orientação a objetos | 4.857143 |
+---------------+---------------------------+----------+
1 row in set (0.00 sec)

```

3. Exiba todos os cursos e a sua quantidade de matrículas. Mas, exiba somente cursos que tenham mais de 1 matrícula.

```sql
SELECT c.nome, count(a.nome) AS Matricula
    FROM matricula m
     JOIN curso c ON m.curso_id = c.id
    JOIN aluno a ON m.aluno_id = a.id
    GROUP BY c.nome
    HAVING COUNT(a.nome) > 1;
+------------------------------------+-----------+
| nome                               | Matricula |
+------------------------------------+-----------+
| C# e orientação a objetos          |         4 |
| Desenvolvimento mobile com Android |         2 |
| Desenvolvimento web com VRaptor    |         2 |
| Scrum e métodos ágeis              |         2 |
| SQL e banco de dados               |         4 |
+------------------------------------+-----------+
5 rows in set (0.00 sec)

MariaDB [escola]>

```

4. Exiba o nome do curso e a quantidade de seções que existe nele. Mostre só cursos com mais de 3 seções.

```sql
SELECT c.nome, COUNT(s.numero)
FROM secao s
JOIN curso c ON s.curso_id = c.id
GROUP BY c.nome
HAVING COUNT(s.numero) > 3;
+---------------------------------+-----------------+
| nome                            | COUNT(s.numero) |
+---------------------------------+-----------------+
| C# e orientação a objetos       |               4 |
| Desenvolvimento web com VRaptor |               4 |
| Scrum e métodos ágeis           |               4 |
| SQL e banco de dados            |               4 |
+---------------------------------+-----------------+
4 rows in set (0.00 sec)


```
