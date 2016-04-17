.. _EditorConfig: http://editorconfig.org/
.. _reStructuredText: http://docutils.sourceforge.net/rst.html

:orphan:

Como Contribuir?
----------------

- Crie um fork do projeto no GitHub.
- Faça suas alterações no seu fork.

  - Se possível, utilize o plugin EditorConfig_ no seu editor de texto.
  - Escreva o conteúdo usando reStructuredText_.

    - Fique atento a marcação dos títulos.
    - Utilize um bloco de código com syntax highlight para código.

      Exemplo:

      .. code-block:: rst
        :linenos:

        .. code-block:: sql

          SELECT * FROM tabela;

  - Adicione os link nos arquivos ``index.rst`` caso tenha criado algum arquivo novo.
  - Adicione os arquivos modificados (``git add``) e faça o commit (``git commit``).

- Crie um pull request no GitHub.
- Espere sua contribuição ser aprovada.
