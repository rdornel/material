EXERCÍCIOS Procedure
====================

1. Neste exercício vamos criar um banco de dados para armazenar os dados dos alunos de uma universidade. Além de desenhar o diagrama, criar o banco de dados e seus objetos, você deverá criar os scripts de população básica. Em seguida deverá criar as procedures que irão executar as operações de manipulação das notas e faltas. Abaixo uma sugestão de parte da solução:


  .. code-block:: sql
    :linenos:

   
	USE MASTER 
	ALTER DATABASE Universidade SET SINGLE_USER WITH ROLLBACK IMMEDIATE
	GO
	DROP DATABASE Universidade;
	GO
	USE master;
	CREATE DATABASE Universidade;
	GO
	USE Universidade;
	GO
	CREATE TABLE ALUNOS (MATRICULA INT NOT NULL IDENTITY CONSTRAINT PK_ALUNO PRIMARY KEY, NOME VARCHAR(50) NOT NULL);
	GO
	CREATE TABLE CURSOS (CURSO CHAR(3) NOT NULL CONSTRAINT PK_CURSO PRIMARY KEY, NOME VARCHAR(50) NOT NULL);
	GO
	CREATE TABLE PROFESSOR (PROFESSOR INT IDENTITY NOT NULL CONSTRAINT PK_PROFESSOR PRIMARY KEY, NOME VARCHAR(50) NOT NULL);
	GO
	CREATE TABLE MATERIAS (SIGLA CHAR(3) NOT NULL, NOME VARCHAR(50) NOT NULL, CARGAHORARIA INT NOT NULL,
	CURSO CHAR(3) NOT NULL, PROFESSOR INT
	CONSTRAINT PK_MATERIA PRIMARY KEY (SIGLA,CURSO,PROFESSOR)
	CONSTRAINT FK_CURSO FOREIGN KEY (CURSO) REFERENCES CURSOS(CURSO),
	CONSTRAINT FK_PROFESSOR FOREIGN KEY (PROFESSOR) REFERENCES PROFESSOR (PROFESSOR)
	);
	GO
	INSERT ALUNOS (NOME) VALUES ('Pedro')
	GO
	INSERT CURSOS (CURSO, NOME) VALUES ('SIS','SISTEMAS'),('ENG','ENGENHARIA')
	GO
	INSERT PROFESSOR (NOME ) VALUES ('DORNEL'),('WALTER')
	GO
	INSERT MATERIAS (SIGLA, NOME, CARGAHORARIA, CURSO,PROFESSOR) 
	VALUES ('BDA','BANCO DE DADOS',144,'SIS',1), ('PRG','PROGRAMAÇÃO',144,'SIS',2)
	GO
	INSERT MATERIAS (SIGLA, NOME, CARGAHORARIA, CURSO,PROFESSOR) 
	VALUES ('BDA','BANCO DE DADOS',144,'ENG',1), ('PRG','PROGRAMAÇÃO',144,'ENG',2)
	GO
	CREATE TABLE MATRICULA (MATRICULA INT, CURSO CHAR(3), MATERIA CHAR(3), PROFESSOR INT, PERLETIVO INT,
	N1 FLOAT, N2 FLOAT, N3 FLOAT, N4 FLOAT, TOTALPONTOS FLOAT, MEDIA FLOAT, 
	F1 INT, F2 INT, F3 INT, F4 INT, TOTALFALTAS INT, PERCFREQ FLOAT, RESULTADO VARCHAR(20)  

	CONSTRAINT PK_MATRICULA PRIMARY KEY (MATRICULA,CURSO,MATERIA,PROFESSOR,PERLETIVO),
	CONSTRAINT FK_ALUNOS_MATRICULA FOREIGN KEY (MATRICULA) REFERENCES ALUNOS (MATRICULA),
	CONSTRAINT FK_CURSOS_MATRICULA FOREIGN KEY (CURSO) REFERENCES CURSOS (CURSO),
	--CONSTRAINT FK_MATERIAS FOREIGN KEY (MATERIA) REFERENCES MATERIAS (SIGLA),
	CONSTRAINT FK_PROFESSOR_MATRICULA FOREIGN KEY (PROFESSOR) REFERENCES PROFESSOR(PROFESSOR)
	)
	
	
	CREATE PROCEDURE sp_MatriculaAluno
	(
	@NOMEALUNO VARCHAR(50),
	@CURSOALUNO VARCHAR(50)
	)
	AS
	BEGIN

	DECLARE @MATRICULAALUNO INT, @CODIGOCURSO VARCHAR(3) 

	SET @MATRICULAALUNO = (SELECT MATRICULA FROM ALUNOS WHERE NOME = @NOMEALUNO)

	SET @CODIGOCURSO = (SELECT CURSO FROM CURSOS WHERE NOME = @CURSOALUNO)

	INSERT MATRICULA
		(
				MATRICULA,
				CURSO,
				MATERIA,
				PROFESSOR,
				PERLETIVO

		)
		SELECT @MATRICULAALUNO AS MATRICULA, CURSO, SIGLA,PROFESSOR, YEAR(GETDATE()) AS PERLETIVO FROM MATERIAS WHERE CURSO ='ENG'

	END
	
	--Calculo do percentual de Frequencia (144-NrFaltas*100)/144
	
	
	EXEC sp_MatriculaAluno @NOMEALUNO = 'Guilherme', -- varchar(50)
						   @CURSOALUNO = 'Sistemas' -- varchar(50)

	
Exemplo de INSERT com SELECT


  .. code-block:: sql
    :linenos:
	
	INSERT MATRICULA
	(
		MATRICULA,
		CURSO,
		MATERIA,
		PROFESSOR,
		PERLETIVO
		
	)
	SELECT 1 AS MATRICULA, CURSO, SIGLA,PROFESSOR, YEAR(GETDATE()) FROM MATERIAS WHERE CURSO ='ENG'
	
Exemplo de PROCEDURE para inserir (atualizar) as notas


  .. code-block:: sql
    :linenos:
	
	CREATE PROCEDURE [dbo].[sp_CadastraNotas]
	(
		@MATRICULA INT,
		@CURSO CHAR(3),
		@MATERIA CHAR(3),
		@PERLETIVO CHAR(4),
		@NOTA FLOAT,
		@FALTA INT,
		@PARAMETRO INT
	)
	AS
	BEGIN

		IF @PARAMETRO = 1
		BEGIN

			UPDATE MATRICULA
			SET N1 = @NOTA,
				F1 = @FALTA,
				TOTALPONTOS = @NOTA,
				TOTALFALTAS = @FALTA,
				MEDIA = @NOTA
			WHERE MATRICULA = @MATRICULA
				  AND CURSO = @CURSO
				  AND MATERIA = @MATERIA
				  AND PERLETIVO = @PERLETIVO;
		END;

		ELSE IF @PARAMETRO = 2
		BEGIN

			UPDATE MATRICULA
			SET N2 = @NOTA,
				F2 = @FALTA,
				TOTALPONTOS = @NOTA + N1,
				TOTALFALTAS = @FALTA + F1,
				MEDIA = (@NOTA + N1) / 2
			WHERE MATRICULA = @MATRICULA
				  AND CURSO = @CURSO
				  AND MATERIA = @MATERIA
				  AND PERLETIVO = @PERLETIVO;
		END;

		ELSE IF @PARAMETRO = 3
		BEGIN

			UPDATE MATRICULA
			SET N3 = @NOTA,
				F3 = @FALTA,
				TOTALPONTOS = @NOTA + N1 + N2,
				TOTALFALTAS = @FALTA + F1 + F2,
				MEDIA = (@NOTA + N1 + N2) / 3
			WHERE MATRICULA = @MATRICULA
				  AND CURSO = @CURSO
				  AND MATERIA = @MATERIA
				  AND PERLETIVO = @PERLETIVO;
		END;

		ELSE IF @PARAMETRO = 4
		BEGIN

			DECLARE @RESULTADO VARCHAR(50),
					@FREQUENCIA FLOAT,
					@MEDIAFINAL FLOAT;



			UPDATE MATRICULA
			SET N4 = @NOTA,
				F4 = @FALTA,
				TOTALPONTOS = @NOTA + N1 + N2 + N3,
				TOTALFALTAS = @FALTA + F1 + F2 + F3,
				MEDIA = (@NOTA + N1 + N2 + N3) / 4,
				@MEDIAFINAL = (@NOTA + N1 + N2 + N3) / 4,
				MEDIAFINAL = @MEDIAFINAL,
				@FREQUENCIA = 100 - (((@FALTA + F1 + F2 + F3) * 144) / 100),
				PERCFREQ = @FREQUENCIA,
				RESULTADO = CASE
								WHEN @FREQUENCIA >= 75
									 AND @MEDIAFINAL >= 7 THEN
									'APROVADO'
								WHEN @FREQUENCIA >= 75
									 AND @MEDIAFINAL >= 3 THEN
									'EXAME'
								ELSE
									'REPROVADO'
							END
			WHERE MATRICULA = @MATRICULA
				  AND CURSO = @CURSO
				  AND MATERIA = @MATERIA
				  AND PERLETIVO = @PERLETIVO;



		END;

		ELSE IF @PARAMETRO = 5
		BEGIN

			DECLARE @MEDIA FLOAT =
					(
						SELECT MEDIA
						FROM MATRICULA
						WHERE MATRICULA = @MATRICULA
							  AND CURSO = @CURSO
							  AND MATERIA = @MATERIA
							  AND PERLETIVO = @PERLETIVO
							  AND RESULTADO = 'EXAME'
					);

			UPDATE MATRICULA
			SET NOTAEXAME = @NOTA,
				RESULTADO = CASE
								WHEN (@NOTA + @MEDIA) >= 10 THEN
									'APROVADO'
								ELSE
									'REPROVADO'
							END
			WHERE MATRICULA = @MATRICULA
				  AND CURSO = @CURSO
				  AND MATERIA = @MATERIA
				  AND PERLETIVO = @PERLETIVO
				  AND RESULTADO = 'EXAME';


		END;

	END;
	GO






Exemplo de execução da PROCEDURE para inserir (atualizar) as notas


  .. code-block:: sql
    :linenos:
	
	--ALTER TABLE MATRICULA ADD MEDIAFINAL FLOAT
	
	--ALTER TABLE MATRICULA ADD NOTAEXAME FLOAT


	EXEC sp_CadastraNotas @MATRICULA = 4,      -- int
						  @CURSO = 'ENG',      -- char(3)
						  @MATERIA = 'BDA',    -- char(3)
						  @PERLETIVO = '2018', -- char(4)
						  @NOTA = 7.0,         -- float
						  @FALTA = 2,
						  @PARAMETRO = 4;      -- int
						  
						  
Exemplo de INSERT - SELECT

  .. code-block:: sql
    :linenos:
  
	
		CREATE TABLE pedidos
	(
	idpedido INT,
	idproduto INT,
	valorpedido float
	)

	CREATE TABLE itenspedido
	(
	idpedido INT,
	iditem int,
	idproduto int
	)

	CREATE TABLE itens
	(
	iditem INT,
	nome varchar(50)
	)
	INSERT itens
	(
		iditem,
		nome
	)
	VALUES
	(   1, -- iditem - int
		'AR CONDICIONADO' -- nome - varchar(50)
		)
		

	CREATE TABLE subitens
	(
	idsubitem INT,
	iditem INT,
	nomesubitem VARCHAR(50)
	)
	INSERT subitens
	(
		idsubitem,
		iditem,
		nomesubitem
	)
	VALUES
	(   2, -- idsubitem - int
		1, -- iditem - int
		'MOTOR' -- nomesubitem - varchar(50)
		)



		SELECT * FROM itens
		SELECT * FROM subitens

		SELECT * FROM PEDIDOS
		
		
		INSERT pedidos
		(
			idpedido,
			idproduto,
			valorpedido
		)
		VALUES
		(   1,  -- idpedido - int
			1,  -- idproduto - int
			1000.00 -- valorpedido - float
			)

			DECLARE @produto INT
			SET @produto = (SELECT idproduto FROM pedidos WHERE idpedido =1)

			SELECT @produto AS 'AR COND'

			INSERT itenspedido
			(
				idpedido,
				iditem,
				idproduto
			)
			SELECT IDPEDIDO=1, idsubitem, iditem 
			FROM subitens WHERE iditem = 1--@CURSO

			--VALUES
			--(   0, -- idpedido - int
			--    0, -- iditem - int
			--    0  -- idproduto - int
			--    )
		