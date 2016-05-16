SELECT
======

- Comando utilizado para recuperar as informações armazenadas em um banco de dados. O comando SELECT é composto dos atributos que desejamos, a ou as tabela(s) que possuem esses atributos e as consdições que podem ajudar a filtrar os resultados desejados. Não é uma boa prática usar o * ou (star) para trazer os registros de uma tabela. Procure especificar somente os campos necessários. Isso ajudo o motor de execação de consultas a construir bons planos de execução. Se você conhecer a estrutura da tabela e seus índices, procure tirar proveito disso usando campos chaves, ou buscando e filtrando por atributos que fazem parte de chaves e índices no banco de dados.

  .. code-block:: sql
    :linenos:

    SELECT * FROM Clientes;
       
       
- Um comando que pode auxiliar na obtenção de metadados da tabela que você deseja consultar é o comando sp_help. Esse comando mostrar a estrutura da tabela, seus atributos, relacionamentos e o mais importante, se ela possui índice ou não.

  .. code-block:: sql
    :linenos:

    sp_help clientes