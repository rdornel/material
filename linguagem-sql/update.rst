UPDATE
======

- Comando utilizado para alterar registros em um banco de dados. Antes de executar qualquer comando ``UPDATE``, procure se informar sobre transações (será abordado mais pra frente).
- Sempre que for trabalhar com o comando ``UPDATE`` ou ``DELETE``, procure executar um ``SELECT`` antes para validar se os registros que serão afetados, são exatamente aqueles que você deseja.

.. code-block:: sql
  :linenos:

  UPDATE CartaoCredito SET CartaoLimite = 1000 WHERE ClienteCodigo = 1;
