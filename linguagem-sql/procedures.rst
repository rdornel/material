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
  
