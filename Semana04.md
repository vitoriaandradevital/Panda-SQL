# Panda-sql

## Semana 04

O `UPPER`, `LOWER`, `CONCAT`, `SUBSTR` e `LENGTH` são funções muito utilizadas no SQL para manipulação de textos.
O `UPPER` converte todos os caracteres de uma string para maiúsculas,
enquanto o `LOWER` converte para minúsculas, sendo úteis para padronização de dados.
O `CONCAT` serve para unir textos ou colunas, permitindo criar combinações como nome completo a partir de nome e sobrenome.
Já o `SUBSTR` (ou `SUBSTRING`) é usado para extrair parte de uma string, indicando posição inicial e quantidade de caracteres.
Por fim, o `LENGTH` retorna o tamanho de uma string, ou seja, a quantidade de caracteres presentes nela.

As funções `EXTRACT` e `DATE_PART` são utilizadas para trabalhar com datas no SQL.
Elas permitem extrair partes específicas de uma data, como ano, mês, dia, hora ou minuto.
Por exemplo, é possível extrair apenas o ano de uma data de nascimento ou o mês de uma compra.
O `EXTRACT` é mais padrão SQL, enquanto o `DATE_PART` é muito usado no PostgreSQL, mas ambos têm a mesma finalidade.
Além disso, o SQL permite realizar cálculos com datas, como somar ou subtrair intervalos de tempo, o que é útil para analisar prazos, 
empo de entrega ou diferença entre datas.

As funções `ROUND`, `ABS` e `CAST` são usadas para operações numéricas e conversões de tipo.
O `ROUND` serve para arredondar valores decimais, definindo quantas casas decimais devem ser mantidas.
O `ABS` retorna o valor absoluto de um número, ou seja, remove o sinal negativo.
Já o `CAST` é utilizado para converter um tipo de dado em outro, como transformar um texto em número ou uma data em string.
Essas funções são essenciais para garantir consistência nos cálculos e na manipulação de dados.

O `COALESCE` e o `NULLIF` são funções voltadas para o tratamento de valores nulos.
O `COALESCE` retorna o primeiro valor não nulo de uma lista de expressões, sendo muito útil para substituir valores vazios por padrões definidos,
como “não informado”. Já o `NULLIF` compara dois valores e retorna NULL caso eles sejam iguais,
sendo útil para evitar divisões por zero ou tratar dados duplicados de forma condicional.
Essas funções ajudam a tornar os dados mais consistentes e evitam problemas em consultas e relatórios.


### Prática 1: Padronizar nomes de cidades ou categoria

Utilizando a tabela produtos:

Nesta etapa, foi realizada a padronização de campos textuais no banco de dados.
O objetivo foi garantir consistência na escrita dos dados, evitando 
problemas como variações de letras maiúsculas e minúsculas (por exemplo: “SAO PAULO”, “sao paulo”, “Sao paulo”).


Inicialmente, foi feita a consulta simples com SELECT para visualizar os valores originais da coluna.
Essa etapa é importante para entender como os dados estão armazenados antes de qualquer modificação.

```sql
select nome_categoria_produto from produtos;
```
Em seguida, foi utilizada uma expressão para padronizar os textos no formato:

* Primeira letra maiúscula
* Restante das letras minúsculas

```sql
select
    upper(substr(nome_categoria_produto, 1, 1)) ||
    lower(substr(nome_categoria_produto, 2, length(nome_categoria_produto))) 
    as categoria_padronizada
from produtos;
```

Após validar o resultado com SELECT, foi aplicado um UPDATE para salvar a padronização diretamente no banco:
* UPDATE → altera os dados existentes
* SET → define o novo valor da coluna
* WHERE ... IS NOT NULL → garante que apenas valores existentes sejam atualizados, evitando erros com valores nulos

```sql
update produtos
set nome_categoria_produto = 
    upper(substr(nome_categoria_produto, 1, 1)) ||
    lower(substr(nome_categoria_produto, 2, length(nome_categoria_produto)))
where nome_categoria_produto IS NOT NULL;
```

A mesma lógica foi aplicada em diferentes colunas do banco de dados, como:

* cidade_cliente (tabela clientes)
* tipo_pagamento (tabela pagamentos_pedidos)
* status_pedido (tabela pedidos)
* cidade_vendedor (tabela vendedores)

```sql
select
    upper(substr(cidade_cliente, 1, 1)) ||
    lower(substr(cidade_cliente , 2, length(cidade_cliente ))) 
    as categoria_padronizada
from clientes;

update clientes
set cidade_cliente = 
    upper(substr(cidade_cliente, 1, 1)) ||
    lower(substr(cidade_cliente, 2, length(cidade_cliente)))
where cidade_cliente IS NOT NULL;

select tipo_pagamento from pagamentos_pedidos;

select
    upper(substr(tipo_pagamento, 1, 1)) ||
    lower(substr(tipo_pagamento , 2, length(tipo_pagamento ))) 
    as categoria_padronizada
from pagamentos_pedidos;

update pagamentos_pedidos
set tipo_pagamento = 
    upper(substr(tipo_pagamento, 1, 1)) ||
    lower(substr(tipo_pagamento, 2, length(tipo_pagamento)))
where tipo_pagamento IS NOT NULL;


select status_pedido from pedidos;

select
    upper(substr(status_pedido, 1, 1)) ||
    lower(substr(status_pedido , 2, length(status_pedido ))) 
    as categoria_padronizada
from pedidos;

update pedidos
set status_pedido = 
    upper(substr(status_pedido, 1, 1)) ||
    lower(substr(status_pedido, 2, length(status_pedido)))
where status_pedido IS NOT NULL;


select cidade_vendedor from vendedores;

select
    upper(substr(cidade_vendedor, 1, 1)) ||
    lower(substr(cidade_vendedor , 2, length(cidade_vendedor))) 
    as categoria_padronizada
from vendedores;

update vendedores
set cidade_vendedor = 
    upper(substr(cidade_vendedor, 1, 1)) ||
    lower(substr(cidade_vendedor, 2, length(cidade_vendedor)))
where cidade_vendedor IS NOT NULL;
```
Essa etapa de padronização é fundamental em processos de análise de dados, pois:

* Evita duplicidade de informações causadas por variações de escrita
* Facilita filtros, agrupamentos e análises
* Melhora a qualidade e confiabilidade dos dados



### Prática 2: Calcular ano ou mês das compras

Utilizando tabela pedidos

Nesta etapa, foram realizadas consultas para extrair informações da coluna data_hora_compra da tabela pedidos. 
O objetivo foi analisar os dados ao longo do tempo, como identificar o ano, mês e a quantidade de compras realizadas.

Primeiramente, foi feita uma consulta simples para visualizar os valores da coluna de data.
Essa etapa permite entender o formato dos dados armazenados (timestamp) antes de realizar transformações.

```sql
select data_hora_compra from pedidos;
```
Em seguida, foi utilizado o comando EXTRACT para obter apenas o ano da data.
Essa consulta permite identificar em qual ano cada pedido foi realizado.

```sql

/*Mostra ano das compras*/

select 
    id_pedido,
    cast(extract (year from data_hora_compra) as text )as ano_compra
from pedidos;

/*Mostra ano e mês juntos*/

select
    id_pedido,
    cast(extract(year from data_hora_compra) as text) as ano_compra,
    extract(month from data_hora_compra) as mes_compra
from pedidos;

/*Agora, ano e mês juntos utilizando data part*/

select
    id_pedido,
    cast(date_part('year', data_hora_compra) as text) as ano_compra,
    date_part('month', data_hora_compra) as mes_compra
from pedidos;

/*Agora, irei agrupar compras por ano*/

select
    cast(extract(year from data_hora_compra) as text) as ano,
    count(*) as total_compras
from pedidos
group by ano
order by ano;
```

* date_part tem a mesma função que extract,
* O uso de cast foi necessário pois o retorno pode ser do tipo numérico (float), 
e foi desejado trabalhar com texto,
* count(*) → conta o número total de registros (compras),
* group by ano → agrupa os dados por ano,
* order by ano → organiza os resultados em ordem crescente.

Rodando esse código, é possível observar que no ano de 2016, o total de compras foi igual a 329, muito pequeno
se comparado com o ano de 2017 e 2018, que tiveram respectivamente 45.101 e 54.001 compras no total.

Logo mais, vou agrupar compras por mês a fim de observar alguma diferença no nosso conjunto de dados.

```sql

select
    extract (month from data_hora_compra) as mes,
    count(*) as total_compras
from pedidos
group by mes
order by total_compras DESC;
```
Com isso, podemos observar que a maior quantidade de compras feitas ocorre no mês 8 ( Agosto ).

Ademais, é perceptível que o ano com o maior total de compras foi em 2018 e o mês com o maior total de 
compras foi em agosto.

Em fim, irei descobrir em qual ano e mês em conjunto tiveram o maior total de compras feitas.

```sql
select
    cast(extract(year from data_hora_compra) as text) as ano,
    extract (month from data_hora_compra) as mes,
    count(*) as total_compras
from pedidos
group by
    extract(year from data_hora_compra),
    extract(month from data_hora_compra)
order by total_compras desc
limit 1;
```

Portando, ocorreu no ano de 2017 no mês 11 (novembro), com o total de compras igual a 7.544,



### Prática 3: Calcular diferenças entre datas de pedido e entrega

Utilizando a tabela pedidos:

Nesta etapa, foi realizada uma análise das datas de compra e entrega dos pedidos,
com o objetivo de identificar possíveis valores nulos e calcular o tempo de entrega em dias e meses.

```sql
select data_hora_compra, data_entrega_cliente from pedidos;

select
    sum(case when data_hora_compra is null then 1 else 0 end) as data_hora_compra_nulos,
    sum(case when  data_entrega_cliente is null or data_entrega_cliente = '' then 1 else 0 end) as entrega_cliente_nulos
from pedidos;
```

* case when then 1 else 0 end → cria uma contagem condicional
* sum → soma os valores, resultando no total de nulos
* Também foi considerado '' (string vazia) como valor inválido

Essa etapa é importante para garantir a qualidade dos dados antes das análises.

Com esse código, foi possível observar que na coluna data_hora_compra não há valores nulos, 
enquanto na coluna data_entrega_cliente existem 2.965 valores nulos.
Esses valores indicam pedidos que ainda não possuem data de entrega registrada, 
impossibilitando o cálculo da diferença entre a data de compra e a data de entrega.

Por esse motivo, para dar continuidade ao cálculo das diferenças entre datas de pedido e entrega,
serão considerados apenas os registros que possuem valores válidos na coluna data_entrega_cliente,
evitando distorções nos resultados da análise.

Calcula a diferença entre a data da compra e a data da entrega ao cliente em dias:

```sql
SELECT 
    id_pedido,data_hora_compra, data_entrega_cliente,
    data_entrega_cliente::date - data_hora_compra::date AS dias_entrega
FROM pedidos
WHERE NULLIF(data_entrega_cliente, '') IS NOT NULL;
```
* ::date → converte o valor para tipo data
* A subtração entre datas retorna a diferença em dias
* NULLIF(data_entrega_cliente, '') → transforma valores vazios em NULL
* O WHERE garante que apenas registros válidos sejam considerados

Calcula a diferença em meses da data da compra e da data de entrega ao cliente:

```sql
select
    id_pedido,data_hora_compra, data_entrega_cliente,
    extract(year from AGE(data_entrega_cliente::timestamp, data_hora_compra)) * 12 +
    extract(month from AGE(data_entrega_cliente::timestamp, data_hora_compra)) as meses_entrega
from pedidos
where NULLIF(data_entrega_cliente, '') is not null;
```

* AGE(data_entrega, data_compra) → calcula a diferença entre datas
* extract(year ...) * 12 → converte anos em meses
* extract(month ...) → obtém os meses restantes
* A soma resulta no total de meses de entrega

Calculando a diferença em dias e meses juntos:

```sql
select
    id_pedido, data_hora_compra, data_entrega_cliente,
    data_entrega_cliente::date - data_hora_compra::date as dias_entrega,
    extract(year from age(data_entrega_cliente::timestamp, data_hora_compra)) * 12 +
    extract(month from age(data_entrega_cliente::timestamp, data_hora_compra)) as meses_entrega
from pedidos
where nullif(data_entrega_cliente, '') is not null;
```
Essa análise permite avaliar a eficiência do processo de entrega, possibilitando:

* Medir o tempo médio de entrega
* Identificar atrasos ou padrões anormais

### Prática 4: Arredondar valores de pagamento

Nesta etapa, foi realizada uma análise da coluna valor_pagamento da tabela pagamentos_pedidos,
com o objetivo de visualizar e padronizar os valores monetários.

Utilizando a tabela pagamentos pedidos

```sql

select valor_pagamento from pagamentos_pedidos;

select
    id_pedido,
    valor_pagamento,
    round(valor_pagamento, 1) as valor_arredondado
from pagamentos_pedidos;
```
* round(valor_pagamento, 1) → arredonda o valor para 1 casa decimal
* id_pedido → permite identificar a qual pedido o valor pertence
* valor_pagamento → valor original
* valor_arredondado → novo valor após arredondamento

Essa etapa contribui para melhorar a organização e a apresentação dos dados financeiros,
tornando as análises mais claras e consistentes.
