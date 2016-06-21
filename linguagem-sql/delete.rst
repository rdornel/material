DELETE
======

- Comando utilizado para deletes registros em um banco de dados.
- Sempre que for trabalhar com o comando UPDATE ou DELETE, procure executar um SELECT antes para validar se os registros que serão afetados, são exatamente aqueles que você deseja.

 .. code-block:: sql
    :linenos:
    
    DELETE FROM CartaoCredito WHERE ClienteCodigo = 1;
