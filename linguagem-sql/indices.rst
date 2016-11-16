INDICES
=======

- Criação de índices e estatísitcas

- Atenção Leia o material complementar na biblioteca Virtual

SELECT dbo.Clientes.ClienteNome, dbo.Clientes.ClienteNascimento
FROM dbo.Clientes
WHERE ClienteNascimento >= '1980-01-01'


.. code-block:: sql
  :linenos:

CREATE INDEX [IX_NOME] ON [Clientes]
(
	[ClienteNascimento],
	[ClienteNome] 
)
