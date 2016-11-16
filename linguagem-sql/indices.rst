INDICES
=======

- Criação de índices e estatísitcas

.. code-block:: sql
  :linenos:

CREATE NONCLUSTERED INDEX [IX_NOME] ON [dbo].[Clientes]
(
	[ClienteNascimento],
	[ClienteNome] 
)
GO
