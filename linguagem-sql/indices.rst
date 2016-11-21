INDICES
=======

- Criação de índices e estatísitcas

Os índices garantem um bom desempenho para as consultas que serão realizadas no banco de dados.
Comece verificando com a procedure sp_help os metadados das tabelas para verificar se não existe um índice
que possa ajudar na sua consulta.

Caso precise criar um índice comece analisando os campos que estão na sua cláusula WHERE.
Esses campos são conhecidos como predicados.
Ainda dentro da cláusula WHERE procure filtrar primeiramente os campos com maior seletividade, que
possam filtar os dados de forma que não sejam trazidos ou pesquisados dados descessários.

Em seguida olhe os campos da cláusula SELECT e adicione eles no índice.

- Atenção Leia o material complementar na biblioteca Virtual

- Exemplo

A consulta abaixo busca nome e data de nascimentos do cliente com base em uma data passada pelo usuário ou sistema.
Como primeiro passo vamos olhar a cláusula WHERE e em seguida a cláusula SELECT.
Dessa forma temos um índice que deverá conter ClienteNascimento e ClienteNome onde ClienteNascimento é o predicado.

Comando

.. code-block:: sql
  :linenos:

	SELECT Clientes.ClienteNome, Clientes.ClienteNascimento
	FROM Clientes
	WHERE ClienteNascimento >= '1980-01-01'

Índice

.. code-block:: sql
  :linenos:

	CREATE INDEX IX_NOME ON Clientes
	(
		ClienteNascimento,
		ClienteNome 
	)
