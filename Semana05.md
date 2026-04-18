# Panda-sql

## Semana 05

As funções de agregação no SQL são utilizadas para resumir grandes volumes de dados em informações mais simples e úteis.
Entre as principais estão o `COUNT`, que conta a quantidade de registros; o `SUM`, que realiza a soma de valores numéricos;
o `AVG`, que calcula a média; o `MIN`, que retorna o menor valor; e o `MAX`, que retorna o maior valor dentro de um conjunto de dados.
Essas funções são essenciais em análises, pois permitem transformar dados brutos em métricas que facilitam a tomada de decisão.

O `GROUP BY` é um recurso que trabalha em conjunto com as funções de agregação.
Ele é responsável por agrupar registros que possuem valores iguais em determinadas colunas,
permitindo que os cálculos sejam feitos dentro de cada grupo. Por exemplo, é possível agrupar pedidos por status e contar quantos existem em cada categoria.
Sem o `GROUP BY`, as funções de agregação retornariam apenas um único resultado geral, enquanto com ele é possível obter análises segmentadas.

As cláusulas `WHERE` e `HAVING` são utilizadas para filtrar dados, mas atuam em momentos diferentes da execução da consulta.
O `WHERE` filtra os dados antes de qualquer agrupamento acontecer, sendo usado para restringir os registros que serão analisados.
Já o `HAVING` é aplicado após o `GROUP BY`, ou seja, ele filtra os resultados já agrupados. Por esse motivo, o `HAVING` permite o uso de funções de agregação,
enquanto o `WHERE` não permite.

O `HAVING` é especialmente útil quando se deseja aplicar condições sobre resultados agregados, como por exemplo mostrar apenas grupos que
possuem uma quantidade mínima de registros ou um valor total acima de determinado limite. Dessa forma, ele complementa o uso do `GROUP BY`,
permitindo análises mais refinadas e direcionadas.

De forma geral, esses conceitos são fundamentais para análise de dados em SQL, pois permitem organizar,
resumir e filtrar informações de maneira eficiente. O domínio dessas ferramentas possibilita extrair insights relevantes a partir dos dados,
sendo uma habilidade essencial em projetos de banco de dados e análise de dados.

### Prática 1: Contar quantos pedidos existem no banco

Utilizando a tabela pedidos

```sql
select count(*) as total_pedidos
from pedidos;
```

Podemos observar que o total de pedidos é igual a 99.441.


### Prática 2: Calcular quantidade de clientes por estado

Utilizando a tabela clientes

```sql
select estado_cliente,
  count(*) as total_clientes_estado
from clientes
group by estado_cliente 
order by total_clientes_estado desc;
```

Com esse código, verificamos que o o estado com a maior quantidade de clientes é São Paulo.

Vou fazer também para saber a cidade com o maior número de clientes.

```sql
select cidade_cliente,
  count(*) as total_clientes_cidade
from clientes
group by cidade_cliente 
order by total_clientes_cidade desc;

```
Portanto, verificamos que a cidade com mais clientes é Rio de Janeiro.


### Prática 3: Calcular média de valor de pagamento

Utilizando a tabela pagamentos pedidos

```sql

select 
 round(avg (valor_pagamento), 2) as media_pagamento
from pagamentos_pedidos;
```

Agora vou calcular a média de valor de pagamento por pedido.

```sql
select id_pedido,
 avg(valor_pagamento)as media_pagamento_pedido
from pagamentos_pedidos
group by id_pedido;
```

### Prática 4: Identificar categorias com maior número de produtos


Utilizando a tabela produtos

```sql
select nome_categoria_produto,
count(*) as total_produtos
from produtos p 
group by nome_categoria_produto 
order by total_produtos desc;
```

É possível observar que a categoria com a maior quantidade de produtos
é cama_mesa_banho e em seguida esporte_lazer.


### Prática 5: Filtrar agrupamentos com base em condições específicas

Utilizando a tabela produtos, com as colunas comprimento_produto_cm, 
altura_produto_cm, largura_produto_cm

```sql


select 
    nome_categoria_produto, 
    round(avg(comprimento_produto_cm) ,2)as media_comprimento,
    round(avg(altura_produto_cm) ,2) as media_altura,
    round(avg(largura_produto_cm),2) as media_largura
from produtos
group by nome_categoria_produto
having avg(comprimento_produto_cm) > 30;
```

Nesta consulta, o objetivo é filtrar agrupamentos com base em condições
específicas utilizando a tabela produtos. Para isso, foram utilizadas as
colunas de dimensões dos produtos, como comprimento, altura e largura.

Inicialmente, foi aplicada a função AVG para calcular a média dessas
dimensões para cada categoria de produto. Em seguida, a cláusula GROUP BY
foi utilizada para agrupar os dados com base na coluna nome_categoria_produto,
permitindo que as médias fossem calculadas separadamente para cada grupo.

Após o agrupamento, foi utilizada a cláusula HAVING para filtrar apenas
as categorias cuja média de comprimento dos produtos é maior que 30 cm.
Diferente do WHERE, o HAVING permite aplicar condições sobre valores
agregados, como médias e somas.

Essa abordagem é útil para identificar padrões e destacar grupos que
atendem a critérios específicos, sendo muito utilizada em análises de
dados.

O resultado da consulta mostrou apenas 30 categorias de um total de 73 vistas na prática 4.




