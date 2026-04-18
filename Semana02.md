# Panda-SQL

## Semana 02

O comando **SELECT** é o ponto de partida do SQL e é utilizado para consultar dados em uma tabela. 
Ele permite escolher quais informações serão exibidas no resultado da consulta.
A estrutura básica é `SELECT coluna FROM tabela`, onde o `SELECT` define o que será mostrado e o `FROM` indica de qual tabela os dados serão retirados.
Quando usamos `SELECT *`, estamos selecionando todas as colunas da tabela.

O `FROM` é a parte do comando SQL que especifica a origem dos dados.
Ele indica a tabela ou conjunto de tabelas de onde as informações serão buscadas. 
Sem o `FROM`, o SQL não sabe de onde extrair os dados. Em consultas mais complexas, o `FROM` também pode ser usado com junções entre tabelas,
mas no nível básico ele apenas define a tabela principal da consulta.

O `AS` é utilizado para criar **apelidos (aliases)** para colunas ou tabelas.
Ele não altera os dados, apenas muda o nome exibido no resultado da consulta, tornando a leitura mais clara.
Por exemplo, `SELECT nome AS nome_cliente FROM clientes` faz com que a coluna “nome” apareça como “nome_cliente”.
Isso é muito útil para relatórios e organização dos resultados.

O `WHERE` é utilizado para filtrar registros, ou seja, ele define condições que os dados devem atender para aparecer no resultado.
Ele funciona junto com operadores de comparação como `=`, `<>`, `>`, `<`, `>=` e `<=`. Esses operadores permitem comparar valores, por exemplo,
encontrar clientes com idade maior que 18 ou produtos com preço igual a um determinado valor. O `WHERE` é essencial para restringir consultas e
trabalhar apenas com dados relevantes.

Os operadores lógicos `AND`, `OR` e `NOT` são usados para combinar ou negar condições dentro do `WHERE`.
O `AND` exige que todas as condições sejam verdadeiras ao mesmo tempo, enquanto o `OR` exige que pelo menos uma condição seja verdadeira.
Já o `NOT` inverte a condição, ou seja, retorna resultados que não atendem ao critério especificado.
Esses operadores permitem criar filtros mais complexos e precisos.

O operador `LIKE` é utilizado para buscar padrões em textos. Ele é muito útil quando não se sabe o valor exato, mas se conhece parte dele.
O símbolo `%` representa qualquer sequência de caracteres, enquanto `_` representa um único caractere. Por exemplo, `LIKE 'S%'`
retorna valores que começam com “S”, e `LIKE '%a'` retorna valores que terminam com “a”. Ele é muito usado em buscas flexíveis.

O operador `IN` é usado para verificar se um valor pertence a uma lista de valores.
Ele substitui várias condições com `OR`, tornando a consulta mais simples e legível. Por exemplo, `WHERE estado IN ('SP', 'RJ', 'MG')`
retorna registros que pertencem a qualquer um desses estados. Ele é bastante útil quando se trabalha com múltiplas opções fixas.

O operador `BETWEEN` é utilizado para filtrar valores dentro de um intervalo.
Ele funciona tanto com números quanto com datas. A sintaxe `BETWEEN valor1 AND valor2` retorna todos os registros que estão entre esses dois valores,
incluindo os limites. Por exemplo, ele pode ser usado para buscar pedidos entre duas datas ou produtos com preço entre dois valores.
É uma forma prática de trabalhar com intervalos sem precisar usar vários operadores de comparação.


### Prática 1: Filtrar clientes por estado

Utilizando a tabela clientes.
Para observar as colunas id do cliente e estado do cliente da tabela clientes:

```sql
select id_cliente, estado_cliente from clientes;
```

Mostra todos os estados que são diferentes de SP:

```sql
select estado_cliente from clientes
where estado_cliente != 'SP';
```

Mostra todos clientes que tem SC como estado:

```sql
select id_cliente, estado_cliente from clientes 
where estado_cliente = 'SC';
```

Mostra todos os clientes que tem o estado começado com a letra S e seguido por qualquer caractere:

```sql
select id_cliente, estado_cliente from clientes 
where estado_cliente like 'S%';
```


### Prática 2: Buscar produtos de uma categoria específica

Utilicando a tabela produtos:

```sql
select * from produtos;

/*Seleciona todos as categorias que tem na coluna nome_categoria_produto sem repeti-las:*/

select distinct nome_categoria_produto from produtos;


/*Seleciona todas as categorias que tem na coluna nome_categoria_produto sem repeti-las de forma ordenada alfabeticamente:*/

select distinct nome_categoria_produto from produtos
order by nome_categoria_produto;

/*Seleciona todos os ids de produtos que estão na categoria de perfumaria:*/

select id_produto , nome_categoria_produto from produtos 
where nome_categoria_produto = 'perfumaria';

/*Seleciona todos os ids de produtos que não estão na categoria de perfumaria:*/

select id_produto, nome_categoria_produto from produtos
where nome_categoria_produto <>  'perfumaria';

/*Seleciona todos as categorias de produtos que são diferente de perfumaria mais que não são iguais a categoria papelaria, com repetição:*/

select nome_categoria_produto from produtos
where nome_categoria_produto <> 'perfumaria'and nome_categoria_produto <> 'papelaria';

/*Seleciona todas as categorias de produtos que são diferentes de papelaria, moveis decoração e perfumaria:*/

select nome_categoria_produto from produtos
where nome_categoria_produto not in ('papelaria', 'moveis_decoracao', 'perfumaria');

/*Seleciona todas as categorias que são diferentes de perfumaria sem repiti-las:*/

select distinct nome_categoria_produto from produtos
where nome_categoria_produto <> 'perfumaria';

/*Seleciona os ids de produtos que são iguais a perfumaria ou iguais a moveis de decoração:*/

select id_produto, nome_categoria_produto from produtos
where nome_categoria_produto = 'perfumaria' or nome_categoria_produto = 'moveis_decoracao';
```


### Prática 3: Filtrar pedidos entre dois valores ou datas

Utilizando a tabela pedidos:

```sql
select * from pedidos;

/*Seleciona os pedidos com as datas de entrega ao cliente entre o intervalo de 2017/10/04 a 2018/0/04, ou seja, no período de um ano de forma ordenada:*/

select id_pedido, data_entrega_cliente from pedidos
where data_entrega_cliente between '2017-10-04' and '2018-10-04'
order by data_entrega_cliente ASC;

/*Seleciona os pedidos com a data de entrega estimada entre o intervalo de 2027/05/09 a 2019/05/09, ou seja, um período de dois anos de forma ordenada:*/

select id_pedido, data_estimada_entrega from pedidos
where data_estimada_entrega between '2017-05-09' and '2019-05-09'
order by data_estimada_entrega asc;
```


### Prática 4: Encontrar padrões em nomes de cidades ou categorias

Utilizando a tabela geolocalização:

```sql
select * from geolocalizacao;

/*Seleciona todas as cidades sem repeti-las com o padrão de terminação em ão*/
select distinct cidade_geolocalizacao from geolocalizacao
where cidade_geolocalizacao like '%ao';

/*Seleciona todas as cidades sem repeti-las com o padrão de inicio igual são e sem o ter a letra i no nome*/
select distinct cidade_geolocalizacao from geolocalizacao
where cidade_geolocalizacao like 'sao%' and cidade_geolocalizacao not like '%i%';
```



