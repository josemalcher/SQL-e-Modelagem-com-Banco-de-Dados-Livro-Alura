# lista 08

1. Exiba a média das notas por curso.

2. Devolva o curso e as médias de notas, levando em conta somente alunos que tenham "Silva" ou "Santos" no sobrenome.

3. Conte a quantidade de respostas por exercício. Exiba a pergunta e o número de respostas.

4. Você pode ordenar pelo COUNT também. Basta colocar ORDER BY COUNT(coluna).Pegue a resposta do exercício anterior, e ordene por número de respostas, em ordem decrescente.

    1. Podemos agrupar por mais de um campo de uma só vez. Por exemplo, se quisermos a média de notas por aluno por curso, podemos fazer GROUP BY aluno.id, curso.id.
    
---

1. Exiba a média das notas por curso.

```sql
MariaDB [escola]> SELECT c.nome,
    ->        AVG(n.nota) AS MEDIA
    -> FROM nota n
    -> JOIN resposta r ON n.resposta_id = r.id
    -> JOIN exercicio e ON r.exercicio_id = e.id
    -> JOIN secao s ON e.secao_id = s.id
    -> JOIN curso c ON s.curso_id = c.id
    -> GROUP BY c.nome;
+---------------------------------+----------+
| nome                            | MEDIA    |
+---------------------------------+----------+
| C# e orientação a objetos       | 4.857143 |
| Desenvolvimento web com VRaptor | 8.000000 |
| Scrum e métodos ágeis           | 5.777778 |
| SQL e banco de dados            | 6.100000 |
+---------------------------------+----------+
4 rows in set (0.00 sec)
```

2. Devolva o curso e as médias de notas, levando em conta somente alunos que tenham "Silva" ou "Santos" no sobrenome.

```sql
MariaDB [escola]> SELECT c.nome,a.nome,
    ->        AVG(n.nota) AS MEDIA
    -> FROM nota n
    -> JOIN resposta r ON n.resposta_id = r.id
    -> JOIN exercicio e ON r.exercicio_id = e.id
    -> JOIN secao s ON e.secao_id = s.id
    -> JOIN curso c ON s.curso_id = c.id
    -> JOIN matricula m ON c.id = m.curso_id
    -> JOIN aluno a ON m.aluno_id = a.id
    -> WHERE a.nome  LIKE '%silva%' OR a.nome LIKE '%santos%'
    -> GROUP BY c.nome, a.nome;
+---------------------------+----------------+----------+
| nome                      | nome           | MEDIA    |
+---------------------------+----------------+----------+
| C# e orientação a objetos | Alberto Santos | 4.857143 |
| C# e orientação a objetos | João da Silva  | 4.857143 |
| Scrum e métodos ágeis     | Alberto Santos | 5.777778 |
| Scrum e métodos ágeis     | Manoel Santos  | 5.777778 |
| SQL e banco de dados      | João da Silva  | 6.100000 |
| SQL e banco de dados      | Manoel Santos  | 6.100000 |
+---------------------------+----------------+----------+
6 rows in set (0.00 sec)

```

3. Conte a quantidade de respostas por exercício. Exiba a pergunta e o número de respostas.

```sql
MariaDB [escola]> SELECT e.pergunta, COUNT(r.id) AS 'Qnt Respostas'
    -> FROM resposta r
    ->        JOIN exercicio e ON r.exercicio_id = r.id
    -> GROUP BY e.pergunta;
+------------------------------+---------------+
| pergunta                     | Qnt Respostas |
+------------------------------+---------------+
| como faz?                    |             7 |
| Como funciona a web?         |            14 |
| Como funciona um select?     |             7 |
| como funciona?               |             7 |
| Cuidados com ele?            |             7 |
| Frameworks que usam?         |             7 |
| java funciona?               |             7 |
| O que é a classe Result?     |             7 |
| O que é um delete?           |             7 |
| O que é um interceptor?      |             7 |
| O que é um select?           |             7 |
| O que é um update?           |             7 |
| o que eh a reuniao diaria?   |             7 |
| o que eh a web?              |             7 |
| o que eh deploy?             |             7 |
| o que eh iteracao            |             7 |
| o que eh kanban?             |             7 |
| O que eh MVC?                |             7 |
| o que eh mysql               |             7 |
| o que eh o apache?           |             7 |
| o que eh um insert?          |             7 |
| o que eh xp?                 |             7 |
| o que sao retrospectiva?     |             7 |
| Perigos do update?           |             7 |
| qual tamanho bom?            |             7 |
| quando devemos fazer?        |             7 |
| quando fazemos?              |             7 |
| quando usar?                 |             7 |
| Que linguagens posso ajudar? |             7 |
| tem outros?                  |             7 |
+------------------------------+---------------+
30 rows in set (0.00 sec)

```

4. Você pode ordenar pelo COUNT também. Basta colocar ORDER BY COUNT(coluna).Pegue a resposta do exercício anterior, e ordene por número de respostas, em ordem decrescente.

    1. Podemos agrupar por mais de um campo de uma só vez. Por exemplo, se quisermos a média de notas por aluno por curso, podemos fazer GROUP BY aluno.id, curso.id.
```sql
MariaDB [escola]> SELECT e.pergunta, COUNT(r.id) AS 'Qnt Respostas'
    -> FROM resposta r
    ->        JOIN exercicio e ON r.exercicio_id = r.id
    -> GROUP BY e.pergunta
    -> ORDER BY COUNT(pergunta);
+------------------------------+---------------+
| pergunta                     | Qnt Respostas |
+------------------------------+---------------+
| Cuidados com ele?            |             7 |
| quando devemos fazer?        |             7 |
| o que eh um insert?          |             7 |
| o que eh a reuniao diaria?   |             7 |
| como funciona?               |             7 |
| quando fazemos?              |             7 |
| o que eh kanban?             |             7 |
| Que linguagens posso ajudar? |             7 |
| o que eh xp?                 |             7 |
| O que eh MVC?                |             7 |
| tem outros?                  |             7 |
| Frameworks que usam?         |             7 |
| o que eh a web?              |             7 |
| O que é a classe Result?     |             7 |
| o que eh o apache?           |             7 |
| O que é um select?           |             7 |
| O que é um interceptor?      |             7 |
| java funciona?               |             7 |
| Como funciona um select?     |             7 |
| quando usar?                 |             7 |
| o que eh mysql               |             7 |
| O que é um update?           |             7 |
| o que eh iteracao            |             7 |
| o que eh deploy?             |             7 |
| Perigos do update?           |             7 |
| qual tamanho bom?            |             7 |
| como faz?                    |             7 |
| O que é um delete?           |             7 |
| o que sao retrospectiva?     |             7 |
| Como funciona a web?         |            14 |
+------------------------------+---------------+
30 rows in set (0.00 sec)


```