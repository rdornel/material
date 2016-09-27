SELECT - Nível 2
================

- Existem diversos tipos de ``JOINS``. O mais tradicional e restritivo é o ``JOIN`` ou ``INNER JOIN`` que requer que o registros usado na comparação exista em ambas as tabelas.

No exemplo abaixo, o ``ClienteCodigo`` não poderá ser vazio em nenhuma das tabelas envolvidas, caso isso ocorra, aquela linha não será retornada no resultado.

.. figure:: linguagem-sql\joins.jpg
   :scale: 30%
   :alt: map to buried treasure

Fonte da imagem: `Representação Visual das Joins <http://www.codeproject.com/Articles/33052/Visual-Representation-of-SQL-Joins/>`_

.. code-block:: sql
  :linenos:

  SELECT * FROM Clientes
    JOIN Contas
      ON Clientes.ClienteCodigo=Contas.ClienteCodigo;

  SELECT * FROM CLIENTES
    INNER JOIN Contas
      ON Clientes.ClienteCodigo=Contas.ClienteCodigo;

- LEFT JOIN

O comando LEFT indica que todos os registros existentes na tabela da sua esquerda serão retornados e os registros da outra tabela da direita irão ser retornados ou então virão em branco.

  .. code-block:: sql
    :linenos:

    SELECT ClienteNome, ContaSaldo,
      CASE WHEN CartaoCodigo IS NULL THEN 'LIGAR' ELSE 'NÃO INCOMODAR' END AS 'NN'
      FROM Clientes
      INNER JOIN Contas
      ON (Contas.ClienteCodigo = Clientes.ClienteCodigo)
      LEFT JOIN CartaoCredito
      ON (CartaoCredito.ClienteCodigo = Clientes.ClienteCodigo);

- RIGHT

Já o comando RIGHT traz todos os registros da tabela da direita e os registos da tabela da esquerda, mostrando em branco aqueles que não tem relação.
  .. code-block:: sql
    :linenos:

    SELECT * FROM CartaoCredito RIGHT JOIN Clientes ON CartaoCredito.ClienteCodigo=Clientes.ClienteCodigo;

- FULL

O comando full retorna todos os registros das tabelas relacionadas, mesmo que não existe um correspondente entre elas.

  .. code-block:: sql
    :linenos:

    SELECT * FROM CartaoCredito FULL OUTER JOIN Clientes ON CartaoCredito.ClienteCodigo=Clientes.ClienteCodigo;

- CROSS

Efetua um operação de produto cartesiano, para cada registro de uma tabela ele efetua um relacionamento com os registros das outras tabelas.

  .. code-block:: sql
    :linenos:

    SELECT * FROM CLIENTES CROSS JOIN Contas;


- As FUNÇÕES DE AGREGAÇÃO, ``SUM``, ``MIN``, ``MAX``, ``COUNT``, ``AVG`` permitem um nível mais robusto de informação, criando cojuntos de dados agrupados, médias entre outros, permitindo que possamos resumir e totatlizar comjuntos de resultados. Sempre que usarmos a função de agregação em conjunto com um campo agregador devemos usar a função ``GROUP BY`` para indicar qual o compo será o responsável pelo agrupamento das informações.

Caso você deseje comparar conjuntos de informações contidos na função de agragação você deve compará-los usando o ``HAVING``.

.. code-block:: sql
  :linenos:

  SELECT TOP 2 AgenciaNome, SUM(ContaSaldo) AS TOTAL
    FROM Contas,  Agencias
    WHERE Agencias.AgenciaCodigo=Contas.AgenciaCodigo
    GROUP BY AgenciaNome
    HAVING SUM(ContaSaldo) > (SELECT MAX(ContaSaldo) AS VALORMETA FROM Contas AS META)
    ORDER BY 2 DESC;

  SELECT SUM( Contas.ContaSaldo),
    AgenciaCodigo, ContaNumero
    FROM Contas
    GROUP BY AgenciaCodigo,ContaNumero
    --WHERE COM AVG ???
    --WHERE COM SUBCONSULTA ???
    HAVING SUM( Contas.ContaSaldo) > (SELECT AVG( Contas.ContaSaldo) FROM  Contas); --667,0833

  SELECT MAX(ContaSaldo) FROM  Contas;
  SELECT MIN(ContaSaldo) FROM  Contas;
  SELECT AVG(ContaSaldo) FROM  Contas;
  SELECT COUNT(*), COUNT(CONTAS.ClienteCodigo), COUNT(DISTINCT CONTAS.ClienteCodigo) FROM  Contas;

- EXISTS

O comando EXISTS é pareceido com o comando IN, quando queremos comparar mais de um campo contra uma subconsulta.

  .. code-block:: sql
    :linenos:

    SELECT * FROM  Contas C
	WHERE EXISTS
			(SELECT * FROM  CartaoCredito CC
				WHERE C.ClienteCodigo=CC.ClienteCodigo
				AND C.AgenciaCodigo=CC.AgenciaCodigo
			)

- FUNÇÕES DE Data e Hora

  .. code-block:: sql
    :linenos:

    SET DATEFORMAT YDM

    SET LANGUAGE PORTUGUESE

    SELECT YEAR(getdate()) -YEAR( Clientes.ClienteNascimento),
      DATEDIFF(YEAR,ClienteNascimento,GETDATE()),
      DATEPART(yy,ClienteNascimento),
      dateadd(yy,1,ClienteNascimento),
      EOMONTH(GETDATE()),
      DATENAME(MONTH,(GETDATE()))
    FROM  Clientes;

  .. code-block:: sql
    :linenos:

    SELECT * FROM  Contas
      WHERE YEAR(ContaAbertura) = '2011'
      ORDER BY ContaAbertura;
  
- Variáveis

Muitas vezes necessitamos armazenar determinados valores para uso posterior. Um exemplo é gardar um valor total em uma variável para que ele seja usado em cálculo de percentual por exemplo

.. code-block:: sql
  :linenos:

  declare @numero int
  set @numero = 1

  declare @dia int
  set @dia = (select day(getdate()))

- SELECT INTO

.. code-block:: sql
  :linenos:

	SELECT Clientes.ClienteNome, 
	DATEDIFF(YEAR,Clientes.ClienteNascimento,GETDATE()) AS IDADE
	INTO ClientesIdade -- O comando INTO vem depois do campos listados no SELECT e antes do FROM.
	FROM Clientes

	SELECT * FROM ClientesIdade
	
- CAST, CONVERT e concatenação

Comandos utilizados para converter tipos de dados e concatenar Strings.

  .. code-block:: sql
    :linenos:
	
    SELECT Clientes.ClienteNome + Clientes.ClienteCidade FROM Clientes;

    SELECT Clientes.ClienteNome + ' ' + Clientes.ClienteCidade FROM Clientes;

    SELECT Clientes.ClienteNome + ' de ' + Clientes.ClienteCidade FROM Clientes;

    SELECT Clientes.ClienteNome + ' - R$ ' + CAST (Contas.ContaSaldo AS VARCHAR(10) )FROM Clientes INNER JOIN Contas ON Contas.ClienteCodigo = Clientes.ClienteCodigo;
    
    SELECT Clientes.ClienteNome + ' - R$ ' + CONVERT  (VARCHAR(10), Contas.ContaSaldo )FROM Clientes INNER JOIN Contas ON Contas.ClienteCodigo = Clientes.ClienteCodigo;
