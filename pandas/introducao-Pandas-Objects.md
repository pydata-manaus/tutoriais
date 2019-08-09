
# Introdução a Pandas Objects

No nível básico, os objetos Pandas podem ser considerados como versões aprimoradas de matrizes estruturadas ``NumPy`` nas quais as linhas e colunas são identificadas com rótulos, em vez de simples índices inteiros. O Pandas fornece uma série de ferramentas, métodos e funcionalidades úteis sobre as estruturas básicas de dados, mas quase tudo o que se segue requer uma compreensão do que são essas estruturas. Assim, existem três estruturas de dados fundamentais do Pandas: a ``Série``, o ``DataFrame`` e o ``Index``.

Vamos começar nossas sessões de código com as importações padrão de ``NumPy`` e ``Pandas``


```python
import numpy as np
import pandas as pd
```

## Series Objeto

O Pandas ``Series`` é uma matriz unidimensional de dados indexados. Pode ser criado a partir de uma lista ou matriz da seguinte forma:


```python
data = pd.Series([0.25, 0.5, 0.75, 1.0])
data
```




    0    0.25
    1    0.50
    2    0.75
    3    1.00
    dtype: float64



Como vemos na saída, a ``Series`` envolve uma sequência de valores e uma sequência de índices, que podemos acessar com os valores e os atributos de índice. Os valores são simplesmente uma matriz familiar do NumPy:


```python
data.values
```




    array([ 0.25,  0.5 ,  0.75,  1.  ])



O índice é um objeto tipo array do tipo ``pd.Index``.


```python
data.index
```




    RangeIndex(start=0, stop=4, step=1)



Como com uma matriz ``NumPy``, os dados podem ser acessados pelo índice associado por meio da notação de colchetes do Python:


```python
data[1]
```




    0.5




```python
data[1:3]
```




    1    0.50
    2    0.75
    dtype: float64



## Series e NumPy array

O objeto Series é basicamente intercambiável com um array NumPy unidimensional. A diferença essencial é a presença do índice: enquanto o Numpy Array tem um índice inteiro implicitamente definido usado para acessar os valores, o Pandas ``Series`` possui um índice explicitamente definido associado aos valores.


```python
data = pd.Series([0.25, 0.5, 0.75, 1.0],
                 index=['a', 'b', 'c', 'd'])
data
```




    a    0.25
    b    0.50
    c    0.75
    d    1.00
    dtype: float64



Acessando um item com o index:


```python
data['b']
```




    0.5



Podemos até usar índices não contíguos ou não sequenciais:


```python
data = pd.Series([0.25, 0.5, 0.75, 1.0],
                 index=[2, 5, 3, 7])
data
```




    2    0.25
    5    0.50
    3    0.75
    7    1.00
    dtype: float64




```python
data[5]
```




    0.5



## Series com dicionários

Um dicionário é uma estrutura que mapeia chaves arbitrárias para um conjunto de valores arbitrários, e uma ``Series`` é uma estrutura que mapeia chaves digitadas para um conjunto de valores digitados. Essa digitação é importante: assim como o código compilado específico do tipo por trás de um array NumPy torna mais eficiente do que uma lista Python para certas operações, as informações de tipo de uma série Pandas o tornam muito mais eficiente do que dicionários Python para determinadas operações.

A analogia entre séries e dicionários pode ser feita ainda mais claramente construindo um objeto ``Series`` diretamente de um dicionário em Python:


```python
population_dict = {'California': 38332521,
                   'Texas': 26448193,
                   'New York': 19651127,
                   'Florida': 19552860,
                   'Illinois': 12882135}
population = pd.Series(population_dict)
population
```




    California    38332521
    Texas         26448193
    New York      19651127
    Florida       19552860
    Illinois      12882135
    dtype: int64



Por padrão, uma série será criada onde o índice é extraído das chaves classificadas. A partir daqui, o acesso típico a itens de estilo de dicionário pode ser realizado:


```python
population['California']
```




    38332521



Ao contrário de um dicionário, no entanto, a ``Serie`` também oferece suporte a operações no estilo de matriz, como fatiamento:


```python
population['California':'Illinois']
```




    California    38332521
    Texas         26448193
    New York      19651127
    Florida       19552860
    Illinois      12882135
    dtype: int64



## Construindo Series objetos

Já vimos algumas maneiras de construir uma ``Series``Pandas a partir do zero; todos eles são alguma versão do seguinte:

```python
>>> pd.Series (data, index = index)
```

em que ``index`` é um argumento opcional e os dados podem ser uma das muitas entidades.

Por exemplo, ``data`` podem ser uma lista ou uma matriz NumPy, em cujo caso o ``index`` é padronizado para uma sequência inteira:


```python
pd.Series([2, 4, 6])
```




    0    2
    1    4
    2    6
    dtype: int64



``data`` pode ser um escalar, que são repetidos para preencher o índice especificado


```python
pd.Series(5, index=[100, 200, 300])
```




    100    5
    200    5
    300    5
    dtype: int64



os dados podem ser um dicionário, em que o índice é padronizado para as chaves de dicionário ordenadas:


```python
pd.Series({2:'a', 1:'b', 3:'c'})
```




    2    a
    1    b
    3    c
    dtype: object



Em cada caso, o índice pode ser explicitamente definido se um resultado diferente for preferido:


```python
pd.Series({2:'a', 1:'b', 3:'c'}, index=[3, 2])
```




    3    c
    2    a
    dtype: object



## Pandas DataFrame Objeto

A próxima estrutura fundamental no Pandas é o DataFrame. O ``DataFrame`` pode ser considerado como uma generalização de uma matriz NumPy ou como uma especialização de um dicionário Python.

## DataFrame como uma matriz NumPy generalizada

Se uma série é um análogo de uma matriz unidimensional com índices flexíveis, um ``DataFrame`` é um análogo de uma matriz bidimensional com índices de linha flexíveis e nomes de colunas flexíveis. Assim como você pode pensar em um array bidimensional como uma sequência ordenada de colunas unidimensionais alinhadas, você pode pensar em um ``DataFrame`` como uma sequência de objetos  ``Series`` alinhados. Aqui, por "alinhados", queremos dizer que eles compartilham o mesmo índice.

Para demonstrar isso, vamos primeiro construir uma nova ``Serie`` listando a área de cada um dos cinco estados discutidos na seção anterior:


```python
area_dict = {'California': 423967, 'Texas': 695662, 'New York': 141297,
             'Florida': 170312, 'Illinois': 149995}
area = pd.Series(area_dict)
area
```




    California    423967
    Texas         695662
    New York      141297
    Florida       170312
    Illinois      149995
    dtype: int64



Agora que temos isso junto com a série de população de antes, podemos usar um dicionário para construir um único objeto bidimensional contendo essas informações:


```python
states = pd.DataFrame({'population': population,
                       'area': area})
states
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>population</th>
      <th>area</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>38332521</td>
      <td>423967</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>26448193</td>
      <td>695662</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>19651127</td>
      <td>141297</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>19552860</td>
      <td>170312</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>12882135</td>
      <td>149995</td>
    </tr>
  </tbody>
</table>
</div>



Como o objeto ``Series``, o ``DataFrame`` possui um atributo de índice que dá acesso aos rótulos de índice:


```python
states.index
```




    Index(['California', 'Texas', 'New York', 'Florida', 'Illinois'], dtype='object')



Além disso, o ``DataFrame`` possui um atributo columns, que é um objeto Index que contém os rótulos da coluna:


```python
states.columns
```




    Index(['population', 'area'], dtype='object')



Assim, o ``DataFrame`` pode ser considerado como uma generalização de um array NumPy bidimensional, em que as linhas e colunas têm um índice generalizado para acessar os dados.

## DataFrame como dicionário especializado

Da mesma forma, também podemos pensar em um ``DataFrame`` como uma especialização de um dicionário. Onde um dicionário mapeia uma chave para um valor, um ``DataFrame`` mapeia um nome de coluna para uma série de dados de coluna. Por exemplo, pedir o atributo ``'area'`` retorna o objeto ``Series`` contendo as áreas que vimos anteriormente:


```python
states['area']
```




    California    423967
    Texas         695662
    New York      141297
    Florida       170312
    Illinois      149995
    Name: area, dtype: int64



Observe o potencial ponto de confusão aqui: em um array NumPy bidimensional, data [0] retornará a primeira linha. Para um ``DataFrame``, data ['col0'] retornará a primeira *coluna*. Por causa disso, provavelmente é melhor pensar em ``DataFrame`` como dicionários generalizados em vez de matrizes generalizadas, embora as duas formas de examinar a situação possam ser úteis.

## Construindo DataFrame objetos

Um DataFrame do Pandas pode ser construído de várias maneiras. 

### De um único objeto série

Um ``DataFrame`` é uma coleção de objetos da Série e um ``DataFrame`` de coluna única pode ser construído a partir de uma única Série:


```python
pd.DataFrame(population, columns=['population'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>38332521</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>26448193</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>19651127</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>19552860</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>12882135</td>
    </tr>
  </tbody>
</table>
</div>



### De uma lista de dicts

Qualquer lista de dicionários pode ser feita em um ``DataFrame``. Usaremos uma compreensão simples de lista para criar alguns dados:


```python
data = [{'a': i, 'b': 2 * i}
        for i in range(3)]
pd.DataFrame(data)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



Mesmo que algumas chaves no dicionário estejam faltando, os Pandas as preencherão com valores ``NaN`` (ou seja, "não um número"):


```python
pd.DataFrame([{'a': 1, 'b': 2}, {'b': 3, 'c': 4}])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>2</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>NaN</td>
      <td>3</td>
      <td>4.0</td>
    </tr>
  </tbody>
</table>
</div>



### De um dicionário de objetos de Serie

Como vimos antes, um ``DataFrame`` também pode ser construído a partir de um dicionário de objetos da série:


```python
pd.DataFrame({'population': population,
              'area': area})
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>population</th>
      <th>area</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>38332521</td>
      <td>423967</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>26448193</td>
      <td>695662</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>19651127</td>
      <td>141297</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>19552860</td>
      <td>170312</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>12882135</td>
      <td>149995</td>
    </tr>
  </tbody>
</table>
</div>



### De uma matriz NumPy bidimensional

Dada uma matriz bidimensional de dados, podemos criar um ``DataFrame`` com qualquer coluna especificada e nomes de índice. Se omitido, um índice inteiro será usado para cada um:


```python
pd.DataFrame(np.random.rand(3, 2),
             columns=['foo', 'bar'],
             index=['a', 'b', 'c'])
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>foo</th>
      <th>bar</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>0.324686</td>
      <td>0.661083</td>
    </tr>
    <tr>
      <th>b</th>
      <td>0.906190</td>
      <td>0.160109</td>
    </tr>
    <tr>
      <th>c</th>
      <td>0.919200</td>
      <td>0.961499</td>
    </tr>
  </tbody>
</table>
</div>



## De uma matriz estruturada NumPy


```python
A = np.zeros(3, dtype=[('A', 'i8'), ('B', 'f8')])
A
```




    array([(0,  0.), (0,  0.), (0,  0.)],
          dtype=[('A', '<i8'), ('B', '<f8')])




```python
pd.DataFrame(A)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>


