<div align="center">
<img src="images/diamonds-transparent-background-20.png" width="400">

# Corporación Favorita
<div align="left">
Uso de uma base de dados de características e preços de diamantes, limpeza de dados e construção de um modelo de regressão linear para prever os preços de uma segunda base de dados. 


## Tecnologia

Os softwares  usados neste projeto foram:

* Python version  3.9.5

## Serviços Usados

* Github


## Bibliotecas Python

* Pandas
* Numpy
* Matplotlib.pyplot
* Seaborn
* Sklearn.model_selection.train_test_split
* Sklearn.linear_model.LinearRegression
* Sklearn.metrics
* Sklearn.preprocessing

## Como foi feito

Será descrito abaixo através de textos e imagens.

  
  
Para este projeto usamos diversas bases de dados, como de transções, preço do petróleo, feriados, dados das lojas e a base de dados principal
tratamos as bases separadamente, e posteriormente integramos os dados necessários ao nosso modelo na base principal.
Em um primeiro momento recoeremos ao SQL para filtrar a base pois a base original possui algo em torno de 125 milhões de linhas e devido as minhas limitações computacionais elaboramos um script para filtrar os 20 produtos mais vendidos e exportar o resultado desta consulta em um arquivo csv, ficamos com uma base de aproximadamente 1,6 millhoes de linhas.
  
### Ambiente SQL
  
Primeiramente criamos o banco de dados a tabela e alimentamos ela neste banco de dados
  
<img src=images/sql_08.png> 
  
Depois filtramos os 20 maiores valores em unidades vendidas totais,
como resultado desta consulta copiamos estes id de produtos e salvamos em um aquivo texto para uso futuro
  
<img src=images/sql_09.png> 
  
Finalmente consultamos apenas os valores da lista gerada pela consulta anterior e limitamos a consulta apenas a estes valores, foi computacionalmente mais rápido como iamos consultar poucos valores é um caminho válido, pois um join entre tabelas deste tamanho seria relativamente demorado.
Exportamos nossa base reduzida para um arquivo .csv para uso no Python.
  
<img src=images/sql_final.png> 
  
### Tabela de treino e teste  
  
Agora no Python vamos trabalhar nos dados de treino e teste.
esta lista foi gerada baseada na consulta sql anteriomente realizada, que nos descreveu os 20 produtos mais vendidos.  

<img src=images/cff_006.png>

Após filtrar os dados ficamos com uma base de teste reduzida contendo apenas os 20 produtos mais vendidos

  
<img src=images/cff_007.png>


Importamos a base de treino e esta ficou desformatatada

<img src=images/cff_008.png>
  
Arrumamos o index e inserimos os nomes das colunas deixando com este aspécto abaixo


<img src=images/cff_009.png>
 
### Tabela de preço do petróleo
  

A Segunda base que vamos tratar seria a de preço do petróleo
utilizamos a ferramenta KNNImputer para preencher os valores faltantes
<img src=images/cff_001.png>

esta base como sendo uma série histórica para ser usada em nosso modelo preditivo ela deve também ser prevista pois como trabalharemos com dados futuros a prever também precisaremos prever este valor.
assim começamos como primeiro passo separando a base em teste e treino para validar nosso modelo preditivo.

<img src=images/cff_002.png>
  
Contruímos nosso modelo, variamos o hiperparâmetro n_neighbors, que o mais determinante neste tipo de modelo de Nearest Neighbors.
Assim variando este hiperpaâmetro obtivemos um erro com um patamar mais baixo possível com os dados disponíveis.
Como para este dado estamos contruindo um modelo preditivo para prever um total de 15 dias não estamos interresado em prever na vírgula o valor, mas prever o patamar que ele vai estar nestes 15 dias que o modelo deverá prever.

<img src=images/cff_003.png>

Agora vamos treinar novamente a base com todos os dados de treino e usar este modelo para prever a base de teste.
   
<img src=images/cff_004.png>

Agora temos uma base de teste com a coluna de preço do pretóleo definida de acordo com a previsão de nosso modelo.
  
<img src=images/cff_005.png>

### Tabela de feriados e lojas
  
A terceira base que vamos precisar tratar seria a base de feriados, vamos cruzar esta tabela com a tabela das localidades das lojas para obter o resultado de que lojas são afetadas por qual feriado.

Assim temos os dados de lojas com eu id de loja(store_nbr) e onde se localiza, as demais informações da tabela não utilizaremos
  
 <img src=images/cff_010.png> 
 
 
 Aqui temos a tabela de feriados
  
 <img src=images/cff_011.png>
  
 Verificamos quais as ocorências de cidades e estados para começar a pensar como abordar este problema de cruzamento
  
 <img src=images/cff_012.png>
  
  Agora verificamos os tipos de feriados e verificamos 3 categorias municipal, estadual e nacional
  Verificamos também os tipos de feriados
  
 <img src=images/cff_013.png> 
  
  
Agora vamos construir uma função que nos devolve uma lista com as lojas afetadas por cada feriado
  
 <img src=images/cff_014.png>
  
Assim ficamos com este resultado
  
<img src=images/cff_015.png>  

Depois escrevemos outra função que vai dividir os feriados em diferentes classificações
 
  
<img src=images/cff_016.png>

Assim verificamos que rodou corretameten e a coluna classification foi criada e povoada corretamente com valores desejados
  
  
 <img src=images/cff_017.png>
  
  Criamos uma função para classificar os dias de mais venda véspera do natal e véspera do dia das mães, assim criamos a coluna major_holiday
  Por fim ficamos com este resultado.
  
  <img src=images/cff_018.png>
  
  ### Pré Processamento

  importamos todas estas tabelas para uso
  agora vamos deixar os dados prontos para usar na EDA e no nosso modelo preditivo 
  
  <img src=images/cff_019.png>
  
  Assim
  
  <img src=images/cff_020.png>
  
Futuramente, seria interessante, testar mais extensamente as combinações de função logarítmica e dados sem transformação e usar outros métodos como decision forest para obter melhores resultados.


## Recursos Usados

  - Importação de Database
  - Limpeza de dados
  - Criação de um DataFrame
  - Análise de parâmetros estatísticos
  - Análise de gráficos
  - Construção de um modelo de regressão linear
  - Teste e validação deste modelo
  

## Links

  - Repositório: https://github.com/Alexandremsn/Ricks_Diamonds
  - Se for encontrado um bug, favor entrar em contato alexandremsneto1986@gmail.com


## Versão

1.0.0.0


## Autor

* **Alexandre Machado da Silva Neto**: @alexandremsn (https://github.com/alexandremsn)
