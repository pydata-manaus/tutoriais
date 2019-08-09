
## Seleção de dados em Series

Um objeto Series atua de várias maneiras, como um array NumPy unidimensional e, em muitos aspectos, como um dicionário padrão do Python. Se mantivermos essas duas analogias sobrepostas em mente, isso nos ajudará a entender os padrões de indexação e seleção de dados nesses arrays.

### Series de dicionario


```python
import pandas as pd
data = pd.Series([0.25, 0.5, 0.75, 1.0],
                 index=['a', 'b', 'c', 'd'])
data
```




    a    0.25
    b    0.50
    c    0.75
    d    1.00
    dtype: float64




```python
data['b']
```




    0.5



Também podemos usar expressões e métodos Python semelhantes a dicionários para examinar as chaves/índices e valores:


```python
'a' in data
```




    True




```python
data.keys()
```




    Index(['a', 'b', 'c', 'd'], dtype='object')




```python
list(data.items())
```




    [('a', 0.25), ('b', 0.5), ('c', 0.75), ('d', 1.0)]



Os objetos da série podem até ser modificados com uma sintaxe semelhante a um dicionário. Assim como você pode estender um dicionário atribuindo a uma nova chave, você pode estender uma série atribuindo a um novo valor de índice:


```python
data['e'] = 1.25
data
```




    a    0.25
    b    0.50
    c    0.75
    d    1.00
    e    1.25
    dtype: float64



### Series unidimensionais


```python
data['a':'c']
```




    a    0.25
    b    0.50
    c    0.75
    dtype: float64




```python
data[0:2]
```




    a    0.25
    b    0.50
    dtype: float64




```python
data[(data > 0.3) & (data < 0.8)]
```




    b    0.50
    c    0.75
    dtype: float64




```python
data[['a', 'e']]
```




    a    0.25
    e    1.25
    dtype: float64



Entre estes, o fatiamento pode ser a fonte da maior confusão. Observe que ao fatiar com um índice explícito (ou seja, data ['a': 'c']), o índice final é incluído na fatia, enquanto ao fatiar com um índice implícito (isto é, dados [0: 2]), o índice final é excluído da fatia.

### Indexação: loc, iloc e ix

Essas convenções de divisão e indexação podem ser uma fonte de confusão. Por exemplo, se sua série tiver um índice inteiro explícito, uma operação de indexação como data [1] usará os índices explícitos, enquanto uma operação de segmentação como data [1: 3] usará o índice implícito de estilo Python.


```python
data = pd.Series(['a', 'b', 'c'], index=[1, 3, 5])
data
```




    1    a
    3    b
    5    c
    dtype: object




```python
data[1]
```




    'a'




```python
data[1:3]
```




    3    b
    5    c
    dtype: object



Devido a essa confusão potencial no caso de índices inteiros, o Pandas fornece alguns atributos de indexador especiais que explicitamente expõem certos esquemas de indexação. Esses não são métodos funcionais, mas atributos que expõem uma interface de fatiamento específica aos dados da Série.


```python
data.loc[1]
```




    'a'




```python
data.loc[1:3]
```




    1    a
    3    b
    dtype: object



O atributo ``iloc`` permite a indexação e o fatiamento que sempre fazem referência ao índice implícito do estilo Python:


```python
data.iloc[1]
```




    'b'




```python
data.iloc[1:3]
```




    3    b
    5    c
    dtype: object



Um terceiro atributo de indexação, ix, é um híbrido dos dois, e para os objetos da Série é equivalente à indexação baseada no padrão []. 

Um princípio orientador do código Python é que "explícito é melhor que implícito". A natureza explícita de loc e iloc os torna muito úteis na manutenção de código limpo e legível; especialmente no caso de índices inteiros, recomendo usar ambos para tornar o código mais fácil de ler e entender, e para evitar erros sutis devido à convenção mista de indexação/fatiamento.

## Seleção de Dados em um DataFrame


```python
area = pd.Series({'California': 423967, 'Texas': 695662,
                  'New York': 141297, 'Florida': 170312,
                  'Illinois': 149995})
pop = pd.Series({'California': 38332521, 'Texas': 26448193,
                 'New York': 19651127, 'Florida': 19552860,
                 'Illinois': 12882135})
data = pd.DataFrame({'area':area, 'pop':pop})
data
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
      <th>area</th>
      <th>pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>423967</td>
      <td>38332521</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>695662</td>
      <td>26448193</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>141297</td>
      <td>19651127</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>149995</td>
      <td>12882135</td>
    </tr>
  </tbody>
</table>
</div>



As séries individuais que compõem as colunas do ``DataFrame`` podem ser acessadas por meio da indexação no estilo de dicionário do nome da coluna:


```python
data['area']
```




    California    423967
    Texas         695662
    New York      141297
    Florida       170312
    Illinois      149995
    Name: area, dtype: int64



De forma equivalente, podemos usar o acesso ao estilo de atributo com nomes de coluna que são strings:


```python
data.area
```




    California    423967
    Texas         695662
    New York      141297
    Florida       170312
    Illinois      149995
    Name: area, dtype: int64



Esse acesso de coluna no estilo de atributo acessa o mesmo objeto exato que o acesso ao estilo de dicionário:


```python
data.area is data['area']
```




    True



Embora esta seja uma taquigrafia útil, tenha em mente que ela não funciona para todos os casos! Por exemplo, se os nomes das colunas não forem sequências, ou se os nomes das colunas entrarem em conflito com os métodos do DataFrame, esse acesso ao estilo do atributo não será possível. Por exemplo, o DataFrame tem um método pop (), portanto, o data.pop apontará para essa coluna em vez da coluna "pop":


```python
data.pop is data['pop']
```




    False



Em particular, você deve evitar a tentação de tentar a atribuição de colunas via atributo (isto é, usar data ['pop'] = z em vez de data.pop = z).

Essa sintaxe no estilo de dicionário também pode ser usada para modificar o objeto, neste caso, adicionando uma nova coluna:


```python
data['density'] = data['pop'] / data['area']
data
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
      <th>area</th>
      <th>pop</th>
      <th>density</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>423967</td>
      <td>38332521</td>
      <td>90.413926</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>695662</td>
      <td>26448193</td>
      <td>38.018740</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>141297</td>
      <td>19651127</td>
      <td>139.076746</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
      <td>114.806121</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>149995</td>
      <td>12882135</td>
      <td>85.883763</td>
    </tr>
  </tbody>
</table>
</div>



### DataFrame bidimensional 

Como mencionado anteriormente, também podemos visualizar o DataFrame como uma matriz bidimensional aprimorada. Podemos examinar o array de dados subjacente bruto usando o atributo values:


```python
data.values
```




    array([[  4.23967000e+05,   3.83325210e+07,   9.04139261e+01],
           [  6.95662000e+05,   2.64481930e+07,   3.80187404e+01],
           [  1.41297000e+05,   1.96511270e+07,   1.39076746e+02],
           [  1.70312000e+05,   1.95528600e+07,   1.14806121e+02],
           [  1.49995000e+05,   1.28821350e+07,   8.58837628e+01]])



Com essa imagem em mente, muitas observações familiares parecidas com uma matriz podem ser feitas no próprio DataFrame. Por exemplo, podemos transpor o DataFrame completo para trocar linhas e colunas:


```python
data.T
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
      <th>California</th>
      <th>Texas</th>
      <th>New York</th>
      <th>Florida</th>
      <th>Illinois</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>area</th>
      <td>4.239670e+05</td>
      <td>6.956620e+05</td>
      <td>1.412970e+05</td>
      <td>1.703120e+05</td>
      <td>1.499950e+05</td>
    </tr>
    <tr>
      <th>pop</th>
      <td>3.833252e+07</td>
      <td>2.644819e+07</td>
      <td>1.965113e+07</td>
      <td>1.955286e+07</td>
      <td>1.288214e+07</td>
    </tr>
    <tr>
      <th>density</th>
      <td>9.041393e+01</td>
      <td>3.801874e+01</td>
      <td>1.390767e+02</td>
      <td>1.148061e+02</td>
      <td>8.588376e+01</td>
    </tr>
  </tbody>
</table>
</div>



No entanto, quando se trata de indexação de objetos DataFrame, fica claro que a indexação de colunas em estilo de dicionário exclui nossa capacidade de simplesmente tratá-la como uma matriz NumPy. Em particular, passar um único índice para uma matriz acessa uma linha:


```python
data.values[0]
```




    array([  4.23967000e+05,   3.83325210e+07,   9.04139261e+01])



e passar um único "index" para um DataFrame acessa uma coluna:


```python
data['area']
```




    California    423967
    Texas         695662
    New York      141297
    Florida       170312
    Illinois      149995
    Name: area, dtype: int64



Assim, para indexação no estilo array, precisamos de outra convenção. Aqui, o Pandas usa novamente os indexadores ``loc, iloc`` e ``ix`` mencionados anteriormente. Usando o indexador iloc, podemos indexar a matriz subjacente como se fosse uma matriz NumPy simples (usando o índice de estilo Python implícito), mas os rótulos de índice e coluna DataFrame são mantidos no resultado:


```python
data.iloc[:3, :2]
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
      <th>area</th>
      <th>pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>423967</td>
      <td>38332521</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>695662</td>
      <td>26448193</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>141297</td>
      <td>19651127</td>
    </tr>
  </tbody>
</table>
</div>



Da mesma forma, usando o indexador loc, podemos indexar os dados subjacentes em um estilo de matriz, mas usando os nomes explícitos de índice e coluna:


```python
data.loc[:'Illinois', :'pop']
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
      <th>area</th>
      <th>pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>423967</td>
      <td>38332521</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>695662</td>
      <td>26448193</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>141297</td>
      <td>19651127</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>149995</td>
      <td>12882135</td>
    </tr>
  </tbody>
</table>
</div>



O indexador ``ix`` permite um híbrido dessas duas abordagens:


```python
data.ix[:3, :'pop']
```

    /home/jailsonpereira/anaconda3/envs/events/lib/python3.6/site-packages/ipykernel_launcher.py:1: DeprecationWarning: 
    .ix is deprecated. Please use
    .loc for label based indexing or
    .iloc for positional indexing
    
    See the documentation here:
    http://pandas.pydata.org/pandas-docs/stable/indexing.html#ix-indexer-is-deprecated
      """Entry point for launching an IPython kernel.





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
      <th>area</th>
      <th>pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>423967</td>
      <td>38332521</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>695662</td>
      <td>26448193</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>141297</td>
      <td>19651127</td>
    </tr>
  </tbody>
</table>
</div>




```python
data.loc[data.density > 100, ['pop', 'density']]
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
      <th>pop</th>
      <th>density</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>New York</th>
      <td>19651127</td>
      <td>139.076746</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>19552860</td>
      <td>114.806121</td>
    </tr>
  </tbody>
</table>
</div>



Qualquer uma dessas convenções de indexação também pode ser usada para definir ou modificar valores; isso é feito da maneira padrão que você pode estar acostumado a trabalhar com o NumPy:


```python
data.iloc[0, 2] = 90
data
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
      <th>area</th>
      <th>pop</th>
      <th>density</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>California</th>
      <td>423967</td>
      <td>38332521</td>
      <td>90.000000</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>695662</td>
      <td>26448193</td>
      <td>38.018740</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>141297</td>
      <td>19651127</td>
      <td>139.076746</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
      <td>114.806121</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>149995</td>
      <td>12882135</td>
      <td>85.883763</td>
    </tr>
  </tbody>
</table>
</div>



### Convenções adicionais de indexação


```python
data['Florida':'Illinois']
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
      <th>area</th>
      <th>pop</th>
      <th>density</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
      <td>114.806121</td>
    </tr>
    <tr>
      <th>Illinois</th>
      <td>149995</td>
      <td>12882135</td>
      <td>85.883763</td>
    </tr>
  </tbody>
</table>
</div>



Essas fatias também podem se referir a linhas por número em vez de por índice:


```python
data[1:3]
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
      <th>area</th>
      <th>pop</th>
      <th>density</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Texas</th>
      <td>695662</td>
      <td>26448193</td>
      <td>38.018740</td>
    </tr>
    <tr>
      <th>New York</th>
      <td>141297</td>
      <td>19651127</td>
      <td>139.076746</td>
    </tr>
  </tbody>
</table>
</div>



Da mesma forma, as operações de mascaramento direto também são interpretadas em linha, em vez de em coluna:


```python
data[data.density > 100]
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
      <th>area</th>
      <th>pop</th>
      <th>density</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>New York</th>
      <td>141297</td>
      <td>19651127</td>
      <td>139.076746</td>
    </tr>
    <tr>
      <th>Florida</th>
      <td>170312</td>
      <td>19552860</td>
      <td>114.806121</td>
    </tr>
  </tbody>
</table>
</div>



Essas duas convenções são sintaticamente semelhantes àquelas de uma matriz NumPy, e embora possam não se ajustar exatamente ao modelo das convenções dos Pandas, elas são, no entanto, bastante úteis na prática.
