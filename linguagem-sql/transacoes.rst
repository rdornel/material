
TRANSAÇÕES
==========

Transações
----------

Comando utilizado para alterar registros em um banco de dados. Antes de executar qualquer comando ``UPDATE``, procure se informar sobre transações (será abordado mais pra frente).
Sempre que for trabalhar com o comando ``UPDATE`` ou ``DELETE``, procure executar um ``SELECT`` antes para validar se os registros que serão afetados, são exatamente aqueles que você deseja.

.. code-block:: sql
  :linenos:

  BEGIN TRAN --> Inicia a transação

  UPDATE dbo.CartaoCredito SET CartaoLimite = CartaoLimite * 1.1

  COMMIT --> Finaliza a transação

  --OR

  ROLLBACK --> Desfaz a transação

Execute primeiro sem o WHERE e verifique que nenhuma linha será alterada. Depois remova o comentário e verá que apenas uma linha foi alterada.

.. code-block:: sql
  :linenos:

  BEGIN TRAN

  UPDATE dbo.CartaoCredito SET CartaoLimite = CartaoLimite * 1.1
  --WHERE ClienteCodigo = '12'

  IF (@@ROWCOUNT > 1 OR @@ERROR > 0)

    ROLLBACK

  ELSE

    COMMIT


Try Catch
---------

.. code-block:: sql
  :linenos:

  BEGIN TRY

    SELECT 1/0

  END TRY

  BEGIN CATCH
    SELECT
        ERROR_NUMBER() AS ErrorNumber,
        ERROR_MESSAGE() AS ErrorMessage;
  END CATCH;
