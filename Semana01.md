# Panda-SQL

## Semana 01 


### SQL

SQL (Structured Query Language) é a linguagem padrão utilizada para trabalhar com bancos de dados relacionais. 
Ela permite criar, consultar, atualizar e excluir dados armazenados em tabelas. Entre seus principais comandos estão o SELECT, 
usado para consultar dados; o INSERT, para inserir novos registros; o UPDATE, para atualizar informações existentes; 
o DELETE, para remover dados; e o CREATE TABLE, utilizado para criar estruturas de tabelas dentro do banco.


### Banco de dados relacionais

Os bancos de dados relacionais são sistemas que organizam os dados em forma de tabelas.
Cada tabela é composta por linhas, chamadas de registros, e colunas, chamadas de atributos. 
Esses bancos são chamados “relacionais” porque permitem a criação de relações entre tabelas por meio de chaves, 
como a chave primária (PRIMARY KEY), que identifica cada registro de forma única, e a chave estrangeira (FOREIGN KEY), 
que conecta uma tabela a outra. Esse modelo facilita a organização, integridade e consulta eficiente dos dados.


### Postgres (PostgreSQL) e DBeaver

O PostgreSQL é um dos sistemas de banco de dados relacionais mais utilizados no mundo.
Ele é conhecido por sua robustez, segurança e capacidade de lidar com grandes volumes de dados.
Para facilitar o uso do PostgreSQL, é comum utilizar ferramentas como o DBeaver, que é um ambiente gráfico (GUI) que permite conectar,
visualizar e gerenciar bancos de dados de forma mais simples, sem depender apenas do terminal.

Para utilizar o PostgreSQL com o DBeaver, primeiro é necessário instalar o PostgreSQL, que funciona como o servidor do banco de dados.
Durante a instalação, é definido um usuário, geralmente “postgres”, e uma senha de acesso.
Em seguida, instala-se o DBeaver, onde será criada uma conexão com o banco informando dados como host (geralmente localhost),
porta (normalmente 5432), usuário, senha e o nome do banco de dados.
Após a conexão ser estabelecida, é possível visualizar tabelas e executar comandos SQL.


### SELECT

O primeiro comando fundamental ao aprender SQL é o SELECT, utilizado para consultar dados de uma tabela.
Sua forma mais básica é SELECT * FROM nome_da_tabela;, que retorna todos os registros e colunas da tabela.
O SELECT é a base de praticamente todas as consultas em SQL e o primeiro passo para trabalhar com bancos de dados relacionais.


## Práticas: 

* Instalar os softwares
* Baixar o banco de dados do Kaggle Brazilian E‑Commerce Public Dataset by Olist
* Importar as tabelas do dataset no ambiente
* Explorar as tabelas do banco para entender as colunas e os dados disponíveis


Utilizei o comando Select para explorar as tabelas do Conjunto de dados públicos de comércio eletrônico brasileiro da Olist



select * from clientes;  

select * from geolocalizacao;  

select * from itens_pedidos;

select * from pagamentos_pedidos;

select * from avaliacao_pedidos;

select * from pedidos;

select * from pagamentos_pedidos;

select * from produtos;

select * from traducao_categoria_produto;

select * from vendedores;

Nessa etapa, foram registradas as alterações realizadas nos nomes e tipos de dados das colunas das tabelas do banco de dados.
Essas modificações foram feitas manualmente, ao invés da utilização do comando AS, pois as colunas originalmente estavam nomeadas em inglês e,
por questão de praticidade, optou-se por realizar a tradução diretamente na estrutura do banco no início do processo.

Dessa forma, foi utilizado o DDL (Data Definition Language) gerado automaticamente pela ferramenta para a criação das tabelas, 
sendo posteriormente ajustado manualmente para refletir as alterações necessárias. Abaixo estão apresentadas as estruturas das tabelas do banco de dados:



```sql 
CREATE TABLE public.clientes (
    id_cliente text NULL,
    id_unico_cliente text NULL,
    cinco_digitos_cep_cliente varchar NULL,
    cidade_cliente text NULL,
    estado_cliente text NULL
);

CREATE TABLE public.geolocalizacao (
    prefixo_codigo_postal varchar NULL,
    latitude_geolocalizacao float8 NULL,
    geolocalizacao_longitude float8 NULL,
    cidade_geolocalizacao varchar(100) NULL,
    estado_geolocalizacao varchar(100) NULL
);

CREATE TABLE public.itens_pedidos (
    id_pedido varchar(50) NULL,
    id_item_pedido int4 NULL,
    id_produto varchar(50) NULL,
    id_vendedor varchar(50) NULL,
    data_limite_envio timestamp NULL,
    preco numeric NULL,
    valor_frete numeric NULL
);

CREATE TABLE public.vendedores (
    id_vendedor varchar(50) NULL,
    prefixo_cep_vendedor varchar NULL,
    cidade_vendedor varchar(20) NULL,
    estado_vendedor varchar(20) NULL
);

CREATE TABLE public.pagamentos_pedidos (
    id_pedido varchar(50) NULL,
    pagamente_sequencial int4 NULL,
    tipo_pagamento varchar(20) NULL,
    parcelamento_pagamento int4 NULL,
    valor_pagamento numeric NULL
);

CREATE TABLE public.pedidos (
    id_pedido varchar(50) NULL,
    id_cliente varchar(50) NULL,
    status_pedido varchar(20) NULL,
    data_hora_compra timestamp NULL,
    pedido_aprovado text NULL,
    data_entrega_transportadora text NULL,
    data_entrega_cliente text NULL,
    data_estimada_entrega text NULL
);

CREATE TABLE public.produtos (
    id_produto varchar(50) NULL,
    nome_categoria_produto varchar(100) NULL,
    comprimento_nome_produto int4 NULL,
    comprimento_descricao_produto int4 NULL,
    quantidade_foto_produto int4 NULL,
    peso_produto_g int4 NULL,
    comprimento_produto_cm int4 NULL,
    altura_produto_cm int4 NULL,
    largura_produto_cm int4 NULL
);


CREATE TABLE public.traducao_categoria_produto (
	nome_categoria_produto varchar(50) NULL,
	categoria_produto_ingles varchar(50) NULL
);
