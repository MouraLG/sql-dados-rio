# Semana 1 — SQL

## Terça 27/10
Conteúdo: SQLBolt lições 1–4 + REVIEW(5), Kaggle Intro to SQL lições 1–2, LeetCode SQL 50 seção Select.

Revisados conteúdos básicos de SQL que não via há um tempo, sintaxe, queries, restrições, filtragem e ordenações.

Introduzido ao BigQuery em Python e algumas APIs para realizar essas queries.

Dei umas trupicadas na sintaxe do BigQuery e das condições e reaprendi que o "IS/IS NOT NULL" é utilizado porque 
comparações com NULL sempre retornam desconhecido, reaprendi a apelidagem (aliasing) da query (SELECT .. AS)


## Quarta 28/10
Conteúdo: SQLBolt lições 6–12, Kaggle Intro to SQL lição 3

Revisados queries multi-tabelas com JOINS (principalmente INNER e LEFT JOIN), uma breve passada sobre operações 
com valores NULL (também vistas ontem), queries com expressões no SELECT, expressões agregadas (funções 
COUNT, MIN, MAX, AVG, SUM) e ordem de execução das queries: 
FROM (e JOINS) -> WHERE -> GROUP BY -> HAVING -> SELECT -> ORDER BY -> LIMIT/OFFSET

GROUP BY agrupa linhas; HAVING filtra grupos depois da agregação, enquanto o WHERE filtra linhas antes.

A lição 3 do Kaggle tocou nos mesmo assuntos, mas trouxe um ponto importante: se a tabela possuir uma coluna chamada "by" 
(ou qualquer outra palavra reservada do SQL), será necessário "escapar" o nome, e isso é feito de diferentes maneiras, por ex.:
BigQuery, MySQL, Spark SQL (Databricks)	        \`by\` (crases)
PostgreSQL, SQLite (padrão SQL)	                "by" (aspas duplas)
SQL Server / T-SQL (Fabric)	                    [by] (colchetes) ou "by"

Consideração: Por que você não pode usar um apelido (alias) criado no SELECT dentro do WHERE da mesma consulta?
Basicamente porque o SELECT entra depois na ordem de execução, então o WHERE ainda não consegue reconhecer esses aliases.
apenas o ORDER BY tem essa capacidade, pois entra depois.
Nota: GROUP BY não aceita aliases pela lógica SQL padrão, mas alguns bancos (MySQL, PostgreSQL e BigQuery) fazem exceção
por conveniência. SQL Server / T-SQL (Warehouse do Fabric) não aceita; Spark SQL (Databricks) aceita. Na dúvida, repetir a expressão.


## Quinta 29/10
Conteúdo: Kaggle Intro to SQL lições 4 e 5, LeetCode SQL 50 seção Basic Aggregate Functions (4 problemas).

Kaggle 4: ORDER BY e datas. `EXTRACT(YEAR/MONTH/DAY FROM coluna)` tira ano, mês ou dia de uma data.

Kaggle 5: apelidos com AS e CTEs com WITH. Uma CTE é uma "tabela temporária com nome" que divide a consulta em etapas. 
Várias CTEs: um único WITH, separadas por vírgula.

LeetCode:
- Not Boring Movies: `id % 2 = 1` (ou `MOD(id, 2) = 1`) para ímpares.
- Average Selling Price: o "easy" mais difícil até agora.
  - Condição extra no ON: `ON p.id = u.id AND data BETWEEN inicio AND fim`
  - LEFT JOIN + `COALESCE(valor, 0)` para manter produtos sem venda.
  - Média ponderada = `SUM(preco * unidades) / SUM(unidades)`. `AVG(preco * unidades)` dá receita média por venda, não preço por unidade.
- Project Employees I: esqueci o GROUP BY. Toda coluna fora de agregação no SELECT precisa estar no GROUP BY.
- Percentage of Users Attended a Contest:
  - Depois do GROUP BY, cada agregação só enxerga as linhas do grupo. Por isso as duas contagens davam 100%.
  - Total geral vem de fora: `(SELECT COUNT(*) FROM Users)`.
  - Alternativas: CTE + `CROSS JOIN`, ou `JOIN ... ON TRUE` (equivale ao CROSS JOIN). Aqui a subconsulta é a mais simples.

Lembrete de estilo: aspas simples para textos; evitar `SELECT *` (no BigQuery se paga pelo volume lido).

## Sexta 38/10
Conteúdo: Kaggle lição 6 (final), LeetCode SQL 50 seçao Basic Joins (7 problemas).

LeetCode:
- Self join: a mesma tabela com dois apelidos (ex: hoje / ontem).
- Diferença de datas varia: MySQL `DATEDIFF(a, b)`, BigQuery `DATE_DIFF(a, b, DAY)`, SQL Server `DATEDIFF(day, b, a)`, PostgreSQL `a - b`.
- Evitar `current` como apelido (palavra-chave).
- Self join separando start e end; casar também o `process_id`.
- No INNER JOIN, filtro no ON ou no WHERE dá o mesmo resultado. No LEFT JOIN, não: no WHERE ele pode descartar linhas da esquerda.
- Padrão "contar incluindo zeros": CROSS JOIN (todas as combinações) → LEFT JOIN (fatos) → `COUNT(coluna_da_direita)`.
- `COUNT(*)` conta a linha mesmo com NULLs; `COUNT(coluna)` ignora NULL.
- Ordem dos JOINs importa: aplicados de cima para baixo.
- CROSS JOIN não leva ON.
