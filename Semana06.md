# Panda-sql

## Semana 06

A estrutura de bancos de dados relacionais é baseada na organização de informações em tabelas que se conectam entre si.
Para que essas conexões funcionem de forma consistente e confiável, utilizam-se conceitos fundamentais como chave primária,
chave estrangeira e operações de junção (joins), como `INNER JOIN` e `LEFT JOIN`. Esses elementos são essenciais para garantir integridade dos dados
e possibilitar análises mais complexas.

A **chave primária (Primary Key)** é um atributo — ou conjunto de atributos — que identifica de forma única cada registro dentro de uma tabela.
Isso significa que não podem existir dois registros com o mesmo valor na chave primária, e esse campo também não pode conter valores nulos.
A principal função da chave primária é garantir a unicidade dos dados, evitando duplicidade e permitindo que cada linha seja localizada de maneira precisa.
Por exemplo, em uma tabela de clientes, um campo como `id_cliente` geralmente é definido como chave primária, pois cada cliente deve possuir um identificador único.

Já a **chave estrangeira (Foreign Key)** é um campo que estabelece uma relação entre duas tabelas. Ela representa um vínculo com a chave primária de outra tabela,
funcionando como uma referência. Em outras palavras, a chave estrangeira permite que uma tabela “aponte” para registros de outra. Por exemplo, em uma tabela
de pedidos, o campo `id_cliente` pode ser uma chave estrangeira que se relaciona com a chave primária da tabela de clientes. Isso garante que um pedido
esteja sempre associado a um cliente válido, reforçando a integridade referencial do banco de dados.

A integridade referencial é um conceito importante nesse contexto, pois assegura que os relacionamentos entre tabelas sejam consistentes.
Ou seja, não é possível inserir um valor em uma chave estrangeira que não exista na tabela de origem.
Isso evita problemas como pedidos vinculados a clientes inexistentes. Além disso, dependendo da configuração do banco,
ações como exclusão ou atualização de registros podem ser controladas (por exemplo, com `ON DELETE CASCADE`), garantindo que alterações em uma tabela
sejam refletidas corretamente nas tabelas relacionadas.

Para trabalhar com dados distribuídos em várias tabelas, utilizam-se os **JOINs**, que permitem combinar informações com base em uma condição de
relacionamento. O **INNER JOIN** é o tipo mais comum e retorna apenas os registros que possuem correspondência em ambas as tabelas.
Em outras palavras, ele traz apenas os dados que estão relacionados. Por exemplo, ao fazer um `INNER JOIN` entre clientes e pedidos,
o resultado incluirá apenas os clientes que possuem pedidos registrados, ignorando aqueles que não têm.

Por outro lado, o **LEFT JOIN** (ou LEFT OUTER JOIN) retorna todos os registros da tabela da esquerda (a primeira mencionada na consulta),
mesmo que não exista correspondência na tabela da direita. Quando não há correspondência, os campos da tabela da direita aparecem como `NULL`.
Esse tipo de junção é útil quando se deseja manter todos os registros principais, independentemente de terem relação com outra tabela.
Por exemplo, ao fazer um `LEFT JOIN` entre clientes e pedidos, será possível visualizar todos os clientes, inclusive aqueles que nunca realizaram um pedido.

A principal diferença entre `INNER JOIN` e `LEFT JOIN` está justamente no tratamento dos dados sem correspondência. Enquanto o `INNER JOIN`
filtra e retorna apenas interseções entre as tabelas, o `LEFT JOIN` preserva todos os dados da tabela principal,
complementando com informações da outra tabela quando disponíveis. Essa escolha depende do tipo de análise que se deseja realizar.

Em resumo, a chave primária garante a identificação única dos registros, a chave estrangeira estabelece relacionamentos entre tabelas e os
JOINs permitem combinar essas informações de forma eficiente. 


### Prática 1: Relacionar clientes com seus pedidos

Utilizando a tabela clientes e pedidos

```sql
select 
   clientes.id_cliente,
   clientes.cidade_cliente,
   pedidos.id_pedido,
   pedidos.data_hora_compra
from clientes 
inner join pedidos 
    on clientes.id_cliente = pedidos.id_cliente;
```

Portanto, retorna somente clientes que possuem pedidos.

### Prática 2: Relacionar pedidos com informações de pagamento

Utilizando a tabela pedidos e pagamentos pedidos

```sql
select 
  pedidos.id_pedido,
  pagamentos_pedidos.pagamente_sequencial,
  pagamentos_pedidos.tipo_pagamento,
  pagamentos_pedidos.parcelamento_pagamento,
  pagamentos_pedidos.valor_pagamento
from pedidos 
inner join pagamentos_pedidos
  on pedidos.id_pedido = pagamentos_pedidos.id_pedido;
```

Mostra apenas pedidos que possuem pagamento registrado.


#### As práticas a seguir utilizam as tabelas clientes e pedidos

### Prática 3: Identificar pedidos que possuem clientes cadastrados

```sql

select 
    pedidos.id_pedido,
    pedidos.id_cliente,
    clientes.cidade_cliente,
    clientes.estado_cliente
from pedidos 
inner join clientes 
    on pedidos.id_cliente = clientes.id_cliente;
```

Mostra apenas pedidos que têm clientes válidos no banco.


### Prática 4: Listar todos os clientes, incluindo aqueles que não possuem pedidos

```sql

select 
    clientes.id_cliente,
    clientes.cidade_cliente,
    clientes.estado_cliente,
    pedidos.id_pedido,
    pedidos.data_hora_compra
from clientes 
left join pedidos 
    on clientes.id_cliente = pedidos.id_cliente;
```

Nesta consulta, o objetivo é listar todos os clientes cadastrados,
incluindo aqueles que não possuem pedidos registrados.
Para isso, foi utilizado um LEFT JOIN entre as tabelas clientes e pedidos.

O LEFT JOIN garante que todos os registros da tabela da esquerda
(clientes) sejam retornados, independentemente de haver
correspondência na tabela da direita (pedidos).
Quando um cliente não possui pedidos, as colunas provenientes
da tabela pedidos aparecem com valores NULL.

Foram selecionadas informações dos clientes, como identificador,
cidade e estado, além de dados dos pedidos, como o identificador
do pedido e a data da compra. Isso permite visualizar tanto clientes
ativos quanto aqueles que ainda não realizaram compras.

Em resumo, tal consulta mostra Uma lista com todos os clientes, mostrando seus pedidos quando existirem,
e NULL quando não existirem.




