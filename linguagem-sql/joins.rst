JOINS
======

- Existem diversos tipos de JOINS. O mais tradicional e restritivo é o JOIN ou INNER JOIN que requer que o registros usado na comparação exista em ambas as tabelas.
No exemplo abaixo, o ClienteCodigo não poderá ser vazio em nenhuma das tabelas envolvidas, caso isso ocorra, aquela linha não será retornada no resultado.

.. figure:: http://i.stack.imgur.com/1Tfy0.jpg
   :scale: 30 %
   :alt: map to buried treasure

`Fonte da imagem: Representação Visual das Joins  <http://www.codeproject.com/Articles/33052/Visual-Representation-of-SQL-Joins/>`_

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

    SELECT ClienteNome, ContaSaldo, 
		CASE WHEN CartaoCodigo IS NULL THEN 'LIGAR' ELSE 'NÃO INCOMODAR' END AS 'NN'
	FROM Clientes 
	INNER JOIN Contas
		ON (Contas.ClienteCodigo = Clientes.ClienteCodigo)
	LEFT JOIN dbo.CartaoCredito
		ON (CartaoCredito.ClienteCodigo = Clientes.ClienteCodigo)

    
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


