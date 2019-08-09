
# Manipulação de Dados com Pandas

O Pandas é um pacote, construído sobre o NumPy, e fornece uma implementação eficiente de um ``DataFrame``. Os ``DataFrames`` são essencialmente matrizes multidimensionais com rótulos de linha e coluna anexados e geralmente com tipos heterogêneos e/ou dados ausentes. Além de oferecer uma interface de armazenamento conveniente para dados rotulados, o Pandas implementa várias operações de dados poderosas, familiares aos usuários de estruturas de bancos de dados e programas de planilhas.

A estrutura de dados ``ndarray`` do ``NumPy`` fornece recursos essenciais para o tipo de dados limpos e bem organizados normalmente vistos em tarefas de computação numérica. Embora sirva a esse propósito muito bem, suas limitações tornam-se claras quando precisamos de mais flexibilidade (por exemplo, anexar rótulos a dados, trabalhar com dados ausentes, etc.) e ao tentar operações que não mapeiam bem a transmissão elementar (por exemplo, agrupamentos, pivôs, etc.), cada um dos quais é uma parte importante da análise dos dados menos estruturados disponíveis em muitas formas no mundo ao nosso redor. Os pandas, e em particular seus objetos ``Series`` e ``DataFrame``, se baseiam na estrutura do array ``NumPy`` e fornecem acesso eficiente a esses tipos de tarefas de "coleta de dados" que ocupam muito do tempo de um cientista de dados.

## Instalando o Pandas

A instalação de Pandas no seu sistema requer que o NumPy seja instalado e, se você construir a biblioteca a partir do código-fonte, requer as ferramentas apropriadas para compilar as fontes C e Cython nas quais o Pandas é construído.

Detalhes sobre a instalção podem ser encontradas em  [Preface](00.00-Preface.ipynb) e o Pandas por padrão vem instalado junto com o Anaconda.

Depois que o Pandas estiver instalado, você poderá importá-lo e verificar a versão:


```python
import pandas
pandas.__version__
```




    '0.24.2'



Nós importamos os Pandas com a convenção pd:


```python
import pandas as pd
```

## Lembrete Sobre a Documentação

Não se esqueça de que o IPython oferece a capacidade de explorar rapidamente o conteúdo de um pacote (usando o recurso de conclusão de guias), bem como a documentação de várias funções (usando o caractere?). (Consulte a Ajuda e Documentação no IPython se precisar de uma atualização sobre isso.)

Por exemplo, para exibir todo o conteúdo do namespace pandas, você pode digitar
```ipython
In [3]: pd. <TAB>
```    

E para exibir a documentação integrada do Pandas, você pode usar isto:

```ipython
In [4]: pd?
```
    
Documentação mais detalhada, juntamente com tutoriais e outros recursos, pode ser encontrada em http://pandas.pydata.org/.
