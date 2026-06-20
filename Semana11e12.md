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
aspectos relacionados ao perfil dos vendedores,
ao comportamento dos clientes, às formas de pagamento mais utilizadas, aos indicadores de
satisfação dos consumidores e aos aspectos logísticos das entregas.

O estudo busca transformar dados brutos em informações estratégicas, permitindo identificar
padrões e características que contribuem para o sucesso de determinados produtos.

Dessa forma, pretende-se gerar insights capazes de apoiar a tomada de decisões comerciais e
evidenciar fatores que podem influenciar o desempenho de vendas em ambientes de comércio
eletrônico.

Objetivo principal: Quais fatores parecem estar mais associados ao sucesso comercial do produto de maior faturamento?


Perguntas:

1. qual produto gera a maior receita bruta no marketplace?

2. a qual categoria pertence o produto que gera a maior receita bruta?

3. quais estados concentram o maior volume de compras desse produto?

4. o estado que mais compra esse produto coincide com o estado dos vendedores que mais o comercializam?

5. quais vendedores geram o maior faturamento com esse produto?

6. os vendedores que mais comercializam esse produto possuem avaliações acima da média do marketplace?

7. qual é a taxa de recompra dos clientes que adquiriram esse produto?

8. qual é a forma de pagamento mais utilizada na compra desse produto?

9. existe relação entre o prazo de entrega e as avaliações recebidas pelos clientes?

10. qual é a avaliação média desse produto?

11. produtos com maior faturamento tendem a apresentar avaliações melhores?

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

A análise permitiu identificar o produto responsável pela maior geração de receita bruta no marketplace (63.885) . Esse resultado evidencia qual item possui maior relevância financeira para a plataforma.

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

A identificação da categoria do produto de maior faturamento (beleza/saúde) permite compreender quais segmentos apresentam maior potencial de geração de receita. Esse resultado pode auxiliar na identificação de tendências de consumo e oportunidades de mercado.

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

A distribuição geográfica das compras permite identificar os principais mercados consumidores do produto analisado. Conhecer o estado com maior volume de vendas (São Paulo ) auxilia no direcionamento de estratégias comerciais e na compreensão da demanda regional.


## Semana 12 - Projeto Final (Parte 2)

4. o estado que mais compra esse produto coincide com o estado dos vendedores que mais o comercializam?

```sql
with produto_mais_receita as (
    select
        id_produto
    from itens_pedidos
    group by id_produto
    order by sum(preco) desc
    limit 1
)

select
    v.estado_vendedor,
    count(*) as quantidade_vendas
from produto_mais_receita pmr
inner join itens_pedidos ip
    on pmr.id_produto = ip.id_produto
inner join vendedores v
    on ip.id_vendedor = v.id_vendedor
group by v.estado_vendedor
order by quantidade_vendas desc;
```

<img width="484" height="71" alt="image" src="https://github.com/user-attachments/assets/81c3a0e6-a031-4c30-a7f7-dec2db27d938" />

A comparação entre a localização dos clientes e dos vendedores permite avaliar a existência de concentração regional da oferta e da demanda.
Essa análise pode indicar possíveis vantagens logísticas e oportunidades de expansão para outras regiões. Portanto, o estado que mais compra esse produto
coincide com o estado do vendedor que mais o vende.

5. quais vendedores geram o maior faturamento com esse produto?

```sql
with produto_mais_receita as (
    select
        id_produto
    from itens_pedidos
    group by id_produto
    order by sum(preco) desc
    limit 1
)

select
    ip.id_vendedor,
    sum(ip.preco) as faturamento
from produto_mais_receita pmr
inner join itens_pedidos ip
    on pmr.id_produto = ip.id_produto
group by ip.id_vendedor
order by faturamento desc;
```

<img width="493" height="74" alt="image" src="https://github.com/user-attachments/assets/08282082-34ea-4ee9-981e-58b492e1bec7" />

A identificação dos vendedores com maior faturamento permite reconhecer os principais responsáveis pelo desempenho comercial do produto.
Esse vendedorer (f7ba60f8c3f99e7ee4042fdef03b70c4) pode servir como referência para a compreensão de fatores associados ao sucesso nas vendas.

6. o que vendedor mais comercializou esse produto possui avaliações acima da média do marketplace?

```sql
with produto_mais_receita as (
    select
        id_produto
    from itens_pedidos
    group by id_produto
    order by sum(preco) desc
    limit 1
)

select
    ip.id_vendedor,
    avg(ap.pontuacao_avaliacao) as media_avaliacao
from produto_mais_receita pmr
inner join itens_pedidos ip
    on pmr.id_produto = ip.id_produto
inner join pedidos p
    on ip.id_pedido = p.id_pedido
inner join avaliacao_pedidos ap
    on p.id_pedido = ap.id_pedido
group by ip.id_vendedor
order by media_avaliacao desc;
```

<img width="536" height="66" alt="image" src="https://github.com/user-attachments/assets/440689cb-d71e-41a7-82d8-354e56a8fe39" />

A análise das avaliações dos vendedores possibilita verificar se existe relação entre reputação e desempenho comercial.
Avaliações mais elevadas podem indicar maior confiança dos clientes e contribuir para melhores resultados de vendas.
O vendedor (f7ba60f8c3f99e7ee4042fdef03b70c4) possui avaliação 4.2 de um total de 5, portanto, sua avaliação é considerada acima da média.

7. qual é a taxa de recompra dos clientes que adquiriram esse produto?

```sql
with produto_mais_receita as (
    select
        id_produto
    from itens_pedidos
    group by id_produto
    order by sum(preco) desc
    limit 1
),
clientes_produto as (
    select distinct
        c.id_cliente
    from produto_mais_receita pmr
    inner join itens_pedidos ip
        on pmr.id_produto = ip.id_produto
    inner join pedidos p
        on ip.id_pedido = p.id_pedido
    inner join clientes c
        on p.id_cliente = c.id_cliente
)

select
    round(
        100.0 * sum(
            case
                when total_pedidos > 1 then 1
                else 0
            end
        ) / count(*),
        2
    ) as taxa_recompra
from (
    select
        cp.id_cliente,
        count(distinct p.id_pedido) as total_pedidos
    from clientes_produto cp
    inner join pedidos p
        on cp.id_cliente = p.id_cliente
    group by cp.id_cliente
) t;
```

<img width="269" height="73" alt="image" src="https://github.com/user-attachments/assets/6ee5c0ec-c542-48a6-9400-fe11c052b98f" />

A taxa de recompra permite avaliar o nível de fidelização dos clientes que adquiriram o produto analisado.
Valores mais elevados sugerem maior satisfação e maior probabilidade de retorno dos consumidores para novas compras. A taxa de recompra dos
clientes que compraraam esse produto foi zero.

8. qual é a forma de pagamento mais utilizada na compra desse produto?

```sql
with produto_mais_receita as (
    select
        id_produto
    from itens_pedidos
    group by id_produto
    order by sum(preco) desc
    limit 1
)

select
    pp.tipo_pagamento,
    count(*) as quantidade
from produto_mais_receita pmr
inner join itens_pedidos ip
    on pmr.id_produto = ip.id_produto
inner join pagamentos_pedidos pp
    on ip.id_pedido = pp.id_pedido
group by pp.tipo_pagamento
order by quantidade desc;
```

<img width="435" height="140" alt="image" src="https://github.com/user-attachments/assets/7ea26f38-b7ba-42b6-a0a4-e27d34e9355d" />

A identificação da forma de pagamento predominante (cartão de crédito) permite compreender as preferências dos consumidores durante o processo de compra.
Essas informações podem contribuir para análises sobre comportamento de consumo e estratégias comerciais. 

9. existe relação entre o prazo de entrega e as avaliações recebidas pelos clientes?

```sql
with produto_mais_receita as (
    select
        id_produto
    from itens_pedidos
    group by id_produto
    order by sum(preco) desc
    limit 1
)

select
    case
        when (
            cast(p.data_entrega_cliente as timestamp) -
            p.data_hora_compra
        ) <= interval '5 days'
            then 'ate 5 dias'

        when (
            cast(p.data_entrega_cliente as timestamp) -
            p.data_hora_compra
        ) <= interval '10 days'
            then '6 a 10 dias'

        else 'mais de 10 dias'
    end as faixa_entrega,

    round(
        avg(ap.pontuacao_avaliacao)::numeric,
        2
    ) as media_avaliacao

from produto_mais_receita pmr
inner join itens_pedidos ip
    on pmr.id_produto = ip.id_produto
inner join pedidos p
    on ip.id_pedido = p.id_pedido
inner join avaliacao_pedidos ap
    on p.id_pedido = ap.id_pedido

where p.data_entrega_cliente is not null
and trim(p.data_entrega_cliente) <> ''

group by faixa_entrega
order by faixa_entrega;
```

<img width="439" height="125" alt="image" src="https://github.com/user-attachments/assets/b4ae7a85-c10c-4bc6-8681-719654e2eb0f" />

A análise busca verificar se a experiência logística influencia a satisfação dos consumidores.
Diferenças nas avaliações associadas aos prazos de entrega podem indicar a importância da eficiência logística para a percepção de qualidade do serviço.
É possível observar que quanto mais rápido a entrega, maior é a avaliação, portanto existe relação entre prazo de entrega e avaliação do cliente sobre o produto.

10. qual é a avaliação média desse produto?

```sql
with produto_mais_receita as (
    select
        id_produto
    from itens_pedidos
    group by id_produto
    order by sum(preco) desc
    limit 1
)

select
    round(
        avg(ap.pontuacao_avaliacao),
        2
    ) as avaliacao_media
from produto_mais_receita pmr
inner join itens_pedidos ip
    on pmr.id_produto = ip.id_produto
inner join pedidos p
    on ip.id_pedido = p.id_pedido
inner join avaliacao_pedidos ap
    on p.id_pedido = ap.id_pedido;
```

<img width="259" height="70" alt="image" src="https://github.com/user-attachments/assets/6c39e379-7483-4147-a655-d2e0dd0f2723" />

A avaliação média do produto (4.2 de 5) permite medir o nível geral de satisfação dos clientes em relação ao produto.
Esse indicador é importante para compreender se o desempenho comercial do produto está acompanhado de uma experiência positiva para os consumidores.

11. produtos com maior faturamento tendem a apresentar avaliações melhores?

```sql
select
    ip.id_produto,
    round(sum(ip.preco),2) as faturamento,
    round(avg(ap.pontuacao_avaliacao),2) as media_avaliacao
from itens_pedidos ip
inner join pedidos p
    on ip.id_pedido = p.id_pedido
inner join avaliacao_pedidos ap
    on p.id_pedido = ap.id_pedido
group by ip.id_produto
having count(ap.pontuacao_avaliacao) > 10
order by faturamento desc
limit 20;
```

<img width="722" height="550" alt="image" src="https://github.com/user-attachments/assets/03b9abf6-fcf1-44f1-b45e-0a69ca8a29e8" />

A comparação entre faturamento e avaliações busca identificar possíveis relações entre desempenho financeiro e satisfação dos clientes.
Essa análise permite verificar se os produtos mais rentáveis também são aqueles que apresentam melhor aceitação por parte dos consumidores.
É possível observer que embora se tenha um baixo faturamento, a avaliação pode ser tanto alta quanto média/baixa, o mesmo ocorre para produtos com
altos faturamentos.

## Análise:

A análise dos dados do marketplace permitiu identificar o produto responsável pela maior geração de receita bruta, alcançando um faturamento de R$ 63.885,00.
Esse resultado evidencia a relevância desse item para o desempenho financeiro da plataforma e demonstra sua importância dentro do conjunto de produtos comercializados.
Além disso, foi possível verificar que esse produto pertence à categoria de beleza e saúde, indicando que esse segmento apresenta elevado potencial de geração de receita e
forte demanda por parte dos consumidores.

A distribuição geográfica das vendas mostrou que o estado de São Paulo concentra o maior volume de compras desse produto, destacando-se como o principal mercado consumidor.
Esse resultado evidencia a importância estratégica da região para o marketplace, podendo servir de base para ações de marketing direcionadas e para o planejamento logístico. Observou-se ainda que o estado que mais compra o produto coincide com o estado do vendedor que mais o comercializa, sugerindo uma possível vantagem logística decorrente da proximidade entre oferta e demanda, o que pode contribuir para a eficiência das operações.

Entre os vendedores responsáveis pela comercialização do produto, destacou-se o vendedor identificado pelo código f7ba60f8c3f99e7ee4042fdef03b70c4, que apresentou o maior
faturamento associado a esse item. Esse resultado sugere que suas práticas comerciais podem servir como referência para a compreensão dos fatores relacionados ao sucesso
nas vendas. Além disso, esse vendedor obteve avaliação média de 4,2 em uma escala de 1 a 5, valor considerado acima da média, indicando que a boa reputação junto aos
clientes pode estar associada ao seu desempenho comercial.

Em relação ao comportamento dos consumidores, verificou-se que a taxa de recompra dos clientes que adquiriram esse produto foi igual a zero.
Esse resultado sugere que os compradores não retornaram para realizar novas aquisições do mesmo item durante o período analisado. Embora isso possa indicar baixa
fidelização especificamente para esse produto, também pode estar relacionado às características da própria categoria, uma vez que determinados produtos não exigem compras
frequentes.

A análise das formas de pagamento revelou que o cartão de crédito foi o meio mais utilizado pelos consumidores na aquisição do produto. Esse comportamento demonstra a
preferência dos clientes por modalidades de pagamento eletrônicas, além de reforçar a importância desse método para a realização das vendas dentro do marketplace.

Por fim, a comparação entre faturamento e avaliações indicou que não existe uma relação direta entre o desempenho financeiro dos produtos e sua avaliação pelos consumidores.
Foi possível observar que produtos com faturamento elevado podem apresentar tanto avaliações altas quanto médias ou baixas, enquanto produtos com menor faturamento também
podem alcançar avaliações positivas. Esse resultado sugere que fatores adicionais, como preço, necessidade do mercado, concorrência e estratégias de venda, podem exercer
influência significativa sobre o faturamento, não sendo a avaliação do produto o único determinante para seu sucesso comercial.

De modo geral, os resultados obtidos demonstram que o sucesso comercial do produto analisado está associado a uma combinação de fatores, incluindo a categoria de atuação,
a concentração da demanda em determinadas regiões, o desempenho dos vendedores e as preferências dos consumidores. Essas informações permitem compreender melhor os padrões
de comportamento presentes no marketplace e podem servir como suporte para futuras decisões estratégicas relacionadas a vendas, marketing e logística.


