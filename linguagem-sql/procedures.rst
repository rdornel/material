PROCEDURES
==========

- Uma procedure é um bloco de comandos ou instruções SQL organizados para executar uma ou mais tarefas. Ela pode ser utilizada para ser acionada através de uma chamada simples que executa uma série de outros comandos.

.. code-block:: sql
  :linenos:

  CREATE PROCEDURE uspRetornaSaldo   
  @Nome nvarchar(50)
  AS   
  SELECT Clientes.ClienteNome, Contas.ContaSaldo
  FROM Clientes
  INNER JOIN Contas ON Clientes.ClienteCodigo=Contas.ClienteCodigo
  WHERE Clientes.ClienteNome = @Nome;
  
- Execução da procedure

.. code-block:: sql
  :linenos:

  exec uspRetornaSaldo 'Ana';

IF
--

Comando utilizado para checar condições.

.. code-block:: sql
  :linenos:

  CREATE PROCEDURE uspRetornaSaldo2   
  @Nome nvarchar(50)
  AS
  BEGIN

	  IF @Nome = 'Ana'
		  BEGIN
		  SELECT Clientes.ClienteNome, Contas.ContaSaldo
		  FROM Clientes
		  INNER JOIN Contas ON Clientes.ClienteCodigo=Contas.ClienteCodigo
		  WHERE Clientes.ClienteNome = @Nome;
		  END
	  ELSE
		  BEGIN
		  SELECT @Nome 
		  END
  
  END


WHILE
-----

Comando utilizado para realizar laços de repetição.  

.. code-block:: sql
  :linenos:

  DECLARE @contador INT
  SET @contador = 1
  WHILE @contador <= 5
  BEGIN
    SELECT @contador
    SET @contador = @contador + 1
  END