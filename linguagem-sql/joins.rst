JOINS
======

- Existem diversos tipos de JOINS.

  .. code-block:: sql
    :linenos:

    SELECT * FROM Clientes 
              JOIN Contas 
                on Clientes.ClienteCodigo=Contas.ClienteCodigo;
    
      SELECT * FROM CLIENTES 
              INNER JOIN  Contas 
                  ON Clientes.ClienteCodigo=Contas.ClienteCodigo;

- lEFT JOIN.

  .. code-block:: sql
    :linenos:

    SELECT * FROM CLIENTES LEFT JOIN 
      Emprestimos ON Clientes.ClienteCodigo=Emprestimos.ClienteCodigo;

    
- RIGHT    

  .. code-block:: sql
    :linenos:

    SELECT * FROM CartaoCredito RIGHT JOIN Clientes ON CartaoCredito.ClienteCodigo=Clientes.ClienteCodigo;

- FULL
  
  .. code-block:: sql
    :linenos:
      
    SELECT * FROM CartaoCredito FULL OUTER JOIN Clientes ON CartaoCredito.ClienteCodigo=Clientes.ClienteCodigo;

- CROSS
  
  .. code-block:: sql
    :linenos:

    SELECT * FROM CLIENTES CROSS JOIN Contas;
