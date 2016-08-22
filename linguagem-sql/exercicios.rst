EXERCÍCIOS
==========

1. Mostre quais os clientes tem idade superior a média.

  .. code-block:: sql
    :linenos:

    SELECT ClienteNome, YEAR(GETDATE()) - YEAR(ClienteNascimento) AS idade
      FROM Clientes
      WHERE YEAR(GETDATE()) - YEAR(ClienteNascimento) >
        (
          SELECT AVG(YEAR(GETDATE()) -YEAR(ClienteNascimento)) AS IDADE FROM Clientes
        );

2. Mostre qual agência tem quantidade de clientes acima da média.

  .. code-block:: sql
    :linenos:

    SELECT AgenciaNome, COUNT(ClienteCodigo) AS QDTE
    FROM Contas INNER JOIN Agencias
      ON Agencias.AgenciaCodigo = Contas.AgenciaCodigo
    GROUP BY AgenciaNome
    HAVING COUNT(ClienteCodigo) >
      (SELECT COUNT(DISTINCT ClienteCodigo)/
      COUNT(DISTINCT AgenciaCodigo) FROM Contas);

3. Mostre o nome da agência o saldo total, o mínimo, o máximo e a quantidade de clientes de cada agência.

  .. code-block:: sql
    :linenos:
	
	SELECT AgenciaNome, SUM(ContaSaldo) AS TOTAL ,MIN(ContaSaldo) AS MINIMO, MAX(ContaSaldo) AS MAXIMO, 
	COUNT(Contas.ClienteCodigo) AS QTDE_CLIENTES
	FROM Contas INNER JOIN dbo.Agencias ON Agencias.AgenciaCodigo = Contas.AgenciaCodigo
	GROUP BY dbo.Agencias.AgenciaNome
	--ATENCAO AQUI PARA COUNT(*) E COUNT(DISTINT)


4. Mostre o percentual que cada agencia representa no saldo total do banco.

  .. code-block:: sql
    :linenos:

	SELECT AgenciaNome, SUM(ContaSaldo) / (SELECT SUM(ContaSaldo) FROM dbo.Contas) * 100 AS PERCENTUAL
	FROM Contas INNER JOIN dbo.Agencias ON Agencias.AgenciaCodigo = Contas.AgenciaCodigo
	GROUP BY dbo.Agencias.AgenciaNome

5. Mostre as duas cidades que tem o maior saldo total

  .. code-block:: sql
    :linenos:

	SELECT TOP 2 AgenciaCidade, SUM(ContaSaldo) AS SALDO_TOTAL
	FROM Contas INNER JOIN Agencias ON Agencias.AgenciaCodigo = Contas.AgenciaCodigo
	GROUP BY AgenciaCidade
	ORDER BY 2 DESC

6. Mostre qual a agência tem o maior montante de emprestimo


  .. code-block:: sql
    :linenos:

	SELECT TOP 1 AgenciaCidade, Emprestimos.EmprestimoTotal 
	FROM dbo.Emprestimos INNER JOIN Agencias ON Agencias.AgenciaCodigo = Emprestimos.AgenciaCodigo
	ORDER BY 2 DESC


7. Mostre qual o menor valor devido, o maior e o total devido da tabela devedor

  .. code-block:: sql
    :linenos:

	SELECT MIN(DevedorSaldo) AS MINIMO, MAX(DevedorSaldo) AS MAXIMO, SUM(DevedorSaldo) AS TOTAL 
	FROM dbo.Devedores

8. Mostre o nome do cliente, se ele tem cartão de crédito, apenas do cliente que é o maior devedor.


  .. code-block:: sql
    :linenos:

	SELECT TOP 1 --Experimente remover o TOP 1 para conferir o resultado
	ClienteNome
	,CASE WHEN dbo.CartaoCredito.ClienteCodigo IS NULL THEN 'NÃO TEM CARTÃO CRÉDITO' ELSE 'TEM CARTÃO CRÉDITO' END AS 'CARTAO'
	,DevedorSaldo FROM dbo.Clientes 
	INNER JOIN dbo.Devedores ON Devedores.ClienteCodigo = Clientes.ClienteCodigo
	LEFT JOIN dbo.CartaoCredito ON CartaoCredito.ClienteCodigo = Clientes.ClienteCodigo
	ORDER BY 2 DESC
