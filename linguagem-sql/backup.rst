BACK UP
=======

- Comando vinculado a uma tabela que executa um ação assim que algum comando de UPDATE, INSERT ou DELETE é executado na tabela onde a trigger está vinculada.

Comano para BACK UP
-------------------

.. code-block:: sql
  :linenos:

  BACKUP DATABASE [MinhaCaixa] 
  TO  DISK = 'C:\bkp\MinhaCaixa2018.bak';
  

  BACKUP DATABASE [MinhaCaixa] 
  TO  DISK = N'C:\bkp\MinhaCaixa2018_diff.bak' 
  WITH  DIFFERENTIAL , STATS = 10;

  BACKUP LOG [MinhaCaixa] TO  
  DISK = N'C:\bkp\MinhaCaixa2018_log.trn' WITH NOFORMAT,  STATS = 10;


  USE [master]
  RESTORE DATABASE [MinhaCaixa] 
  FROM  DISK = N'C:\bkp\MinhaCaixa2018.bak' 
  WITH REPLACE,  STATS = 10;

  USE [master]

  RESTORE DATABASE [MinhaCaixa] FROM  DISK = N'C:\bkp\MinhaCaixa2018.bak' 
  WITH  FILE = 1,  NORECOVERY,  NOUNLOAD,  STATS = 5
  RESTORE DATABASE [MinhaCaixa] FROM  DISK = N'C:\bkp\MinhaCaixa2018_diff.bak' 
  WITH  FILE = 1,  NORECOVERY,  NOUNLOAD,  STATS = 5
  RESTORE LOG [MinhaCaixa] FROM  DISK = N'C:\bkp\MinhaCaixa2018_log.trn' 
  WITH  FILE = 1,  NOUNLOAD,  STATS = 5;
  
