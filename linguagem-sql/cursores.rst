CURSORES
========

- Cursor.


 Exemplo de um Cursor:

.. code-block:: sql
  :linenos:

  DECLARE @ClienteNome VARCHAR(50);

	DECLARE [cursorListaCliente] CURSOR FOR 
	SELECT
     Clientes.ClienteNome as Nome
	FROM Clientes

	OPEN [cursorListaCliente]
	FETCH NEXT FROM [cursorListaCliente] INTO @ClienteNome
	WHILE @@FETCH_STATUS = 0
	BEGIN
	SELECT @ClienteNome;
	FETCH NEXT FROM [cursorListaCliente] INTO @ClienteNome
	END
	CLOSE [cursorListaCliente];
   DEALLOCATE [cursorListaCliente];

