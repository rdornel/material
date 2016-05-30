SELECT
======

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

- Para escrever um comando SELECT procuramos mostrar ou buscar apenas os atributos que vamos trabalhar, evitando assim carregadar dados desecessários e que serão descartados na hora da montagem do formulário da aplicação.

  .. code-block:: sql
    :linenos:
    
    SELECT Clientes.ClienteNome FROM Clientes
    
