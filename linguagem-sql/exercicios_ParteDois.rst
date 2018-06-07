EXERCÍCIOS Parte 2
==================

1. Faça um consulta que retorne o nome e sobrenome do cliente, seu bairro, e os valores das suas movimentações, a data ordenando as movimentações pelas mais recentes.

  .. code-block:: sql
    :linenos:

    SELECT ClienteNome, ClienteSobrenome, ClienteBairro, MovimentoData, MovimentoValor
      FROM Clientes, Contas, Movimentos
      WHERE Clientes.ClienteCodigo=Contas.ClienteCodigo
      AND Contas.ContaNumero=Movimentos.ContaNumero 
      ORDER BY MovimentoData desc;

2. Mostre o nome do cliente, sobrenome e a sua renda convertida em dolar e euro.

  .. code-block:: sql
    :linenos:

    SELECT ClienteNome, ClienteSobrenome, 
       (ClienteRendaAnual / 3.9) AS Dolar, (ClienteRendaAnual / 4.4) AS Euro 
	   FROM Clientes;

	   
3. Traga o nome dos clientes, o sobrenome, o bairro, o estado civil (descrito), o sexo (descrito) e classifique o cliente de acordo com a sua renda anual, C tem renda menor que 50.000, B tem renda menor que 70.000 e A tem renda acima de 70.000.

  .. code-block:: sql
    :linenos:

    SELECT ClienteNome, ClienteSobrenome, 
       (ClienteRendaAnual / 3.9) AS Dolar, (ClienteRendaAnual / 4.4) AS Euro 
	   FROM Clientes;
	   
4. Liste todos os clientes que moram no mesmo bairro das agências do banco.

  .. code-block:: sql
    :linenos:

    SELECT ClienteNome, ClienteBairro, AgenciaBairro, AgenciaNome FROM Clientes, Agencias
      WHERE ClienteBairro=AgenciaBairro;
	
5. Mostre todos os clientes possuem número no seu e-mail.

   .. code-block:: sql
    :linenos:

    SELECT Clientes.ClienteNome, Clientes.ClienteEmail
      FROM dbo.Clientes
      WHERE Clientes.ClienteEmail LIKE '%[0-9]%';
	
6. Mostre todos os clientes em que o nome da rua começa começa com R. e não com RUA.

   .. code-block:: sql
    :linenos:

    SELECT ClienteRua FROM dbo.Clientes WHERE
      ClienteRua LIKE 'R.%'
      AND ClienteRua NOT LIKE 'RUA%' ;
