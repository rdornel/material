EXERCÍCIOS Parte 1
==================

1. Crie uma tabela para armazenar o nome do feriado e data dele. Em seguida pesquise quais são os feriados nacionais (brasileiros) e insira nessa tabela. A tabela devera ter código do feriado (auto incremento), nome do feriado e a data em que ele é comemorado.

  .. code-block:: sql
    :linenos:

    CREATE TABLE FERIADOS 
     (
     CODFERIADO INT IDENTITY (1,1) CONSTRAINT PK_FERIADO PRIMARY KEY, 
     NOMEFERIADO VARCHAR(100),
     DATAFERIADO DATE
	 );

    INSERT FERIADOS (NOMEFERIADO, DATAFERIADO) 
	         VALUES ('INDEPENDENCIA','2018-09-07');

    SELECT * FROM FERIADOS;

2. Escolha 5 clientes e cadastre cartões de crédito para eles.

  .. code-block:: sql
    :linenos:

    INSERT CartaoCredito (AgenciaCodigo, ContaNumero, ClienteCodigo, CartaoCodigo, CartaoLimite, CartaoExpira, CartaoCodigoSeguranca) 
	                          VALUES (1,'562296-2',25,'1001-2002-3003-4004',3500.00,'2020-10-10',	123  );
    
  
	