# Panda-sql

## Semana 03

O comando **ORDER BY** é utilizado no SQL para organizar os resultados de uma consulta.
Ele permite ordenar os dados com base em uma ou mais colunas. A ordenação pode ser feita de forma crescente, usando `ASC` (ascending),
que é o padrão do SQL, ou de forma decrescente, usando `DESC` (descending). Por exemplo, ao ordenar uma tabela de clientes pelo nome em ordem crescente,
os registros serão exibidos em ordem alfabética. Já com `DESC`, a ordem será invertida, exibindo do maior para o menor ou de Z para A,
dependendo do tipo de dado. O `ORDER BY` é essencial para organizar resultados de forma lógica e facilitar a análise dos dados.

O `LIMIT` e o `OFFSET` são utilizados para controlar a quantidade de registros retornados em uma consulta.
O `LIMIT` define quantas linhas serão exibidas, enquanto o `OFFSET` define quantas linhas serão ignoradas antes de começar a exibir os resultados.
Esses comandos são muito usados em paginação de dados, como em sistemas que exibem listas de clientes ou produtos em várias páginas.
Por exemplo, `LIMIT 10` retorna apenas os 10 primeiros registros, enquanto `OFFSET 10 LIMIT 10` pula os 10 primeiros registros e retorna os próximos 10.
Essa combinação é fundamental para melhorar a performance e organização de grandes volumes de dados.

O tratamento de valores nulos no SQL é feito com os operadores `IS NULL` e `IS NOT NULL`.
Valores nulos representam a ausência de informação em uma coluna, ou seja, um campo vazio.
O operador `IS NULL` é utilizado para identificar registros que não possuem valor preenchido em uma determinada coluna, enquanto `IS NOT NULL`
retorna apenas os registros que possuem algum valor. É importante destacar que não se usa `= NULL`,
pois o correto no SQL é sempre utilizar `IS NULL` para esse tipo de verificação.

O `COALESCE` é uma função utilizada para lidar com valores nulos de forma mais prática.
Ela retorna o primeiro valor não nulo de uma lista de expressões. Ou seja, se um campo estiver vazio (NULL),
o `COALESCE` permite substituí-lo por um valor padrão definido pelo usuário. Por exemplo, se a cidade de um cliente estiver nula,
é possível exibir “Não informado” no lugar. Isso é muito útil em relatórios e consultas, pois evita valores vazios e melhora a apresentação dos dados.


### Prática 1: Ordenar pedidos do mais recente para o mais antigo

Utilizando a tabela pedidos:

```sql
select * from pedidos;

/*Seleciona a coluna data e hora da compra do pedido e a ordena de forma crescente*/

select data_hora_compra from pedidos
order by data_hora_compra asc;
```


#### Utilizando a tabela clientes para as práticas a seguir: 

### Prática 2: Listar apenas uma quantidade limitada de registros


```sql
/*Seleciona as colunas data estimada de entrega e data de entrega ao cliente limitada a
10 registros de forma ordenada crescente */

select data_estimada_entrega, data_entrega_cliente from pedidos
limit (10);
```

### Prática 3: Pular um conjunto de registros e retornar os próximos

```sql
/*Pula os 10 primeiros registros e retorna os 10 próximos*/

select * from pedidos
order by status_pedido 
limit 10 offset 10;
```

### Prática 4: Identificar colunas que possuem valores nulos

```sql
/*Primeiro realizei a contagem de quantos clientes tem em cada cidade para descobrir o valor
padrão, que no caso é o rio de janeiro*/

select cidade_cliente,
count(*) as total from clientes
group by cidade_cliente
order by total DESC;

/*Em seguida, irei substituir os valores nulos por rio de janeiro*/

select cidade_cliente,
coalesce (cidade_cliente,
( select cidade_cliente from clientes
where cidade_cliente is not null
group by cidade_cliente
order by count(*) DESC
limit 1
)
)as cidade_cliente_corrigida
from clientes;

/*Agora, vou atualizar de verdade no banco de dados*/

update clientes
set cidade_cliente = (
select cidade_cliente 
from clientes
where cidade_cliente is not null
group by cidade_cliente 
order by count(*) desc
limit 1
)
where cidade_cliente is null;

select cidade_cliente from clientes;


/*Agora, vou atualizar todas as colunas com valores nulos com esse código direto*/

UPDATE clientes
SET estado_cliente = (
    SELECT estado_cliente
    FROM clientes
    WHERE estado_cliente IS NOT NULL
    GROUP BY estado_cliente
    ORDER BY COUNT(*) DESC
    LIMIT 1
)
WHERE estado_cliente IS NULL;

select estado_cliente from clientes;

select * from clientes;

/*Vou testar se deu certo pulando registros*/

select cidade_cliente, estado_cliente from clientes
limit 10 offset 10;

UPDATE pedidos
SET status_pedido = (
    SELECT status_pedido
    FROM pedidos
    WHERE status_pedido IS NOT NULL
    GROUP BY status_pedido
    ORDER BY COUNT(*) DESC
    LIMIT 1
)
WHERE status_pedido IS NULL;

select * from pedidos;

```





