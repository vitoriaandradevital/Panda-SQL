# Panda-SQL

O cronograma do grupo foi estruturado em 12 semanas de estudo, combinando teoria e prática utilizando o dataset Brazilian E-Commerce Public Dataset by Olist.

Cada semana aborda um conjunto de conceitos fundamentais de SQL e exercícios aplicados para fixação do conteúdo.

## Semana 1 – Introdução e Ambiente

#### O que estudar:

* O que é SQL e bancos de dados relacionais
* Instalação do PostgreSQL e DBeaver (Ou qualquer ambiente de SQL)
* Conectando ao banco
* Primeiro SELECT

#### Prática:

* Instalar os softwares
* Baixar o banco de dados do Kaggle Brazilian E-Commerce Public Dataset by Olist
* Importar as tabelas do dataset no ambiente
* Explorar as tabelas do banco para entender as colunas e os dados disponíveis

## Semana 2 – Consultas Básicas e Filtragem

#### O que estudar:

* SELECT, FROM, AS (alias)
* WHERE com operadores (=, <>, >, <, >=, <=)
* AND, OR, NOT
* LIKE, IN, BETWEEN

#### Prática:

* Utilizando o banco de dados Brazilian E-Commerce Public Dataset by Olist
* Filtrar clientes por estado
* Buscar produtos de uma categoria específica
* Filtrar pedidos entre dois valores ou datas
* Encontrar padrões em nomes de cidades ou categorias

## Semana 3 – Ordenação, Limites e Valores Nulos

#### O que estudar:

* ORDER BY (ASC/DESC)
* LIMIT / OFFSET
* Tratamento de nulos (IS NULL, IS NOT NULL, COALESCE)

#### Prática:

* Utilizando o banco de dados Brazilian E-Commerce Public Dataset by Olist
* Ordenar pedidos do mais recente para o mais antigo
* Listar apenas uma quantidade limitada de registros
* Pular um conjunto de registros e retornar os próximos
* Identificar colunas que possuem valores nulos
* Substituir valores nulos por valores padrão

## Semana 4 – Funções de String, Data e Matemática

#### O que estudar:

* UPPER, LOWER, CONCAT, SUBSTR, LENGTH
* EXTRACT, DATE_PART, cálculo de datas
* ROUND, ABS, CAST
* COALESCE e NULLIF

#### Prática:

* Utilizando o banco de dados Brazilian E-Commerce Public Dataset by Olist
* Padronizar nomes de cidades ou categorias
* Calcular ano ou mês das compras
* Calcular diferenças entre datas de pedido e entrega
* Arredondar valores de pagamento

## Semana 5 – Agregação e Agrupamento

#### O que estudar:

* COUNT, SUM, AVG, MIN, MAX
* GROUP BY
* Diferença entre WHERE e HAVING
* HAVING

#### Prática:

* Utilizando o banco de dados Brazilian E-Commerce Public Dataset by Olist
* Contar quantos pedidos existem no banco
* Calcular quantidade de clientes por estado
* Calcular média de valor de pagamento
* Identificar categorias com maior número de produtos
* Filtrar agrupamentos com base em condições específicas

## Semana 6 – Relacionamentos e JOINs (Parte 1)

#### O que estudar:

* Chave primária e estrangeira (conceito)
* INNER JOIN
* LEFT JOIN

#### Prática:

* Utilizando o banco de dados Brazilian E-Commerce Public Dataset by Olist
* Relacionar clientes com seus pedidos
* Relacionar pedidos com informações de pagamento
* Identificar pedidos que possuem clientes cadastrados
* Listar todos os clientes, incluindo aqueles que não possuem pedidos

## Semana 7 – JOINs (Parte 2) e Operações de Conjunto

#### O que estudar:

* RIGHT JOIN, FULL JOIN (visão geral)
* UNION, UNION ALL
* INTERSECT, EXCEPT

#### Prática:

* Utilizando o banco de dados Brazilian E-Commerce Public Dataset by Olist
* Comparar resultados obtidos com diferentes tipos de JOIN
* Unir resultados de diferentes consultas de clientes
* Identificar registros presentes em uma tabela e ausentes em outra
* Encontrar produtos que nunca apareceram em pedidos

## Semana 8 – Subconsultas (Subqueries)

#### O que estudar:

* Subquery no WHERE (IN, EXISTS)
* Subquery no SELECT
* Subquery no FROM (tabela derivada)

#### Prática:

* Utilizando o banco de dados Brazilian E-Commerce Public Dataset by Olist
* Encontrar pedidos com valor acima da média de pagamentos
* Calcular o total de vendas por produto e a porcentagem de participação de cada produto nas vendas
* Usar uma subconsulta como tabela temporária para analisar pedidos ou pagamentos

## Semana 9 – Manipulação de Dados (DML)

#### O que estudar:

* INSERT (várias formas)
* UPDATE
* DELETE
* Cuidados com WHERE em DELETE/UPDATE

#### Prática:

* Criar uma tabela de testes baseada em dados do banco Brazilian E-Commerce Public Dataset by Olist
* Inserir novos registros simulando novos clientes ou pedidos
* Atualizar informações de registros existentes
* Remover registros duplicados ou incorretos utilizando consultas com subquery

## Semana 10 – Criação e Alteração de Estruturas (DDL)

#### O que estudar:

* CREATE TABLE (tipos, constraints: NOT NULL, UNIQUE, DEFAULT)
* ALTER TABLE (ADD, DROP, MODIFY)
* PRIMARY KEY, FOREIGN KEY
* DROP TABLE

#### Prática:

* Modelar um sistema inspirado no banco Brazilian E-Commerce Public Dataset by Olist
* Criar tabelas para clientes, produtos e pedidos
* Definir chaves primárias e estrangeiras
* Alterar uma tabela para adicionar novas colunas ou restrições

## Semana 11 – Projeto Final (Parte 1)

#### O que estudar:

* Entender o problema de negócio
* Exploração do banco de dados
* Planejamento das consultas

#### Prática:

* Utilizando o banco de dados Brazilian E-Commerce Public Dataset by Olist
* Explorar as tabelas do banco
* Identificar relações entre as tabelas
* Definir perguntas de negócio para análise
* Iniciar consultas SQL para responder essas perguntas

## Semana 12 – Projeto Final (Parte 2)

#### O que estudar:

* Aplicação de todos os conceitos aprendidos
* Organização e interpretação dos resultados
* Apresentação de análises

#### Prática:

* Utilizando o banco de dados Brazilian E-Commerce Public Dataset by Olist
* Finalizar as consultas SQL
* Gerar análises sobre vendas, clientes e produtos
* Organizar os resultados obtidos
* Apresentar os insights encontrados a partir das consultas
