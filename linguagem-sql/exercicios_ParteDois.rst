EXERCÍCIOS Parte 2
==================

1. Faça um consulta que retorne o nome e sobrenome do cliente, seu bairro, e os valores das suas movimentações, a data ordenando as movimentações pelas mais recentes.

  .. code-block:: sql
    :linenos:

    SELECT ClienteNome, ClienteSobrenome, ClienteBairro, MovimentoData, MovimentoValor
      FROM Clientes, Contas, Movimentos
      WHERE Clientes.ClienteCodigo=Contas.ClienteCodigo
      AND Contas.ContaNumero=Movimentos.ContaNumero 
      ORDER BY MovimentoData desc);

2. Mostre o nome do cliente, sobrenome e a sua renda convertida em dolar e euro.

  .. code-block:: sql
    :linenos:

    SELECT ClienteNome, ClienteSobrenome, 
       (ClienteRendaAnual / 3.9) AS Dolar, (ClienteRendaAnual / 4.4) AS Euro 
	   FROM Clientes;
