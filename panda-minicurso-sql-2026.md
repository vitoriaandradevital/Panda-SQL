# Minicurso de Introdução ao SQL

Este repositório reúne os materiais do **Minicurso Presencial de Introdução ao SQL**, promovido pelo **Grupo de Processamento e Análise de Dados (PANDA)** da **Universidade Federal de São Carlos (UFSCar)**.

O **PANDA** é um grupo de extensão voltado à Ciência de Dados e suas diversas áreas de atuação, como análise de dados, engenharia de dados, aprendizado
de máquina e temas correlatos. Entre suas atividades, o grupo promove estudos, capacitações e projetos que contribuem para a formação de estudantes e da
comunidade.

Neste repositório estão disponíveis os conteúdos utilizados durante o minicurso, incluindo explicações dos conceitos fundamentais, exemplos de consultas SQL,
exercícios práticos e desafios desenvolvidos para auxiliar no aprendizado e na aplicação dos conhecimentos em bancos de dados.

O minicurso presencial foi desenvolvido pelos membros **Caio Miyaji Ishii, Fagner Santos de Oliveira, Lucas Sufredini, Pedro Kuiava, Rennan Siqueira, Thamara Gabriella Crispim, Vinicius Yuya Massuda e Vitória Vital**, com o objetivo de apresentar os fundamentos da linguagem por meio de uma abordagem prática.

Ao longo das atividades, os participantes aprendem a construir consultas em bancos de dados relacionais, adquirindo a base necessária para avançar em tópicos
mais complexos.


## Objetivo

O minicurso foi desenvolvido para introduzir os participantes aos fundamentos da linguagem SQL, combinando explicações teóricas com exercícios práticos.
Ao longo das atividades, serão apresentados os principais conceitos de bancos de dados relacionais, desde a realização das primeiras consultas até a
utilização de comandos para filtragem e ordenação de dados.

Durante o minicurso, os participantes irão:

- compreender os conceitos básicos de SQL e bancos de dados relacionais;
- importar e explorar um banco de dados real para compreender sua estrutura;
- construir consultas utilizando `SELECT`, `FROM` e `AS`;
- aplicar filtros com `WHERE`, operadores de comparação, operadores lógicos (`AND`, `OR` e `NOT`) e os operadores `LIKE`, `IN` e `BETWEEN`;
- ordenar resultados utilizando `ORDER BY`, `ASC` e `DESC`;
- resolver exercícios práticos baseados em situações reais de consulta a bancos de dados.

Ao final do minicurso, os participantes terão os conhecimentos necessários para desenvolver consultas SQL básicas e estarão preparados para avançar para tópicos
mais complexos da linguagem.



## Conteúdo

O material está organizado para o período de três semanas:

### Semana 1 – Introdução ao SQL

* O que é SQL
* Bancos de dados relacionais
* Primeiro comando `SELECT`
* Estrutura básica de uma consulta


### Semana 2 – Consultas Básicas e Filtragem

* `WHERE`: permite selecionar apenas os registros que atendem a uma determinada condição,
* funcionando como um filtro para encontrar exatamente os dados desejados.

* Operadores de comparação: utilizados para definir condições de busca, permitindo comparar valores usando operadores como igual (`=`),
* diferente (`<>`), maior que (`>`), menor que (`<`), maior ou igual (`>=`) e menor ou igual (`<=`).

* `AND`, `OR` e `NOT`: permitem combinar ou modificar filtros, possibilitando criar condições mais específicas durante a consulta dos dados.

* `LIKE`: utilizado para buscar informações que seguem determinado padrão de texto, sendo útil quando não sabemos exatamente o valor que estamos procurando.

* `IN`: permite verificar se um valor está dentro de uma lista de possibilidades, evitando a necessidade de escrever várias condições usando `OR`.

* `BETWEEN`: utilizado para buscar valores dentro de um intervalo definido, como números entre dois valores ou datas dentro de um determinado período.


### Semana 3 – Ordenação e organização dos dados

* `ORDER BY`: utilizado para organizar os resultados de uma consulta SQL, permitindo definir a ordem em que os registros serão exibidos.

* Ordenação crescente (`ASC`): organiza os valores do menor para o maior, sendo o comportamento padrão do `ORDER BY` quando nenhuma ordem é especificada.

* Ordenação decrescente (`DESC`): organiza os valores do maior para o menor, sendo útil para visualizar maiores valores, datas mais recentes ou rankings.

* Ordenação por múltiplas colunas: permite definir diferentes critérios de ordenação, organizando os dados por uma coluna principal e utilizando outras colunas
como critério de desempate.



## Banco de dados

Os exercícios utilizam o **Brazilian E-Commerce Public Dataset by Olist**, um conjunto de dados públicos amplamente utilizado para fins educacionais e
estudos de análise de dados.

O banco foi escolhido por possuir dados reais e uma estrutura adequada para praticar consultas SQL desde os conceitos mais básicos. Além disso,
trata-se do mesmo banco de dados utilizado nos estudos do **grupo de estudos de SQL do Panda**, permitindo acompanhar uma base já explorada pela
comunidade e aplicar os conhecimentos desenvolvidos durante as atividades.



## Metodologia

O minicurso foi estruturado para integrar teoria e prática, proporcionando uma experiência de aprendizado didática, progressiva e aplicada.
Inicialmente, os conceitos fundamentais de SQL são apresentados por meio de explicações claras, exemplos práticos e materiais de apoio desenvolvidos para
facilitar a compreensão dos participantes.

Durante as aulas, são utilizados conteúdos e estruturas já desenvolvidos e disponibilizados no **GitHub do grupo de estudos**, aproveitando materiais
previamente construídos e alinhados com a proposta de ensino de SQL.

Como complemento aos estudos, foi desenvolvida uma **plataforma web interativa de desafios SQL**, implementada em **Python** utilizando **Streamlit** e disponibilizada por meio do **Streamlit Community Cloud**, a partir de um repositório no **GitHub**.

O desenvolvimento e a implementação da plataforma foram realizados pelos membros [Thamara Crispim](https://github.com/ThamaraCrispim), [Rennan Siqueira](https://github.com/rennansiq), [Pedro Kuiava](https://github.com/PeKuFer) e [Vitória Vital](https://github.com/vitoriaandradevital).

A plataforma foi projetada para tornar o aprendizado mais intuitivo e dinâmico, permitindo que os participantes escrevam e executem consultas SQL
diretamente pelo navegador, sem a necessidade de configurações complexas. Por meio de uma interface simples e organizada, o usuário pode resolver desafios,
testar seus conhecimentos e receber feedback imediato sobre suas respostas.

A validação automática das consultas auxilia na identificação de erros e contribui para a evolução dos participantes, permitindo que eles pratiquem no
próprio ritmo e apliquem imediatamente os conceitos apresentados durante as aulas. Dessa forma, a metodologia combina explicação teórica, materiais de apoio,
exercícios práticos e uma ferramenta interativa para consolidar o aprendizado em SQL.


## Referências

Os materiais, códigos e recursos utilizados no desenvolvimento do minicurso estão disponíveis nos seguintes links:

* **Repositório do grupo de estudos SQL:** [Acessar repositório](https://github.com/pandaufscar/panda-sql-study-group-2026/tree/main)

* **Plataforma interativa de desafios SQL:** [Acessar plataforma](https://panda-minicurso-sql-2026-oci6s4ze262rtimukpi6ex.streamlit.app/)


## Tecnologias utilizadas

* SQL
* PostgreSQL
* GitHub
* Python
* Streamlit (plataforma de desafios)

