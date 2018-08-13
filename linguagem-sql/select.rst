SELECT
======

- Comando utilizado para recuperar as informações armazenadas em um banco de dados.

O comando ``SELECT`` é composto dos atributos que desejamos, a ou as tabela(s) que possuem esses atributos e as condições que podem ajudar a filtrar os resultados desejados. Não é uma boa prática usar o ``*`` ou *star* para trazer os registros de uma tabela. Procure especificar somente os campos necessários. Isso ajuda o motor de execação de consultas a construir bons planos de execução. Se você conhecer a estrutura da tabela e seus índices, procure tirar proveito disso usando campos chaves, ou buscando e filtrando por atributos que fazem parte de chaves e índices no banco de dados.

.. code-block:: sql
  :linenos:

  SELECT * FROM Clientes;

- O Comando ``FROM`` indica a origem dos dados que queremos.

Na consulta acima indicamos que queremos todas as informações de clientes. É possível especificar mais de uma tabela no comando ``FROM``, porém, se você indicar mais de uma tabela no comando ``FROM``, lembre-se de indicar os campos que fazem o relacionamento entre as tabelas mencionadas na cláusula ``FROM``.

- O comando ``WHERE`` indica quais as consições necessárias e que devem ser obedecidadas para aquela consulta.

Procure usar campos restritivos ou indexados para otimizar sua consulta. Na tabela ``Clientes`` temos o código do cliente como chave, isso mostra que ele é um bom campo para ser usado como filto.

.. code-block:: sql
  :linenos:

  SELECT ClienteNome FROM Clientes WHERE ClienteCodigo=1;

- Um comando que pode auxiliar na obtenção de metadados da tabela que você deseja consultar é o comando ``sp_help``. Esse comando mostrar a estrutura da tabela, seus atributos, relacionamentos e o mais importante, se ela possui índice ou não.

.. code-block:: sql
  :linenos:

  sp_help clientes

- Repare que a tabela Clientes possui uma chave no ``ClienteCodigo``, portanto se você fizer alguma busca ou solicitar o campo ``ClienteCodigo`` a busca será muito mais rápida. Caso você faça alguma busca por algum campo que não seja chave ou não esteja "indexado" (Veremos índice mais pra frente) a busca vai resultar em uma varredura da tabela, o que não é um bom negócio para o banco de dados.

- Para escrever um comando ``SELECT`` procuramos mostrar ou buscar apenas os atributos que vamos trabalhar, evitando assim carregar dados desnecessários e que serão descartados na hora da montagem do formulário da aplicação. Também recomendamos o uso do nome da Tabela antes dos campos para evitar erros de ambíguidade que geralmente aparecem quando usamos mais de uma tabela.

.. code-block:: sql
  :linenos:

  SELECT Clientes.ClienteNome FROM Clientes;

- Você pode usar o comando ``AS`` para dar apelidos aos campos e tabelas para melhorar a visualiação e compreensão.

.. code-block:: sql
  :linenos:

  SELECT Clientes.ClienteNome AS Nome FROM Clientes;

  SELECT C.ClienteNome FROM Clientes AS C;

- Você pode usar o operador ``ORDER BY`` para ordenar os registros da tabela.

Procure identificar os campos da ordenação e verificar se eles possuem alguma ordenação na tabela através de algum índice. As operações de ordenação são muito custosas para o banco de dados. A primeira opção traz os campos ordenados em ordem ascendente ``ASC``, não precisando informar o operador. Caso você deseje uma ordenação descendente você deverá informar o ``DESC``.

.. code-block:: sql
  :linenos:

  SELECT Clientes.ClienteNome FROM Clientes
    ORDER BY Clientes.ClienteNome;

  SELECT Clientes.ClienteNome FROM Clientes
    ORDER BY Clientes.ClienteNome DESC;

- Outro operador que é muito utilizado em parceria com o ``ORDER BY`` é o ``TOP``, que permite limitar o conjunto de linhas retornado. Caso ele não esteja associado com o ``ORDER BY`` ele trará um determinado conjunto de dados baseado na ordem em que estão armazenados. Caso você use um operador ``ORDER BY`` ele mostrará os ``TOP`` maiores ou menores. O Primeiro exemplo mostra as duas maiores contas em relação ao seu saldo. A segunda, as duas menores.

.. code-block:: sql
  :linenos:

  SELECT TOP 2 ContaNumero, ContaSaldo FROM Contas
    ORDER BY ContaSaldo DESC;

  SELECT TOP 2 ContaNumero, ContaSaldo FROM Contas
    ORDER BY ContaSaldo;

- Podemos usar mais de uma tabela no comando ``FROM`` como falamos anteriormente, porém devemos respeitar seus relacionamentos para evitar situações como o exemplo abaixo. Execute o comando e veja o que acontece.

.. code-block:: sql
  :linenos:

  SELECT * FROM Clientes, Contas;

- A maneira correta deve levar em consideração que as tabelas que serão usadas tem relação entre si "chaves", caso não tenham, poderá ser necessário passar por um outra tabela antes. Lembre-se das tabelas associativas.

.. code-block:: sql
  :linenos:

  SELECT CLientes.ClienteNome, Contas.ContaSaldo
    FROM Clientes, Contas
    WHERE Clientes.ClienteCodigo=Contas.ClienteCodigo;

- O comando ``LIKE`` é usado para encontrar registros usando parte do que sabemos sobre ele. Por exemplo podemos buscar todas as pessoas que tenham nome começado com ``R``, usando um coringa ``%`` (Percentual). Podemos fazer diversas combinação com o ``%``.

`Documentação do comando LIKE <http://msdn.microsoft.com/en-us/library/ms179859.aspx/>`_

.. code-block:: sql
  :linenos:

  SELECT ClienteRua FROM dbo.Clientes WHERE ClienteRua LIKE 'a%' AND ClienteRua NOT LIKE 'E%';

  SELECT ClienteRua FROM dbo.Clientes WHERE ClienteRua LIKE '%a%';

  SELECT ClienteRua FROM dbo.Clientes WHERE ClienteRua LIKE '%a';

  SELECT ClienteRua FROM dbo.Clientes WHERE ClienteRua NOT LIKE 'a%';

- O Comando ``CASE`` é utilizado quando queremos fazer validações e até gerar novar colunas durante a execução da consulta. No exemplo abaixo fazemos uma classificação de um cliente com base no seu saldo, gerando assim uma nova coluna ``Curva Cliente``.

.. code-block:: sql
  :linenos:

  SELECT ContaNumero,
    CASE WHEN ContaSaldo < 200 THEN 'Cliente C' WHEN ContaSaldo < 500 THEN 'Cliente B'
    ELSE 'Cliente A' END AS 'Curva Cliente'
    FROM dbo.Contas;

- Podemos incluir em nossas consultas diversos operadores condicionais: ``=`` (igual), ``<>`` (diferente), ``>`` (maior), ``<`` (menor), ``<=`` (menor ou igual), ``>=`` (maior ou igual), ``OR`` (ou), ``AND`` (e) e ``BETWEEN`` (entre).

.. code-block:: sql
  :linenos:

  SELECT Nome_agencia, Numero_conta, saldo
    FROM Conta
    WHERE saldo > 500 AND Nome_agencia = 'Joinville';

  SELECT AgenciaCodigo FROM dbo.Agencias
    WHERE AgenciaCodigo BETWEEN 1 AND 3;


- O ``ALIAS`` ou apelido ajuda na exibição de consultas e tabelas. Dessa forma podemos dar nomes amigáveis para campos e tabelas durante a execução de consultas. Use sempre o ``AS`` antes de cada ``ALIAS``, mesmo sabendo que não é obrigatório.

.. code-block:: sql
  :linenos:

  SELECT Nome_agencia,C.Numero_conta,saldo AS [Total em Conta],
      Nome_cliente,D.Numero_conta AS 'Conta do Cliente'
    FROM Conta AS C, Depositante AS D
    WHERE C.Numero_conta=D.Numero_conta AND Nome_cliente IN ('Rodrigo','Laura')
    ORDER BY saldo DESC

- O comando ``DISTINCT`` serve para retirar do retorno da consulta registros repetidos.

.. code-block:: sql
  :linenos:

  SELECT DISTINCT Cidade_agencia FROM Agencia;


- A SUB CONSULTA, ``IN`` e ``NOT IN`` são poderosos recursos para auxiliar em buscas e filtragem de registros. Podemos criar subconjuntos de registros e usar operadores como ``IN`` para validar se os registros estão dentro daquele subconjunto.

.. code-block:: sql
  :linenos:

  SELECT AgenciaCodigo FROM dbo.Agencias
    WHERE AgenciaCodigo NOT IN ('1','4');

  SELECT Contas.ContaNumero, Contas.ContaSaldo, Contas.AgenciaCodigo
    FROM Contas INNER JOIN
      (
      SELECT AgenciaCodigo, MAX(ContaSaldo) AS VALOR
      FROM Contas
      GROUP BY AgenciaCodigo
      ) AS TB2
    ON
    TB2.AgenciaCodigo=Contas.AgenciaCodigo AND TB2.VALOR=Contas.ContaSaldo;

- Os operadores ``UNION`` e ``UNION ALL`` ajudam a consolidar conjuntos de registros que são retornados por consultas distintas. O operador ``ALL`` faz a junção das consultas sem eliminar itens duplicados. Precisamos obedecer o mesmo número de colunas e tipos de dados entre as consultas.

.. code-block:: sql
  :linenos:

  SELECT ClienteNome FROM dbo.Clientes WHERE ClienteCodigo = 1
  UNION
  SELECT ClienteNome FROM dbo.Clientes WHERE ClienteCodigo = 2;

  SELECT ClienteNome FROM dbo.Clientes WHERE ClienteCodigo = 1
  UNION ALL
  SELECT ClienteNome FROM dbo.Clientes WHERE ClienteCodigo = 1;

- Existem diversos tipos de ``JOINS``. O mais tradicional e restritivo é o ``JOIN`` ou ``INNER JOIN`` que requer que o registros usado na comparação exista em ambas as tabelas.

No exemplo abaixo, o ``ClienteCodigo`` não poderá ser vazio em nenhuma das tabelas envolvidas, caso isso ocorra, aquela linha não será retornada no resultado.

.. figure:: joins.jpg
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

O comando FULL retorna todos os registros das tabelas relacionadas, mesmo que não exista um correspondente entre elas.

  .. code-block:: sql
    :linenos:

    SELECT * FROM CartaoCredito FULL OUTER JOIN Clientes ON CartaoCredito.ClienteCodigo=Clientes.ClienteCodigo;

- CROSS

Efetua um operação de produto cartesiano, para cada registro de uma tabela ele efetua um relacionamento com os registros das outras tabelas.

  .. code-block:: sql
    :linenos:

    SELECT * FROM CLIENTES CROSS JOIN Contas;


- As FUNÇÕES DE AGREGAÇÃO, ``SUM`` (soma), ``MIN`` (mínimo), ``MAX`` (máximo), ``COUNT`` (contagem), ``AVG`` (média), permitem um nível mais robusto de informação, criando conjuntos de dados agrupados, médias entre outros, permitindo o resumo e a totalização de conjuntos de resultados. Sempre que usarmos a função de agregação em conjunto com um campo agregador, devemos usar a função ``GROUP BY`` para indicar qual o campo será o responsável pelo agrupamento das informações.

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

O comando EXISTS é parecido com o comando ``IN``, quando queremos comparar mais de um campo contra uma subconsulta.

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

Muitas vezes necessitamos armazenar determinados valores para uso posterior. Um exemplo é guardar um valor total em uma variável para que ele seja usado em cálculo de percentual por exemplo

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
