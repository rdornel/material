EXERCÍCIOS
==========

1. Mostre quais os clientes tem idade superior a média.

  .. code-block:: sql
    :linenos:

    SELECT ClienteNome, YEAR(GETDATE()) - YEAR(ClienteNascimento) AS idade
      FROM Clientes
      WHERE YEAR(GETDATE()) - YEAR(ClienteNascimento) >
        (
          SELECT AVG(YEAR(GETDATE()) -YEAR(ClienteNascimento)) AS IDADE FROM Clientes
        );

2. Mostre qual agência tem quantidade de clientes acima da média.

  .. code-block:: sql
    :linenos:

    SELECT AgenciaNome, COUNT(ClienteCodigo) AS QDTE
    FROM Contas INNER JOIN Agencias
      ON Agencias.AgenciaCodigo = Contas.AgenciaCodigo
    GROUP BY AgenciaNome
    HAVING COUNT(ClienteCodigo) >
      (SELECT COUNT(DISTINCT ClienteCodigo)/
      COUNT(DISTINCT AgenciaCodigo) FROM Contas);

3. Mostre o nome da agência o saldo total, o mínimo, o máximo e a quantidade de clientes de cada agência.

4. Mostre o percentual que cada agencia representa no saldo total do banco.

5. Mostre as duas cidades que tem o maior saldo total

6. Mostre qual a agência tem o maior montante de emprestimo

7. Mostra qual o menor valor devido, o maior e o total devido	da tabela devedor

8. Mostre o nome do cliente, se ele tem cartão de crédito ou não 	apenas do cliente que é o maior devedor.
