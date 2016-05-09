INSERT
======

- Comando utilizando para popular as tabelas no banco.

  .. code-block:: sql
    :linenos:

    INSERT Clientes (ClienteCodigo, ClienteNome) VALUES (1, 'Nome do Cliente');
    
    INSERT Clientes (colunas) VALUES (valores)
    
    INSERT INTO Clientes SELECT * FROM ...
