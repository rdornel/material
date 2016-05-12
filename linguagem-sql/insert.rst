INSERT
======

- Comando utilizando para popular as tabelas no banco.

O comando INSERT também possui algumas variações que devem ser respeitadas para evitar problemas.
O primeiro exemplo abaixo mostra a inserção na tabela Clientes. Repare que logo abaixo tem um fragmento da criação 
da tabela Clientes mostando que o campo ClienteCodigo é IDENTITY, portanto não deve ser informado no momento do INSERT.

  .. code-block:: sql
    :linenos:

    INSERT Clientes (ClienteNome) VALUES ('Nome do Cliente');
    
    create table Clientes
    (
    ClienteCodigo int IDENTITY CONSTRAINT PK_CLIENTES PRIMARY KEY,
    
    INSERT Clientes (colunas) VALUES (valores)
    
    INSERT INTO Clientes SELECT * FROM ...
