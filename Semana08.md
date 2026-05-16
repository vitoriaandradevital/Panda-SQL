# Panda-sql

## Semana 08 – Subconsultas (Subqueries)


Subquery, também chamada de subconsulta, é uma consulta SQL escrita dentro de outra consulta. Em vez de resolver tudo em um único comando, o
SQL permite dividir o problema em partes: primeiro uma consulta interna busca ou calcula alguma informação, e depois a consulta principal
utiliza esse resultado. Isso torna o código mais organizado e muito útil para situações em que você precisa filtrar dados, comparar valores,
gerar colunas calculadas ou montar tabelas temporárias. Em bancos de dados reais, subqueries são usadas constantemente em relatórios, análises e
consultas mais avançadas.

De forma simples, a consulta principal é a que aparece “por fora”, enquanto a subquery fica entre parênteses dentro dela. Dependendo do contexto,
a subquery pode retornar apenas um valor, vários valores ou até uma tabela inteira. Se ela retornar um único valor, pode ser usada para comparações
simples, como buscar produtos acima da média de preço. Se retornar vários valores, costuma ser usada com operadores como IN. Se retornar várias
colunas e linhas, pode funcionar como uma tabela temporária no FROM.

Uma das utilizações mais comuns é a subquery no WHERE. Nesse caso, ela serve para ajudar a filtrar registros. Imagine que você queira listar
apenas clientes que fizeram pedidos. Em vez de juntar tabelas diretamente, a subquery pode buscar os IDs dos clientes que aparecem na tabela
de pedidos, e a consulta principal mostra apenas esses clientes. Esse tipo de uso é muito comum porque permite criar filtros inteligentes
baseados em outras tabelas ou em cálculos internos.

Dentro do WHERE, um operador muito usado com subquery é o IN. O IN verifica se um valor está dentro de uma lista retornada pela subconsulta.
Por exemplo, se a subquery retornar os IDs 1, 2 e 5, a consulta principal pode verificar quais clientes possuem esses IDs. É uma forma prática
e fácil de entender quando se trabalha com listas de valores. Quando o objetivo é encontrar registros que não estão nessa lista, pode-se usar
NOT IN, embora seja necessário cuidado quando existem valores NULL, pois isso pode causar resultados inesperados.

Outro operador importante é o EXISTS. Diferente do IN, ele não compara valores diretamente, mas verifica se a subquery retorna pelo menos uma
linha. Se retornar, a condição é verdadeira. Isso é muito útil quando você quer saber se existe relacionamento entre registros. Por exemplo,
verificar se um cliente possui pelo menos um pedido cadastrado. Em muitos casos, EXISTS pode ter melhor desempenho que IN, especialmente em tabelas grandes.

Também existe o NOT EXISTS, usado para verificar ausência de registros relacionados. Ele é excelente para perguntas como “quais clientes nunca
compraram?” ou “quais produtos nunca foram vendidos?”. Em situações práticas, NOT EXISTS costuma ser mais seguro do que NOT IN, principalmente
quando há possibilidade de valores nulos nos dados.

Outro tipo importante é a subquery no SELECT. Nesse caso, a subconsulta aparece como se fosse uma coluna adicional no resultado final. Ela é usada
para calcular informações específicas para cada linha retornada. Por exemplo, ao listar clientes, você pode incluir uma coluna mostrando quantos
pedidos cada cliente fez. A consulta principal mostra os clientes, enquanto a subquery calcula individualmente a quantidade de pedidos de cada um.

Esse tipo de subquery no SELECT é muito útil para enriquecer relatórios. Em vez de apenas mostrar nomes ou IDs, você consegue trazer métricas como
total de compras, maior pedido, média de gastos ou data da última compra. Porém, em tabelas muito grandes, pode impactar o desempenho, pois a subquery
pode ser executada repetidamente para cada linha retornada pela consulta principal.

Há também a subquery no FROM, que funciona como uma tabela temporária criada no momento da execução. Nesse caso, a subquery retorna um conjunto de
dados que será tratado como se fosse uma tabela normal. Isso é extremamente útil quando você precisa fazer cálculos intermediários antes de continuar
a consulta. Por exemplo, primeiro somar o total vendido por cliente e depois usar esse resultado para cruzar com a tabela de clientes.

Quando a subquery fica no FROM, normalmente ela recebe um alias, ou seja, um nome temporário. Esse nome é obrigatório na maioria dos bancos de dados,
porque a consulta externa precisa se referir àquela tabela derivada. Esse recurso ajuda muito a organizar consultas complexas, especialmente quando há
agrupamentos, médias, rankings ou várias etapas de cálculo.

As subqueries também podem ser classificadas como correlacionadas e não correlacionadas. Uma subquery não correlacionada é independente da consulta
principal, podendo ser executada sozinha. Já a correlacionada depende de valores da linha atual da consulta externa. Por exemplo, ao calcular quantos
pedidos cada cliente fez, a subquery precisa saber qual cliente está sendo analisado naquele momento. Por isso, ela se relaciona diretamente com a
consulta principal.

Na prática, muita gente compara subquery com JOIN. O JOIN é excelente para unir tabelas relacionadas e costuma ser mais eficiente em várias situações.
Já a subquery muitas vezes oferece uma lógica mais clara e direta, principalmente em filtros, comparações com médias, existência de registros ou cálculos
internos. Não existe uma regra absoluta: em alguns casos JOIN é melhor, em outros a subquery deixa a consulta mais simples e legível.




### Prática 1: Encontrar pedidos com valor acima da média de pagamentos

```sql

/*Encontrando a média, ainda sem subqueries*/


select avg(valor_pagamento) as media_pagamentos
from pagamentos_pedidos;



/*Encontrar pedidos ccom valor acima da média de pagamentos*/

select id_pedido, valor_pagamento
from pagamentos_pedidos 
where valor_pagamento > (
select avg(valor_pagamento)
from pagamentos_pedidos);


/*Quero encontrar pedidos que tenham valor abaixo da média de pagamentos, então apenas mudo o operador*/

select id_pedido, valor_pagamento
from pagamentos_pedidos 
where valor_pagamento < (
select avg(valor_pagamento)
from pagamentos_pedidos);

/*Vou descobrir se existe algum pedido com o valor igual a média de pagamento*/

select id_pedido, valor_pagamento
from pagamentos_pedidos 
where valor_pagamento = (
select avg(valor_pagamento)
from pagamentos_pedidos);

/*Como deu vazio, não tem pedidos com o valor exatamente igual a média de pagamentos (aproximadamente 154,10) */


/*Mostra pedidos com valor acima da média de pagamentos e também mostra a média*/
select 
    id_pedido,
    valor_pagamento,
    (
        select avg (valor_pagamento)
        from pagamentos_pedidos
    ) as media_pagamentos
from pagamentos_pedidos
where valor_pagamento > (
    select avg(valor_pagamento)
    from pagamentos_pedidos
);


/*Agora, vou mostrar os pedidos com o valor de pagamento acima da média e calculando a diferença entre o valor de pagamento e a média*/


select
    id_pedido,
    valor_pagamento,
    (
        select avg (valor_pagamento)
        from pagamentos_pedidos
    ) as media_pagamentos,
    valor_pagamento - (
        select avg(valor_pagamento)
        from pagamentos_pedidos
    ) as diferenca_media
from pagamentos_pedidos
where valor_pagamento > (
    select avg (valor_pagamento)
    from pagamentos_pedidos
);


```


### Prática 2: Calcular o total de vendas por produto e a porcentagem de participação de cada produto nas vendas

```sql

select  
   itens_pedidos.id_produto,
   sum (pagamentos_pedidos.valor_pagamento ) as total_vendas_produtos,
   round (
      sum(pagamentos_pedidos.valor_pagamento) * 100.0 /
      (
          select sum(valor_pagamento)
          from pagamentos_pedidos 
      ),
      2
    ) as procentagem_participacao_vendas
from pagamentos_pedidos 
join itens_pedidos
  on pagamentos_pedidos.id_pedido = itens_pedidos.id_pedido 
group by itens_pedidos.id_produto ;


```

<img width="894" height="161" alt="image" src="https://github.com/user-attachments/assets/4a458357-6d57-4531-b6f2-198fc78960bb" />


### Prática 3: Usar uma subconsulta como tabela temporária para analisar pedidos ou pagamentos

```sql

/*Para analisar pedidos*/

select
    resumo.status_pedido,
    resumo.quantidade_pedidos,
    resumo.valor_total,
    round(
        resumo.valor_total / resumo.quantidade_pedidos,
        2
    ) as media_por_pedido
from
(
    select
        pedidos.status_pedido,
        count(pedidos.id_pedido) as quantidade_pedidos,
        sum(pagamentos_pedidos.valor_pagamento) as valor_total
    from pedidos
    join pagamentos_pedidos
        on pedidos.id_pedido = pagamentos_pedidos.id_pedido
    group by pedidos.status_pedido
) as resumo;

/*Para analisar pagamentos*/

select
    resumo.tipo_pagamento,
    resumo.quantidade_pagamentos,
    resumo.valor_total,
    round(
        resumo.valor_total / resumo.quantidade_pagamentos,
        2
    ) as media_pagamento
from
(
    select
        pagamentos_pedidos.tipo_pagamento,
        count(pagamentos_pedidos.id_pedido) as quantidade_pagamentos,
        sum(pagamentos_pedidos.valor_pagamento) as valor_total
    from pagamentos_pedidos
    group by pagamentos_pedidos.tipo_pagamento
) as resumo;


```
