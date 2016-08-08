SELECT - Nível 1
================

- Comando utilizado para recuperar as informações armazenadas em um banco de dados. O comando SELECT é composto dos atributos que desejamos, a ou as tabela(s) que possuem esses atributos e as consdições que podem ajudar a filtrar os resultados desejados. Não é uma boa prática usar o * ou (star) para trazer os registros de uma tabela. Procure especificar somente os campos necessários. Isso ajuda o motor de execação de consultas a construir bons planos de execução. Se você conhecer a estrutura da tabela e seus índices, procure tirar proveito disso usando campos chaves, ou buscando e filtrando por atributos que fazem parte de chaves e índices no banco de dados.

  .. code-block:: sql
    :linenos:

    SELECT * FROM Clientes;

- O Comando FROM indica a origem dos dados que queremos. Na consulta acima indicamos que queremos todas as informações de clientes. É possível especificar mais de uma tabela no comando FROM, porém, se você indicar mais de uma tabela no comando FROM, lembre-se de indicar os campos que fazem o relacionamento entre as tabelas mencionadas na cláusula FROM.

- O comando WHERE indica quais as consições necessárias e que devem ser obedecidadas para aquela consulta. Procure usar campos restritivos ou indexados para otimizar sua consulta. Na tabela Clientes temos o código do cliente como chave, isso mostra que ele é um bom campo para ser usado como filto.

  .. code-block:: sql
    :linenos:

    SELECT ClienteNome FROM Clientes WHERE ClienteCodigo=1;

- Um comando que pode auxiliar na obtenção de metadados da tabela que você deseja consultar é o comando sp_help. Esse comando mostrar a estrutura da tabela, seus atributos, relacionamentos e o mais importante, se ela possui índice ou não.

  .. code-block:: sql
    :linenos:

    sp_help clientes

- Repare que a tabela Clientes possui uma chave no ClienteCodigo, portanto se você fizer alguma busca ou solicitar o campo ClienteCodigo a busca será muito mais rápida. Caso você faça alguma busca por algum campo que não seja chave ou não esteja "indexado" (Veremos índice mais pra frente) a busca vai resultar em uma varredura da tabela, o que não é um bom negócio para o banco de dados.

- Para escrever um comando SELECT procuramos mostrar ou buscar apenas os atributos que vamos trabalhar, evitando assim carregadar dados desecessários e que serão descartados na hora da montagem do formulário da aplicação. Também recomendamos o uso do nome da Tabela antes dos campos para evitar erros de ambíguidade que geralmente aparecem quando usamos mais de uma tabela.

  .. code-block:: sql
    :linenos:

    SELECT Clientes.ClienteNome FROM Clientes;

- Você pode usar o comando AS para dar apelidos aos campos e tabelas para melhorar a visualiação e compreensão.

  .. code-block:: sql
    :linenos:

    SELECT Clientes.ClienteNome  as Nome FROM Clientes;

    SELECT C.ClienteNome FROM Clientes as C;

- Você pode usar o operador ORDER BY para ordenar os registros da tabela. Procure identificar os campos da ordenação e verificar se eles possuem alguma ordenação na tabela através de algum índice. As opererações de ordenação são muito custozas para o banco de dados. A primeira opção traz os campos ordenados em ordem ascendente ASC, não precisando informar o operador. Caso você deseje uma ordenação descendente DESC você deverá informar o DESC.

  .. code-block:: sql
    :linenos:

    SELECT Clientes.ClienteNome FROM Clientes
    ORDER BY Clientes.ClienteNome;

    SELECT Clientes.ClienteNome FROM Clientes
    ORDER BY Clientes.ClienteNome DESC;

- Outro operador que é muito utilizado em parceria com o ORDER BY é o TOP, que permite limitar o conjunto de linhas retornado. Caso ele não esteja associado com o ORDER BY ele trará um determinado conjunto de dados baseado na ordem em que estão armazenados. Caso você use um operdaor ORDER BY ele mostrar os TOP maiores ou menores. O Primeiro exemplo mostra as duas maiores contas em relação ao seu saldo. A segunda, as duas menores.

  .. code-block:: sql
    :linenos:

    SELECT TOP 2 ContaNumero, ContaSaldo FROM Contas
    ORDER BY ContaSaldo DESC;

    SELECT TOP 2 ContaNumero, ContaSaldo FROM Contas
    ORDER BY ContaSaldo;

- Podemos usar mais de uma tabela no comando FROM como falamos anteriormente, porém devemos respeitar seus relacionamentos para evitar situações como o exemplo abaixo. Execute o comando e veja o que acontece.

  .. code-block:: sql
    :linenos:

    SELECT * FROM Clientes, Contas;

- A maneira correta deve levar em consideração que as tabelas que serão usadas tem relação entre si "chaves", caso não tenham, poderá ser necessário passar por um outra tabela antes. Lembre-se das tabelas associativas.

  .. code-block:: sql
    :linenos:

    SELECT CLientes.ClienteNome, Contas.ContaSaldo
    FROM Clientes, Contas
    where Clientes.ClienteCodigo=Contas.ClienteCodigo

- O comando LIKE é usado para encontrar registros usando parte do que sabemos sobre ele. Por exemplo podemos buscar todas as pessoas que tenham nome começado com R, usando um coringa % (Percentual). Podemos fazer diversas combinação com o %.

`Documentação do comando LIKE <http://msdn.microsoft.com/en-us/library/ms179859.aspx/>`_

  .. code-block:: sql
    :linenos:

    SELECT ClienteRua FROM dbo.Clientes WHERE ClienteRua  LIKE 'a%' AND ClienteRua  NOT LIKE 'E%'

    SELECT ClienteRua FROM dbo.Clientes WHERE ClienteRua  LIKE '%a%'

    SELECT ClienteRua FROM dbo.Clientes WHERE ClienteRua  LIKE '%a'

    SELECT ClienteRua FROM dbo.Clientes WHERE ClienteRua  NOT LIKE 'a%'
     

- O Comando CASE é utilizado quando queremos fazer validações e até gerar novar colunas durante a execução da consulta. No exemplo abaixo fazemos uma classificação de um cliente com base no seu saldo, gerando assim uma nova coluna Curva Cliente.

  .. code-block:: sql
    :linenos:
 
    SELECT ContaNumero, 
    CASE WHEN ContaSaldo < 200 THEN 'Cliente C' WHEN ContaSaldo < 500 THEN 'Cliente B'
    ELSE 'Cliente A' END AS 'Curva Cliente'
    FROM dbo.Contas;

- Podemos incluir em nossas consultas diversos operadores condicionais: = (igual), <> (diferente), >, <, <=, >=, OR (ou), AND (e) e BETWEEN

  .. code-block:: sql
    :linenos:

    SELECT  Nome_agencia , Numero_conta , saldo
    FROM    Conta
    WHERE   saldo > 500 AND Nome_agencia = 'Joinville';

	SELECT AgenciaCodigo FROM dbo.Agencias 
	WHERE AgenciaCodigo BETWEEN 1 AND 3;


- O ALIAS ou apleido ajuda na exibição de consultas e tabelas. Dessa forma podemos dar nomes amigáveis para campos e tabelas durante a execução de consultas. Use sempre o AS antes de cada ALIAS, mesmo sabendo que não é obrigatório.

  .. code-block:: sql
    :linenos:
	
    SELECT Nome_agencia,C.Numero_conta,saldo AS [Total em Conta],
    Nome_cliente,D.Numero_conta AS 'Conta do Cliente'
      FROM Conta AS C, Depositante AS D
            WHERE C.Numero_conta=D.Numero_conta AND Nome_cliente IN ('Rodrigo','Laura')
      ORDER BY saldo DESC


- O comando DISTINCT serve para retirar do retorno da consulta registros repitidos.

  .. code-block:: sql
    :linenos:
	
	SELECT DISTINCT Cidade_agencia FROM Agencia


- A SUB CONSULTA, IN e NOT IN são poderosos recursos para autilizar em buscas e filtragem de registros. Podemos criar subconjuntos de registros e usar operadores como IN para validar se os registros estão dentro daquele subconjunto.

  .. code-block:: sql
    :linenos:
	
	SELECT AgenciaCodigo FROM dbo.Agencias 
	WHERE AgenciaCodigo NOT IN ('1','4')
	
	SELECT Contas.ContaNumero, Contas.ContaSaldo, Contas.AgenciaCodigo
	FROM Contas INNER JOIN
		(
		SELECT AgenciaCodigo, MAX(ContaSaldo) AS VALOR
		FROM Contas 
		GROUP BY   AgenciaCodigo
		) AS TB2 
	ON 
	TB2.AgenciaCodigo=Contas.AgenciaCodigo AND TB2.VALOR=Contas.ContaSaldo;

- Os operadores UNION e UNION ALL ajudam a consolidar conjuntos de registros que são retornados por consultas distintas. O operador ALL faz a junção dos consultos sem eliminar duplicados. Precisamos obedecer o mesmo número de colunas e tipos de dados entre as consultas.

  .. code-block:: sql
    :linenos:
	
	SELECT ClienteNome FROM dbo.Clientes WHERE ClienteCodigo = 1
	UNION 
	SELECT ClienteNome FROM dbo.Clientes WHERE ClienteCodigo = 2
	
	SELECT ClienteNome FROM dbo.Clientes WHERE ClienteCodigo = 1
	UNION ALL
	SELECT ClienteNome FROM dbo.Clientes WHERE ClienteCodigo = 1

