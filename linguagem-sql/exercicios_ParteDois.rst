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

    SELECT ClienteNome, ClienteSobrenome, ClienteBairro, ClienteEstadoCivil,
		CASE WHEN ClienteEstadoCivil = 'S' THEN 'Solteiro' ELSE 'Casado' END AS ESTADOCIVILDECRITO,
		ClienteSexo,
		CASE WHEN ClienteSexo = 'M' THEN 'Masculino' ELSE 'Feminino' END AS SEXODESCRITO,
		ClienteRendaAnual,
		CASE WHEN ClienteRendaAnual < 50000 THEN 'C' 
		WHEN ClienteRendaAnual < 70000 THEN 'B'
		ELSE 'A'
		END AS 'CLASSIFICAÇÃO'
		FROM Clientes ;
	   
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
	  
7. Mostre o nome do cliente e a renda apenas do 5 melhores clientes com base na sua renda.

8. Mostre o nome do cliente e a renda apenas do 5 piores clientes com base na sua renda.

9. Mostre oo nome e a rua dos clientes que moram em residencias cujo número está entre 300 e 500.

10. Utilizando o conceito de sub consulta mostre quais clientes não possuem cartão de crédito.

11. Mostre o nome do cliente, o nome da agêmncia e o bairro da agência, as movimentações do clientes e o limite do cartão de crédito dele somente para os clientes em que a conta foi aberta a partir de 2008.


