# Panda-SQL

## Semana 11 - Projeto final (parte 1)

#### Prática 1: Explorar as tabelas do banco

Nesta etapa foi realizada uma análise exploratória do banco de dados Olist com o objetivo
de compreender sua estrutura, identificar as tabelas disponíveis e verificar quais informações
poderiam contribuir para a análise proposta no projeto.

Foram exploradas as tabelas relacionadas a pedidos, clientes, vendedores, produtos, avaliações,
pagamentos e geolocalização, permitindo identificar os principais atributos disponíveis para
análise. Também foram realizadas consultas para verificar a quantidade de registros, os tipos
de dados armazenados e a presença de possíveis inconsistências ou valores ausentes.

A análise exploratória permitiu compreender como os dados estão organizados e quais tabelas
seriam necessárias para responder às perguntas de negócio definidas para o projeto. Além disso,
essa etapa forneceu uma visão geral do marketplace, servindo como base para as análises
posteriores relacionadas ao desempenho de produtos, vendedores e comportamento dos clientes.

Visualização das tabelas existentes: 

```sql
select table_name
from information_schema.tables
where table_schema = 'public';
```

<img width="300" height="262" alt="image" src="https://github.com/user-attachments/assets/a35fd2d5-7cdd-4c82-b36b-5e387016faa8" />


#### Prática 2: Identificar relações entre as tabelas


Após a exploração inicial do banco de dados, foi realizado o mapeamento das relações existentes
entre as tabelas do conjunto de dados Olist.

Foram identificadas as chaves que permitem conectar informações de diferentes entidades do
marketplace, como pedidos, clientes, vendedores, produtos, pagamentos e avaliações. A compreensão
desses relacionamentos foi fundamental para a construção das consultas analíticas do projeto,
uma vez que muitas das informações necessárias encontram-se distribuídas em tabelas distintas.

Entre os principais relacionamentos identificados estão a associação entre pedidos e clientes,
pedidos e vendedores, pedidos e produtos, pedidos e formas de pagamento e pedidos e avaliações.
A partir dessas conexões foi possível estruturar consultas capazes de integrar informações de
diferentes áreas do negócio e gerar análises mais completas sobre vendas, faturamento,
satisfação dos clientes e desempenho dos vendedores.

Pedidos e clientes:

```sql
select
    p.id_pedido,
    c.id_cliente
from pedidos p
inner join clientes c
    on p.id_cliente = c.id_cliente
limit 10;
```

Pedidos e itens do pedido:

```sql
select
    p.id_pedido,
    ip.id_produto
from pedidos p
inner join itens_pedidos ip 
    on p.id_pedido = ip.id_pedido
limit 10;
```

Itens do pedido e produtos:

```sql
select
    ip.id_produto,
    pr.nome_categoria_produto
from itens_pedidos ip
inner join produtos pr
    on ip.id_produto = pr.id_produto
limit 10;
```

Pedidos e pagamentos: 

```sql
select
    p.id_pedido,
    pp.tipo_pagamento
from pedidos p
inner join pagamentos_pedidos pp
    on p.id_pedido = pp.id_pedido
limit 10;
```

Pedidos e avaliações:

```sql
select
    p.id_pedido,
    ap.pontuacao_avaliacao
from pedidos p
inner join avaliacao_pedidos ap 
    on p.id_pedido = ap.id_pedido
limit 10;
```

Pedidos e vendedores:

```sql
select
    ip.id_pedido,
    ip.id_vendedor
from itens_pedidos ip
limit 10;
```


#### Prática 3: Definir perguntas de negócio para análise

Este projeto tem como objetivo realizar uma análise exploratória do banco de dados Olist
para identificar o produto ou categoria responsável pela maior geração de receita no
marketplace e compreender os fatores associados ao seu desempenho comercial.

A partir da integração e análise das diferentes tabelas do banco de dados, serão investigados
aspectos relacionados à distribuição geográfica das vendas, ao perfil dos vendedores,
ao comportamento dos clientes, às formas de pagamento mais utilizadas, aos indicadores de
satisfação dos consumidores e aos aspectos logísticos das entregas.

O estudo busca transformar dados brutos em informações estratégicas, permitindo identificar
padrões e características que contribuem para o sucesso de determinados produtos.

Dessa forma, pretende-se gerar insights capazes de apoiar a tomada de decisões comerciais e
evidenciar fatores que podem influenciar o desempenho de vendas em ambientes de comércio
eletrônico.

Objetivo principal: o que explica o sucesso do produto que gera mais receita no marketplace?

Perguntas:

1. qual produto gera a maior receita bruta no marketplace?

2. a qual categoria pertence o produto que gera a maior receita bruta?

3. quais estados concentram o maior volume de compras desse produto?

4. o estado que mais compra esse produto coincide com o estado dos vendedores que mais o comercializam?

5. quais vendedores geram o maior faturamento com esse produto?

6. os vendedores que mais comercializam esse produto possuem avaliações acima da média do marketplace?

7. os clientes que compraram esse produto realizaram apenas uma compra ou efetuaram novas compras posteriormente?

8. qual é a taxa de recompra dos clientes que adquiriram esse produto?

9. qual é a forma de pagamento mais utilizada na compra desse produto?

10. qual percentual das vendas desse produto foi realizado por cartão de crédito?

11. considerando as vendas do produto de maior faturamento, qual seria a estimativa de custos com taxas de processamento de pagamentos e qual seu impacto potencial sobre a receita obtida?

12. qual é o prazo médio de entrega desse produto?

13. existe relação entre o prazo de entrega e as avaliações recebidas pelos clientes?

14. qual é a avaliação média desse produto?

15. produtos com maior faturamento tendem a apresentar avaliações melhores?

16. quais fatores parecem estar mais associados ao sucesso comercial do produto de maior faturamento, considerando localização geográfica, perfil dos vendedores, comportamento dos clientes, formas de pagamento, avaliações e desempenho logístico?


#### Prática 4: Iniciar consultas SQL para responder essas perguntas

1. qual produto gera a maior receita bruta no marketplace?

```sql
select
    id_produto,
    sum(preco) as receita_bruta
from itens_pedidos
group by id_produto
order by receita_bruta desc
limit 1;
```

<img width="508" height="68" alt="image" src="https://github.com/user-attachments/assets/2b1c66d8-1ca6-44c0-bc6e-0d17d84021d6" />


2. a qual categoria pertence o produto que gera a maior receita bruta?

```sql
select
    p.id_produto,
    p.nome_categoria_produto,
    sum(ip.preco) as receita_bruta
from itens_pedidos ip
inner join produtos p
    on ip.id_produto = p.id_produto
group by
    p.id_produto,
    p.nome_categoria_produto
order by receita_bruta desc
limit 1;
```

<img width="771" height="69" alt="image" src="https://github.com/user-attachments/assets/75e6f9ff-6622-4a13-a990-6bce04f0b312" />

Logo, é possível observar que o produto que gera maior receita é da categoria beleza/saúde.

3. quais estados concentram o maior volume de compras desse produto?

```sql
with produto_mais_receita as (
    select
        id_produto,
        sum(preco) as receita_bruta
    from itens_pedidos
    group by id_produto
    order by receita_bruta desc
    limit 1
)

select
    c.estado_cliente,
    count(*) as quantidade_compras,
    sum(ip.preco) as receita_gerada
from produto_mais_receita pmr
inner join itens_pedidos ip
    on pmr.id_produto = ip.id_produto
inner join pedidos p
    on ip.id_pedido = p.id_pedido
inner join clientes c
    on p.id_cliente = c.id_cliente
group by c.estado_cliente
order by receita_gerada desc;
```

<img width="663" height="569" alt="image" src="https://github.com/user-attachments/assets/df04b29a-5add-4136-8d8b-eecf47e6a43a" />

Por fim, São Paulo é o estado com a maior quantidade de compras do produto, com uma receita total de 8.185, sendo possível observar que Ceará e Minas Gerais estão próximos.
