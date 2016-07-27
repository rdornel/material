JOINS
======

- Existem diversos tipos de JOINS. O mais tradicional e restritivo é o JOIN ou INNER JOIN que requer 
que o registros usado na comparação exista em ambas as tabelas.
No exemplo abaixo, o ClienteCodigo não poderá ser vazio em nenhuma das tabelas envolvidas, caso isso ocorra,
aquela linha não será retornada no resultado.

- Representação visual das Joins

`Representação visual das Joins <http://www.codeproject.com/Articles/33052/Visual-Representation-of-SQL-Joins/>`_

http://www.codeproject.com/Articles/33052/Visual-Representation-of-SQL-Joins

  .. code-block:: sql
    :linenos:

    SELECT * FROM Clientes 
              JOIN Contas 
                on Clientes.ClienteCodigo=Contas.ClienteCodigo;
    
      SELECT * FROM CLIENTES 
              INNER JOIN  Contas 
                  ON Clientes.ClienteCodigo=Contas.ClienteCodigo;

- LEFT JOIN.

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
