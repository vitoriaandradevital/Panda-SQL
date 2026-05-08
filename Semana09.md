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
('Daniel', 'Araraquara', 'daniel@gmail.com'),
('Eduarda', 'Sorocaba', 'eduarda@gmail.com'),
('Felipe', 'Santos', 'felipe@gmail.com'),
('Gabriela', 'Bauru', 'gabriela@gmail.com'),
('Henrique', 'Piracicaba', 'henrique@gmail.com'),
('Isabela', 'São Carlos', 'isabela@gmail.com'),
('João', 'Limeira', 'joao@gmail.com'),
('Karen', 'Franca', 'karen@gmail.com'),
('Lucas', 'Jundiaí', 'lucas@gmail.com'),
('Mariana', 'Americana', 'mariana@gmail.com'),
('Nicolas', 'Osasco', 'nicolas@gmail.com'),
('Olivia', 'Barretos', 'olivia@gmail.com'),
('Paulo', 'Taubaté', 'paulo@gmail.com'),
('Quezia', 'Guarulhos', 'quezia@gmail.com'),
('Rafael', 'Mogi Guaçu', 'rafael@gmail.com'),
('Sabrina', 'São José dos Campos', 'sabrina@gmail.com'),
('Thiago', 'Mauá', 'thiago@gmail.com'),
('Ursula', 'Diadema', 'ursula@gmail.com'),
('Vinicius', 'Suzano', 'vinicius@gmail.com'),
('William', 'Itu', 'william@gmail.com'),
('Xavier', 'Atibaia', 'xavier@gmail.com'),
('Yasmin', 'Botucatu', 'yasmin@gmail.com'),
('Zeca', 'Cotia', 'zeca@gmail.com'),
('Amanda', 'Indaiatuba', 'amanda@gmail.com'),
('Bianca', 'Paulínia', 'bianca@gmail.com'),
('Caio', 'Marília', 'caio@gmail.com'),
('Débora', 'Jacareí', 'debora@gmail.com'),
('Enzo', 'Caraguatatuba', 'enzo@gmail.com'),
('Fernanda', 'Salto', 'fernanda@gmail.com'),
('Gustavo', 'Rio Claro', 'gustavo@gmail.com'),
('Helena', 'Pindamonhangaba', 'helena@gmail.com'),
('Igor', 'Araçatuba', 'igor@gmail.com'),
('Julia', 'Bragança Paulista', 'julia@gmail.com'),
('Kaique', 'Presidente Prudente', 'kaique@gmail.com'),
('Larissa', 'Mogi das Cruzes', 'larissa@gmail.com'),
('Mateus', 'Avaré', 'mateus@gmail.com'),
('Natália', 'Itapetininga', 'natalia@gmail.com'),
('Otávio', 'Ourinhos', 'otavio@gmail.com'),
('Priscila', 'São Vicente', 'priscila@gmail.com'),
('Renato', 'Catanduva', 'renato@gmail.com'),
('Simone', 'Hortolândia', 'simone@gmail.com'),
('Tales', 'Poá', 'tales@gmail.com'),
('Valentina', 'Guarujá', 'valentina@gmail.com'),
('Wesley', 'Votorantim', 'wesley@gmail.com'),
('Mateus', 'Avaré', 'mateus@gmail.com'),
('Natália', 'Itapetininga', 'natalia@gmail.com'),
('Otávio', 'Ourinhos', 'otavio@gmail.com'),
('Priscila', 'São Vicente', 'priscila@gmail.com'),
('Renato', 'Catanduva', 'renato@gmail.com')

```
### Atualizar informações de registros existentes

```sql

/*Nesse caso, apenas o registro da cliente Maria terá a cidade alterada para Campinas.*/

update teste_cliente
set cidade_teste_cliente = 'Campinas'
where nome_teste_cliente = 'Maria';

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


