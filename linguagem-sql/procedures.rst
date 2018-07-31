PROCEDURES
==========

- Uma procedure é um bloco de comandos ou instruções SQL organizados para executar uma ou mais tarefas. Ela pode ser utilizada para ser acionada através de uma chamada simples que executa uma série de outros comandos.

.. code-block:: sql
  :linenos:

  CREATE PROCEDURE uspRetornaIdade   
  @CodigoCliente int
  AS   
  SELECT Clientes.ClienteNome, YEAR(GETDATE())-YEAR(ClienteNascimento) AS IDADE
  FROM Clientes
  INNER JOIN Contas ON Clientes.ClienteCodigo=Contas.ClienteCodigo
  WHERE Clientes.ClienteCodigo = @CodigoCliente;
  
- Execução da procedure, opção 1

.. code-block:: sql
  :linenos:

  exec uspRetornaIdade 1;
  
- Execução da procedure, opção 2

.. code-block:: sql
  :linenos:
  
  declare @parametro int
  set @parametro = 1 --Código do Cliente desejado
  exec uspRetornaIdade @parametro;

IF
--

- Comando utilizado para checar condições.

.. code-block:: sql
  :linenos:

  CREATE PROCEDURE uspRetornaSeTemCartao   
  @CodigoCliente int
  AS
  BEGIN

  DECLARE @CodigoClienteCartao INT 

  SET @CodigoClienteCartao = (SELECT CartaoCredito.ClienteCodigo FROM Clientes LEFT JOIN CartaoCredito
  ON CartaoCredito.ClienteCodigo = Clientes.ClienteCodigo WHERE CartaoCredito.ClienteCodigo = @CodigoCliente)
	  
	  IF @CodigoClienteCartao IS NULL
		  BEGIN
		  SELECT * FROM CartaoCredito WHERE ClienteCodigo = @CodigoCliente;
		  END
	  ELSE
		  BEGIN
		  SELECT 'LIGAR', * FROM Clientes WHERE ClienteCodigo = @CodigoCliente
		  END
  
  END;

  EXEC uspRetornaSeTemCartao @CodigoCliente = 25; -- TEM CARTÃO

  EXEC uspRetornaSeTemCartao @CodigoCliente = 1; --NÃO TEM CARTÃO


WHILE
-----

- Comando utilizado para realizar laços de repetição.  

.. code-block:: sql
  :linenos:

  DECLARE @contador INT
  SET @contador = 1
  WHILE @contador <= 5
  BEGIN
    SELECT @contador
    SET @contador = @contador + 1
  END