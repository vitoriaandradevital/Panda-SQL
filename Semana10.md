# Panda-sql

## Semana 10  – Criação e Alteração de Estruturas (DDL)

Os comandos `CREATE TABLE`, `ALTER TABLE`, `PRIMARY KEY`, `FOREIGN KEY` e `DROP TABLE` fazem parte da linguagem SQL e são fundamentais para a construção e
manutenção de bancos de dados relacionais. Esses comandos pertencem ao grupo conhecido como DDL (*Data Definition Language*), responsável pela definição da
estrutura do banco de dados. Através deles, é possível criar tabelas, modificar estruturas já existentes, estabelecer relacionamentos entre informações e
remover tabelas quando necessário. O entendimento aprofundado desses conceitos é essencial para qualquer profissional que trabalhe com banco de dados, pois
eles garantem organização, integridade e eficiência no armazenamento das informações.

O comando `CREATE TABLE` é utilizado para criar tabelas dentro do banco de dados. Durante a criação da tabela, o desenvolvedor define os nomes das colunas,
os tipos de dados que cada coluna armazenará e também as restrições que controlam a validade das informações inseridas. Os tipos de dados possuem papel
fundamental, pois determinam quais valores podem ser armazenados em cada campo. Entre os tipos numéricos mais utilizados estão `INTEGER`, usado para números
inteiros, `SERIAL`, que gera números automáticos normalmente utilizados como identificadores, e `NUMERIC` ou `DECIMAL`, usados para valores com casas decimais
e maior precisão. Já para armazenamento de textos, os tipos mais comuns são `CHAR`, `VARCHAR` e `TEXT`. O tipo `VARCHAR` é amplamente utilizado porque permite
textos de tamanho variável, economizando espaço no armazenamento. Existem também tipos voltados para datas e horários, como `DATE`, `TIME` e `TIMESTAMP`, além
do tipo `BOOLEAN`, utilizado para armazenar valores lógicos como verdadeiro e falso.

Além dos tipos de dados, o `CREATE TABLE` permite o uso de constraints, também chamadas de restrições. Essas regras são extremamente importantes para garantir
a integridade dos dados armazenados no banco. Uma das constraints mais utilizadas é a `NOT NULL`, que impede que determinada coluna receba valores vazios.
Essa restrição é essencial em campos obrigatórios, como nome, senha, CPF ou email. Outra constraint importante é a `UNIQUE`, responsável por impedir valores
duplicados em uma coluna. Ela é muito utilizada em campos que devem possuir informações exclusivas, como emails, números de matrícula ou CPF. Existe também
a constraint `DEFAULT`, que define um valor padrão caso o usuário não informe nenhum valor durante a inserção dos dados. Essa funcionalidade é bastante
útil para automatizar informações frequentes, como datas de cadastro ou status iniciais.

Entre as constraints mais importantes está a `PRIMARY KEY`, ou chave primária. Sua principal função é identificar unicamente cada registro de uma tabela.
Uma chave primária não pode possuir valores repetidos nem valores nulos, garantindo que cada linha da tabela seja única. Normalmente, a chave primária é
representada por um campo de identificação numérica, como `id_cliente` ou `id_produto`. A existência da chave primária é fundamental para a organização do
banco de dados, pois ela facilita buscas, relacionamentos e controle das informações armazenadas. Em alguns casos, a chave primária pode ser composta por
mais de uma coluna, formando uma chave composta, onde a combinação dos valores deve ser única.

Outro conceito extremamente importante em bancos relacionais é a `FOREIGN KEY`, conhecida como chave estrangeira. Essa constraint é utilizada para criar
relacionamentos entre tabelas diferentes. A chave estrangeira estabelece uma ligação entre um campo de uma tabela e a chave primária de outra tabela,
garantindo integridade referencial. Isso significa que um valor só poderá existir em determinada tabela se ele já existir previamente na tabela relacionada.
Por exemplo, em uma tabela de pedidos, o campo que representa o cliente normalmente será uma chave estrangeira ligada à tabela de clientes. Dessa forma,
o banco impede a criação de pedidos associados a clientes inexistentes. Esse mecanismo evita inconsistências e garante maior segurança no armazenamento das
informações.

Após a criação das tabelas, muitas vezes surge a necessidade de modificar sua estrutura. Para isso existe o comando `ALTER TABLE`, responsável por realizar
alterações em tabelas já existentes. Com esse comando é possível adicionar novas colunas, remover colunas antigas, alterar tipos de dados e modificar constraints.
O `ALTER TABLE` é extremamente importante porque permite adaptar o banco de dados às novas necessidades do sistema sem precisar recriar toda a estrutura.
Por exemplo, pode ser necessário adicionar um campo de telefone em uma tabela de clientes ou aumentar o tamanho máximo permitido para um nome. Também é possível
adicionar ou remover restrições conforme a evolução do sistema e das regras de negócio.

Entre as operações realizadas com `ALTER TABLE`, destaca-se o uso do `ADD`, utilizado para adicionar novas colunas, e do `DROP`, utilizado para remover
colunas existentes. Além disso, muitos bancos de dados permitem o uso de comandos para modificar propriedades das colunas, como tamanho, tipo de dado ou
obrigatoriedade de preenchimento. Essas alterações devem ser realizadas com cuidado, principalmente em tabelas que já possuem dados armazenados, pois
mudanças incorretas podem gerar perda de informações ou incompatibilidades no sistema.

Por fim, o comando `DROP TABLE` é utilizado para remover completamente uma tabela do banco de dados. Ao executar esse comando, toda a estrutura da tabela,
seus registros, constraints e relacionamentos são apagados permanentemente. Trata-se de uma operação destrutiva e geralmente irreversível, exigindo bastante
atenção antes de sua utilização. Em bancos relacionais, pode ser necessário utilizar a opção `CASCADE` para remover também os relacionamentos associados à tabela.
O uso inadequado do `DROP TABLE` pode causar perda total de dados importantes, motivo pelo qual esse comando costuma ser restrito a administradores ou usuários
com permissões elevadas.

Portanto, os comandos `CREATE TABLE`, `ALTER TABLE`, `PRIMARY KEY`, `FOREIGN KEY` e `DROP TABLE` são essenciais para a modelagem e gerenciamento de bancos de
dados relacionais. Eles permitem criar estruturas organizadas, garantir integridade dos dados, estabelecer relacionamentos entre informações e adaptar o banco às
necessidades do sistema ao longo do tempo. O domínio desses conceitos é indispensável para o desenvolvimento de aplicações robustas e eficientes, além de
representar uma base fundamental para estudos mais avançados em banco de dados e engenharia de software.

### Práticas: 

* Modelar um sistema inspirado no banco Brazilian E-Commerce Public Dataset by Olist
* Definir chaves primárias e estrangeiras
* Criar tabelas para clientes, produtos e pedidos

```sql

create table clientes_modelado (
    id_cliente serial primary key,
    nome_cliente varchar(100) not null,
    email_cliente varchar(100) unique not null,
    cidade_cliente varchar(100),
    estado_cliente char(2),
    data_cadastro date default current_date
);

create table vendedores_modelado (
    id_vendedor serial primary key,
    nome_vendedor varchar(100) not null,
    email_vendedor varchar(100) unique,
    cidade_vendedor varchar(100),
    estado_vendedor char(2)
);

create table categorias_modelado (
    id_categoria serial primary key,
    nome_categoria varchar(100) unique not null
);

create table produtos_modelado (
    id_produto serial primary key,
    nome_produto varchar(150) not null,
    descricao_produto text,
    preco_produto numeric(10,2) not null,
    estoque_produto int default 0,
    id_categoria int,
    id_vendedor int,

    foreign key (id_categoria)
    references categorias_modelado(id_categoria),

    foreign key (id_vendedor)
    references vendedores_modelado(id_vendedor)
);

create table pedidos_modelado (
    id_pedido serial primary key,
    data_pedido timestamp default current_timestamp,
    status_pedido varchar(50) default 'processando',
    valor_total numeric(10,2),
    id_cliente int not null,

    foreign key (id_cliente)
    references clientes_modelado(id_cliente)
);

create table itens_pedido_modelado (
    id_item serial primary key,
    quantidade int not null,
    preco_unitario numeric(10,2) not null,
    id_pedido int,
    id_produto int,

    foreign key (id_pedido)
    references pedidos_modelado(id_pedido),

    foreign key (id_produto)
    references produtos_modelado(id_produto)
);

create table pagamentos_modelado (
    id_pagamento serial primary key,
    tipo_pagamento varchar(50),
    valor_pagamento numeric(10,2) not null,
    data_pagamento timestamp default current_timestamp,
    id_pedido int,

    foreign key (id_pedido)
    references pedidos_modelado(id_pedido)
);

create table entregas_modelado (
    id_entrega serial primary key,
    status_entrega varchar(50),
    data_envio date,
    data_entrega date,
    codigo_rastreio varchar(100) unique,
    id_pedido int,

    foreign key (id_pedido)
    references pedidos_modelado(id_pedido)
);

```

Sempre que ocorre a criação de uma tabela, uma janela similar a essa é aberta:

<img width="564" height="550" alt="image" src="https://github.com/user-attachments/assets/c1070128-0bf5-4189-bb30-1275b1df85be" />






* Alterar uma tabela para adicionar novas colunas ou restrições
  
```sql

/* adicionar novas colunas na tabela clientes_modelado */

alter table clientes_modelado
add cpf_cliente varchar(14) unique;

alter table clientes_modelado
add telefone_cliente varchar(20);


/* adicionar nova coluna na tabela produtos_modelado */

alter table produtos_modelado
add marca_produto varchar(100);

alter table produtos_modelado
add peso_produto numeric(8,2);


/* adicionar restrição not null */

alter table produtos_modelado
alter column marca_produto set not null;


/* adicionar valor default */

alter table pedidos_modelado
alter column status_pedido
set default 'aguardando pagamento';


/* adicionar nova constraint check */

alter table produtos_modelado
add constraint chk_preco_produto
check (preco_produto > 0);


/* adicionar nova coluna na tabela entregas_modelado */

alter table entregas_modelado
add transportadora varchar(100);


```
