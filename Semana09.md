# Panda-sql

## Semana 09  – Manipulação de Dados (DML)

O comando **INSERT** é utilizado para adicionar novos registros em uma tabela do banco de dados. No PostgreSQL,
que pode ser utilizado pelo DBeaver, existem diferentes formas de usar esse comando. A maneira mais comum é inserir
uma única linha especificando as colunas e os valores correspondentes. Também é possível inserir várias linhas em um
único comando, o que torna a operação mais rápida e organizada quando há muitos dados. Além disso, o INSERT pode ser
combinado com consultas SELECT, permitindo copiar dados de uma tabela para outra.

O comando **UPDATE** é responsável por alterar dados já existentes em uma tabela. Com ele, é possível modificar uma
ou mais colunas de determinados registros. Para isso, normalmente utiliza-se a cláusula WHERE, que define quais linhas
serão alteradas. Por exemplo, pode-se atualizar o preço de um produto específico ou mudar o status de um pedido.
No DBeaver, antes de executar um UPDATE, é importante conferir a consulta para garantir que apenas os registros
desejados serão modificados, já que alterações incorretas podem afetar muitos dados ao mesmo tempo.

O comando **DELETE** é utilizado para remover registros de uma tabela. Assim como no UPDATE, a cláusula WHERE é
essencial para definir quais linhas devem ser excluídas. É possível apagar apenas um registro específico ou vários ao mesmo tempo.
Caso o DELETE seja executado sem WHERE, todos os registros da tabela serão removidos, o que pode causar perda de dados importantes.
No DBeaver, é comum utilizar o SELECT antes do DELETE para verificar exatamente quais registros serão afetados pela operação.

Os cuidados com a cláusula **WHERE** em comandos UPDATE e DELETE são fundamentais para a segurança do banco de dados. A ausência
dessa cláusula faz com que todas as linhas da tabela sejam alteradas ou removidas. Por isso, antes de executar esses comandos,
recomenda-se testar a condição utilizando um SELECT com o mesmo WHERE. Dessa forma, é possível confirmar se os registros retornados
são realmente os desejados. 

### Criar uma tabela de testes baseada em dados do banco Brazilian E-Commerce Public Dataset by Olist

```sql

create table teste_cliente (
 id_teste_cliente serial primary key ,
 nome_teste_cliente varchar(50),
 cidade_teste_cliente varchar(50),
 email_teste_cliente varchar(50)
);

select * from teste_cliente;

```
### Inserir novos registros simulando novos clientes ou pedidos

```sql

insert into teste_cliente (nome_teste_cliente, cidade_teste_cliente, email_teste_cliente)
VALUES 
('João', 'Araraquara', 'joao@gmail.com'),
('Ana', 'Ribeirão Preto', 'ana@gmail.com'),
('Carlos', 'Campinas', 'carlos@gmail.com'),
('Ana', 'São Paulo', 'ana@gmail.com'),
('Bruno', 'Campinas', 'bruno@gmail.com'),
('Carla', 'Ribeirão Preto', 'carla@gmail.com'),

```
### Atualizar informações de registros existentes

```sql

/*Nesse caso, apenas o registro da cliente Carla terá a cidade alterada para Campinas.*/

update teste_cliente
set cidade_teste_cliente = 'Campinas'
where nome_teste_cliente = 'Carla';

```

### Remover registros duplicados ou incorretos utilizando consultas com subquery

```sql

select * from teste_cliente
where id_teste_cliente not in (
    select min(id_teste_cliente)
    from teste_cliente
    group by nome_teste_cliente, cidade_teste_cliente, email_teste_cliente
);

delete from teste_cliente
where id_teste_cliente not in (
    select min(id_teste_cliente)
    from teste_cliente
    group by nome_teste_cliente, cidade_teste_cliente, email_teste_cliente
);

select * from teste_cliente;

```


