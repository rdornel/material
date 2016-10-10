TRIGGERS
========

- Comando vinculado a uma tabela que executa um ação assim que algum comando de UPDATE, INSERT ou DELETE é executado na tabela onde a trigger está vinculada.

.. code-block:: sql
  :linenos:

  CREATE TRIGGER [NOME DO TRIGGER]
  ON [NOME DA TABELA]
  [FOR/AFTER/INSTEAD OF] [INSERT/UPDATE/DELETE]
  AS
	--CORPO DO TRIGGER
