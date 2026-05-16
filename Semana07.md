# Panda-sql


## Semana 07: JOINs (Parte 2) e Operações de Conjunto

Os comandos **RIGHT JOIN, FULL JOIN, UNION, UNION ALL, INTERSECT e EXCEPT** são recursos muito importantes do SQL,
pois permitem combinar informações de tabelas diferentes ou comparar resultados de consultas. Eles são bastante usados
em bancos de dados relacionais, como PostgreSQL, MySQL, SQL Server e Oracle. Entender esses comandos é essencial para
trabalhar com consultas mais completas e profissionais.

O **RIGHT JOIN** é um tipo de junção entre tabelas. Ele retorna todos os registros da tabela que está à direita da consulta
e traz os dados correspondentes da tabela da esquerda quando houver relação entre elas. Caso não exista correspondência,
os campos da tabela da esquerda aparecem como NULL. Imagine uma tabela de clientes e outra de pedidos. Se você usar RIGHT JOIN
trazendo todos os pedidos, mesmo que algum pedido esteja sem cliente cadastrado, ele aparecerá no resultado. Esse comando é
menos usado no dia a dia porque geralmente as pessoas preferem inverter a ordem das tabelas e utilizar LEFT JOIN, que tem funcionamento equivalente.

O **FULL JOIN**, também chamado de FULL OUTER JOIN, retorna todos os registros das duas tabelas ao mesmo tempo. Quando existe
correspondência entre os dados, ele une as informações em uma única linha. Quando não existe, os campos ausentes são preenchidos
com NULL. Esse comando é muito útil para comparar tabelas, identificar registros faltando ou fazer auditoria de dados. Por exemplo,
se uma tabela possui clientes e outra possui pedidos, o FULL JOIN mostrará clientes sem pedidos e pedidos sem cliente cadastrado.

Já os comandos **UNION** e **UNION ALL** servem para unir resultados de consultas diferentes. O UNION combina os resultados e remove
valores duplicados automaticamente. Se uma pessoa aparece em duas listas diferentes, ela surgirá apenas uma vez no resultado final.
Isso é útil quando se deseja consolidar informações sem repetição. Para funcionar corretamente, as consultas precisam ter a mesma
quantidade de colunas e tipos compatíveis.

O **UNION ALL** faz algo parecido, porém mantém os valores repetidos. Se um nome aparecer em duas tabelas, ele aparecerá duas vezes
no resultado. Esse comando costuma ser mais rápido que o UNION porque o banco de dados não precisa verificar duplicidades. Ele é
recomendado quando as repetições são importantes ou quando se busca melhor desempenho em tabelas grandes.

O comando **INTERSECT** retorna apenas os registros que existem nos dois resultados consultados. Ele representa a interseção entre
conjuntos. Por exemplo, se uma tabela possui clientes de São Paulo e outra possui clientes do Rio de Janeiro, o INTERSECT mostrará
apenas os clientes que aparecem nas duas listas. Esse comando é muito útil para encontrar elementos em comum entre sistemas,
campanhas, cadastros ou períodos diferentes.

O **EXCEPT** retorna os registros do primeiro SELECT que não existem no segundo. Ele funciona como uma diferença entre conjuntos.
Se uma lista contém clientes de janeiro e outra clientes de fevereiro, o EXCEPT pode mostrar quem comprou em janeiro, mas não
comprou em fevereiro. Um detalhe importante é que a ordem das consultas altera o resultado. Fazer A EXCEPT B é diferente de fazer B EXCEPT A.

Em resumo, os JOINs são usados principalmente para relacionar tabelas, enquanto UNION, INTERSECT e EXCEPT trabalham combinando ou
comparando resultados de consultas. O RIGHT JOIN traz tudo da direita, o FULL JOIN traz tudo de ambas as tabelas, o UNION
junta sem repetir, o UNION ALL junta mantendo repetições, o INTERSECT mostra apenas os dados em comum e o EXCEPT mostra diferenças
entre listas. Dominar esses comandos permite criar consultas muito mais poderosas e resolver problemas reais dentro de bancos de dados profissionais.


### Prática 1: Comparar resultados obtidos com diferentes tipos de JOIN


Usando tabelas clientes, pedidos e pagamentos pedidos 

INNER JOIN

```sql

/* Mostra apenas clientes que possuem pedidos */

select
    clientes.id_cliente,
    clientes.cidade_cliente,
    pedidos.id_pedido,
    pedidos.status_pedido
from clientes 
inner join pedidos 
on clientes.id_cliente = pedidos.id_cliente;

```


LEFT JOIN

```sql

/* Mostra todos os clientes, mesmo sem pedidos */

select 
    clientes.id_cliente,
    clientes.cidade_cliente,
    clientes.estado_cliente,
    pedidos.id_pedido,
    pedidos.status_pedido
from clientes 
left join pedidos 
on clientes.id_cliente = pedidos.id_cliente ;


/*Mostra todos os pedidos, mesmo sem pagamento */

select 
    pedidos.id_pedido,
    pedidos.status_pedido,
    pagamentos_pedidos.tipo_pagamento  ,
    pagamentos_pedidos.valor_pagamento
from pedidos
left join pagamentos_pedidos
on pedidos.id_pedido = pagamentos_pedidos.id_pedido ;

```


RIGHT JOIN

```sql
/* Mostra todos os pedidos, mesmo sem cliente correspondente */
select
    clientes.id_cliente,
    clientes.cidade_cliente,
    pedidos.id_pedido,
    pedidos.status_pedido
from clientes 
right join pedidos 
on clientes.id_cliente = pedidos.id_cliente;

```

FULL JOIN

```sql

/* Mostra todos clientes e todos pedidos */

select 
    clientes.id_cliente,
    clientes.cidade_cliente,
    pedidos.id_pedido,
    pedidos.status_pedido
from clientes 
full join pedidos 
on clientes.id_cliente = pedidos.id_cliente;

```


### Prática 2: Unir resultados de diferentes consultas de clientes

```sql

/* Une os estados São Paulo e Rio de Janeiro dos clientes*/

select estado_cliente,  cidade_cliente from clientes;

select id_cliente, cidade_cliente, estado_cliente
from clientes
where estado_cliente = 'SP'

union

select id_cliente, cidade_cliente, estado_cliente
from clientes
where estado_cliente = 'RJ';

```

<img width="699" height="161" alt="image" src="https://github.com/user-attachments/assets/01726e9d-d0a4-4db8-91d0-669dd0e84e45" />


```sql

/* Une os clientes que são da cidade de Franca e Campinas*/

select id_cliente, cidade_cliente
from clientes
where cidade_cliente = 'Franca'

union

select id_cliente, cidade_cliente
from clientes
where cidade_cliente = 'Campinas'

select id_cliente, cidade_cliente
from clientes
where cidade_cliente = 'Franca'

union all 

select id_cliente, cidade_cliente
from clientes
where cidade_cliente = 'Campinas'

```

Escolhi não mostrar a coluna de estado_cliente pelas duas cidades estarem situadas no mesmo estado (São Paulo).
O comando union remove registros duplicados automaticamente, já o comando union all mantém todas as linhas, inclusive repetidas.


### Prática 3: Identificar registros presentes em uma tabela e ausentes em outra

```sql

select id_pedido
from pedidos

except

select id_pedido
from pagamentos_pedidos;

```

<img width="326" height="62" alt="image" src="https://github.com/user-attachments/assets/cc5ec9c4-efab-4986-8f36-8875c582c3c9" />

Como resultado, apenas um pedido com id igual a bfbd0f9bdef84302105ad712db648a6c esta presente na tabela pedidos mas
não esta presente na tabela pagamentos pedidos.


### Prática 4: Encontrar produtos que nunca apareceram em pedidos

```sql

select id_produto
from produtos

except

select id_produto
from itens_pedidos;

```

O resultado vazio significa que não foi encontrado nenhum produto na tabela produtos que esteja ausente em itens_pedidos.


### Utilizando o comando intersect 

Vou utilizar o intersect para ver os id_cliente que coincidem na tabela clientes e na tabela pedidos, isso é apenas um exemplo, pois 
não faz parte de nenhuma prática da semana.

```sql

select id_cliente
from clientes 

intersect 

select id_cliente
from pedidos;

/*Agora, vou contar quantos coincidem.*/

select count(*)
from (
    select id_cliente
    from clientes
    
    intersect 
    
    select id_cliente
    from pedidos
) x;


/*Logo em seguida, vou verificar se todos os id_cliente da tabela clientes aparecem em pedidos*/

select
(
    select count(*)
    from (
        select id_cliente
        from clientes

        intersect

        select id_cliente
        from pedidos
    ) x
) AS ids_em_comum,

(
    select COUNT(*)
    from clientes
) as total_clientes;


/*Como o resultado foi igual, é provado que todos os clientes aparecem em pedidos*/

```


