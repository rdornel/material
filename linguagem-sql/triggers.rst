TRIGGERS
========

- Comando vinculado a uma tabela que executa um ação assim que algum comando de UPDATE, INSERT ou DELETE é executado na tabela onde a trigger está vinculada.

Trigger para INSERT
-------------------

.. code-block:: sql
  :linenos:

  CREATE TRIGGER trgINSERT_CLIENTE
  ON Clientes
  FOR INSERT
  AS
  BEGIN
  INSERT clientes_audit 
  SELECT *,[TRG_OPERACAO] = 'INSERT', [TRG_DATA]=GETDATE(), [TRG_FLAG]='NEW' FROM Inserted
  END;

  
Trigger para DELETE
-------------------

.. code-block:: sql
  :linenos:
 
  CREATE TRIGGER trgDELETE_CLIENTE
  ON dbo.Clientes
  FOR DELETE
  AS
  BEGIN
  INSERT dbo.clientes_audit SELECT *,[TRG_OPERACAO] = 'DELETE',[TRG_DATA]=GETDATE(),[TRG_FLAG]='OLD' FROM Deleted
  END;

.. code-block:: sql
  :linenos:

Trigger para UPDATE
-------------------

  CREATE TRIGGER trgUPDATE_CLIENTE
  ON dbo.Clientes
  FOR UPDATE
  AS
  BEGIN
  INSERT dbo.clientes_audit SELECT *,[TRG_OPERACAO] = 'UPDATE',[TRG_DATA]=GETDATE(),[TRG_FLAG]='OLD' FROM Deleted
  INSERT dbo.clientes_audit SELECT *,[TRG_OPERACAO] = 'UPDATE',[TRG_DATA]=GETDATE(),[TRG_FLAG]='NEW' FROM Inserted
  END
