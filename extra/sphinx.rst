Como Compilar o Material com o Sphinx
=====================================

Instalar o Python
-----------------

- Usar preferencialmente a versão 3.
- Ambientes Unix provavelmente já possuem ele instalado.
- Pode ser encontrado em https://www.python.org/downloads/.
- No Windows, durante a instalação marcar para instalar o "pip" também.


Instalar as Dependências
------------------------

Dentro do diretório do código do material executar:

.. code-block:: sh

  pip install -r requirements.txt


Compilar o Material
-------------------

Para listar as opções de compilação execute:

.. code-block:: sh

  make help

Alguns exemplos:

HTML
  .. code-block:: sh

    make html

PDF
  .. code-block:: sh

    make latexpdf

  .. note::

    É necessário que o LaTeX esteja instalado no sistema para gerar o PDF.

ePub
  .. code-block:: sh

    make epub

.. note::

  Após a execução do comando, o material compilado, junto com alguns outros arquivos, podem ser encontrados dentro do diretório ``_build`` na raíz do projeto.
