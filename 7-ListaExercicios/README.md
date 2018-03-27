# Lista 7 Exercícios

1. Busque todos os alunos que não tenham nenhuma matrícula nos cursos.
2. Busque todos os alunos que não tiveram nenhuma matrícula nos últimos 45 dias, usando a instrução EXISTS .
3. É possível fazer a mesma consulta sem usar EXISTS ? Quais são?

---


1. Busque todos os alunos que não tenham nenhuma matrícula nos cursos.

```sql
SELECT a.nome
FROM aluno a
WHERE NOT EXISTS(SELECT m.id
                 FROM matricula m
                 WHERE m.aluno_id = a.id);
```

2. Busque todos os alunos que não tiveram nenhuma matrícula nos últimos 45 dias, usando a instrução EXISTS .

```sql
SELECT a.nome
FROM aluno a
WHERE NOT EXISTS(SELECT m.id
                 FROM matricula m
                 WHERE m.aluno_id = a.id
                  AND data < '2015-12-12'
                  );
```

3. É possível fazer a mesma consulta sem usar EXISTS ? Quais são?

-sim: https://forum.imasters.com.br/topic/224193-resolvido-exemplos-do-uso-do-not-exists/

```sql
```
