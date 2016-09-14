FUNÇÕES
=======

- Uma função é ...

.. code-block:: sql
  :linenos:

  CREATE FUNCTION fnRetornaAno (@data DATETIME)
  RETURNS int
  AS
    BEGIN
    DECLARE @ano AS int
    SELECT @ano = YEAR(@data)
    
	RETURN @ano
  
  END

- Chamada ou execução da função

.. code-block:: sql
  :linenos:
  
  SELECT dbo.fnRetornaAno(GETDATE())

  SELECT dbo.fnRetornaAno(Clientes.ClienteNascimento) FROM dbo.Clientes