CREATE
======

- Comando utilizado para criar os principais objetos em um banco de dados.

Neste tópico vamos trabalhar com as diversas variações do comando CREATE relacionados ao início dos trabalhos com 
criação das entidades no banco de dados.

O Primeiro comando é o CREATE DATABASE, que cria o Banco de dados e suas dependências, como arquivos e metadados dentro do sistema. 
Vale lembrar que alguns sistemas gerenciadores de bancos de dados podem implementar maneiras diferentes de tratar os bancos de dados ou espaços de trabalho de cada usuário ou sistema.
Sugiro a leitura do link abaixo, que explica como o Oracle trabalha, ao contrário do SQL Server que vemos em sala de aula.

http://www.oracle.com/technetwork/pt/articles/database-performance/introducao-conceito-de-tablespaces-495850-ptb.html

No nosso banco de dados de Exemplo temos a criação básica de um banco de dados e a criação de uma tabela chamada Clientes.

  .. code-block:: sql
    :linenos:

    CREATE DATABASE MinhaCaixa;

    CREATE TABLE Clientes (
      ClienteCodigo int,
      ClienteNome varchar(20)
    );
       
Podemos ter variações do comando CREATE TABLE de acordo com a necessidade.
    
    CONSTRAINT PRIMARY KEY
    CONSTRAINT FOREIGN KEY
    CONSTRAINT´s de domínio
    
    ALTER TABLE ADD CONSTRAINT

