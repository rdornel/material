SELECT - Nível 2
================

- Existem diversos tipos de JOINS. O mais tradicional e restritivo é o JOIN ou INNER JOIN que requer que o registros usado na comparação exista em ambas as tabelas.
No exemplo abaixo, o ClienteCodigo não poderá ser vazio em nenhuma das tabelas envolvidas, caso isso ocorra, aquela linha não será retornada no resultado.

.. figure:: http://i.stack.imgur.com/1Tfy0.jpg
   :scale: 30 %
   :alt: map to buried treasure

`Fonte da imagem: Representação Visual das Joins  <http://www.codeproject.com/Articles/33052/Visual-Representation-of-SQL-Joins/>`_

  .. code-block:: sql
    :linenos:

    SELECT * FROM Clientes 
              JOIN Contas 
                on Clientes.ClienteCodigo=Contas.ClienteCodigo;
    
      SELECT * FROM CLIENTES 
              INNER JOIN  Contas 
                  ON Clientes.ClienteCodigo=Contas.ClienteCodigo;

- LEFT JOIN.

  .. code-block:: sql
    :linenos:

    SELECT ClienteNome, ContaSaldo, 
	CASE WHEN CartaoCodigo IS NULL THEN 'LIGAR' ELSE 'NÃO INCOMODAR' END AS 'NN'
	FROM Clientes 
	INNER JOIN Contas
	ON (Contas.ClienteCodigo = Clientes.ClienteCodigo)
	LEFT JOIN dbo.CartaoCredito
	ON (CartaoCredito.ClienteCodigo = Clientes.ClienteCodigo);

    
- RIGHT    

  .. code-block:: sql
    :linenos:

    SELECT * FROM CartaoCredito RIGHT JOIN Clientes ON CartaoCredito.ClienteCodigo=Clientes.ClienteCodigo;

- FULL
  
  .. code-block:: sql
    :linenos:
      
    SELECT * FROM CartaoCredito FULL OUTER JOIN Clientes ON CartaoCredito.ClienteCodigo=Clientes.ClienteCodigo;

- CROSS
  
  .. code-block:: sql
    :linenos:

    SELECT * FROM CLIENTES CROSS JOIN Contas;


- As FUNÇÕES DE AGREGAÇÃO, SUM, MIN, MAX, COUNT, AVG permitem um nível mais robusto de informação, criando cojuntos de dados agrupados, médias entre outros, permitindo que possamos resumir e totatlizar comjuntos de resultados. Sempre que usarmos a função de agregação em conjunto com um campo agregador devemos usar a função GROUP BY para indicar qual o compo será o responsável pelo agrupamento das informações.
Caso você deseje comparar conjuntos de informações contidos na função de agragação você deve compará-los usando o HAVING.
  
  .. code-block:: sql
    :linenos:
	
	SELECT TOP 2 AgenciaNome, SUM(ContaSaldo) AS TOTAL
	FROM Contas, dbo.Agencias
	WHERE Agencias.AgenciaCodigo=Contas.AgenciaCodigo
	GROUP BY AgenciaNome 
	HAVING SUM(ContaSaldo) > (SELECT MAX(ContaSaldo) AS VALORMETA FROM Contas AS META)
	ORDER BY 2 DESC
	
	SELECT SUM(dbo.Contas.ContaSaldo),
	AgenciaCodigo, ContaNumero
	FROM Contas
	GROUP BY AgenciaCodigo,ContaNumero
	--WHERE COM AVG ???
	--WHERE COM SUBCONSULTA ???
	HAVING SUM(dbo.Contas.ContaSaldo) > (SELECT AVG(dbo.Contas.ContaSaldo) FROM dbo.Contas);--667,0833
	
	SELECT MAX(ContaSaldo) FROM dbo.Contas;
	SELECT MIN(ContaSaldo) FROM dbo.Contas;
	SELECT AVG(ContaSaldo) FROM dbo.Contas;
	SELECT COUNT(*), COUNT(CONTAS.ClienteCodigo), COUNT(DISTINCT CONTAS.ClienteCodigo) FROM dbo.Contas;


- Variáveis.

- Exists

  .. code-block:: sql
    :linenos:

    SELECT * FROM Clientes;

- SELECT INTO	

- FUNÇÕES DE Data e Hora

  .. code-block:: sql
	:linenos:
		
	SET DATEFORMAT YDM

	SET LANGUAGE PORTUGUESE

	SELECT YEAR(getdate()) -YEAR(dbo.Clientes.ClienteNascimento),

	DATEDIFF(YEAR,ClienteNascimento,GETDATE()),

	DATEPART(yy,ClienteNascimento),

	dateadd(yy,1,ClienteNascimento),

	EOMONTH(GETDATE()),

	DATENAME(MONTH,(GETDATE()))

	FROM dbo.Clientes

  .. code-block:: sql
	:linenos:
	
	SELECT * FROM dbo.Contas 
	WHERE YEAR(ContaAbertura) = '2011'
	ORDER BY ContaAbertura 
	
	
Exercícios
==========

1-Mostre quais os clientes tem idade superior a média.	
  
  .. code-block:: sql
	:linenos:
	
	SELECT ClienteNome, YEAR(GETDATE()) - YEAR(ClienteNascimento) AS idade
	FROM dbo.Clientes
	WHERE YEAR(GETDATE()) - YEAR(ClienteNascimento) > 
		(
		SELECT AVG(YEAR(GETDATE()) -YEAR(ClienteNascimento)) AS IDADE FROM dbo.Clientes
		)

2-Mostre qual agência tem quantidade de clientes acima da média.

.. code-block:: sql
	:linenos:
	
	SELECT AgenciaNome, COUNT(ClienteCodigo) AS QDTE
	FROM dbo.Contas INNER JOIN dbo.Agencias 
	ON Agencias.AgenciaCodigo = Contas.AgenciaCodigo
	GROUP BY AgenciaNome
	HAVING COUNT(ClienteCodigo) >
	(SELECT COUNT(DISTINCT ClienteCodigo)/
	COUNT(DISTINCT AgenciaCodigo) FROM dbo.Contas)

3-Mostre o nome da agência o saldo total, o mínimo, o máximo e a quantidade de clientes de cada agência.

4-Mostre o percentual que cada agencia representa no saldo total do banco.

5-Mostre as duas cidades que tem o maior saldo total
