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
BigQuery, MySQL, Spark SQL (Databricks)	        `` `by` `` (crases)
PostgreSQL, SQLite (padrão SQL)	                "by" (aspas duplas)
SQL Server / T-SQL (Fabric)	                    [by] (colchetes) ou "by"

Consideração: Por que você não pode usar um apelido (alias) criado no SELECT dentro do WHERE da mesma consulta?
Basicamente porque o SELECT entra depois na ordem de execução, então o WHERE ainda não consegue reconhecer esses aliases.
apenas o ORDER BY tem essa capacidade, pois entra depois.
Nota: GROUP BY não aceita aliases pela lógica SQL padrão, mas alguns bancos (MySQL, PostgreSQL e BigQuery) fazem exceção
por conveniência. SQL Server / T-SQL (Warehouse do Fabric) não aceita; Spark SQL (Databricks) aceita. Na dúvida, repetir a expressão.
