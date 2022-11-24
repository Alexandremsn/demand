<div align="center">
<img src="images/grocerybasket.png" width="400">

# Corporación Favorita
<div align="left">
Uso de uma base de dados para desenvolvimento de um modelo preditivo de deep learning para prever a demanda dos 20 produtos mais vendidos em uma rede de lojas de varejo.
  

## Tecnologia

Os softwares  usados neste projeto foram:

* Python version  3.9.5
* Dbeaver 22.1.1
* MySql Workbench 8.0
  
  
  
## Serviços Usados

* Github
* Colab
* Google Drive

## Bibliotecas Python

* Pandas
* Numpy
* Matplotlib.pyplot
* Seaborn
* Sklearn.impute
* Sklearn.metrics
* Sklearn.preprocessing
* Sklearn.neighbors
* Catboost
* Keras
  
  
## Como foi feito

Será descrito abaixo através de textos e imagens.

  
  
Para este projeto usamos diversas bases de dados, como de transações, preço do petróleo, feriados, dados das lojas e a base de dados principal
tratamos as bases separadamente, e posteriormente integramos os dados necessários ao nosso modelo na base principal.
Em um primeiro momento recorremos ao SQL para filtrar a base pois a base original possui algo em torno de 125 milhões de linhas e devido as minhas limitações computacionais elaboramos um script para filtrar os 20 produtos mais vendidos e exportar o resultado desta consulta em um arquivo .csv, ficamos com uma base de aproximadamente 1,6 milhões de linhas.
  
### Ambiente SQL
  
Primeiramente criamos o banco de dados a tabela e alimentamos ela neste banco de dados
  
<img src=images/sql_08.png> 
  
Depois filtramos os 20 maiores valores em unidades vendidas totais.
Como resultado desta consulta copiamos estes id de produtos e salvamos em um arquivo texto para uso futuro
  
<img src=images/sql_09.png> 
  
Finalmente consultamos apenas os valores da lista gerada pela consulta anterior e limitamos a consulta apenas a estes valores, foi computacionalmente mais rápido como íamos consultar poucos valores é um caminho válido, pois um join entre tabelas deste tamanho seria relativamente demorado.
Exportamos nossa base reduzida para um arquivo .csv para uso no Python.
  
<img src=images/sql_final.png> 
  
### Tabela de treino e teste  
  
Agora no Python vamos trabalhar nos dados de treino e teste.
Esta lista foi gerada baseada na consulta sql anteriormente realizada, que nos descreveu os 20 produtos mais vendidos.  

<img src=images/cff_006.png>

Após filtrar os dados ficamos com uma base de teste reduzida contendo apenas os 20 produtos mais vendidos

  
<img src=images/cff_007.png>


Importamos a base de treino e esta ficou desformatada

<img src=images/cff_008.png>
  
Arrumamos o index e inserimos os nomes das colunas deixando com este aspecto abaixo


<img src=images/cff_009.png>
 
### Tabela de preço do petróleo
  

A Segunda base que vamos tratar seria a de preço do petróleo
utilizamos a ferramenta KNNImputer para preencher os valores faltantes
<img src=images/cff_001.png>

esta base como sendo uma série histórica para ser usada em nosso modelo preditivo ela deve também ser prevista pois como trabalharemos com dados futuros a prever também precisaremos prever este valor.
assim começamos como primeiro passo separando a base em teste e treino para validar nosso modelo preditivo.

<img src=images/cff_002.png>
  
Construímos nosso modelo, variamos o hiperparâmetro n_neighbors, que o mais determinante neste tipo de modelo de Nearest Neighbors.
Assim variando este hiperpaâmetro obtivemos um erro com um patamar mais baixo possível com os dados disponíveis.
Como para este dado estamos construindo um modelo preditivo para prever um total de 15 dias não estamos interessados em prever na vírgula o valor, mas prever o patamar que ele vai estar nestes 15 dias que o modelo deverá prever.

<img src=images/cff_003.png>

Agora vamos treinar novamente a base com todos os dados de treino e usar este modelo para prever a base de teste.
   
<img src=images/cff_004.png>

Agora temos uma base de teste com a coluna de preço do petróleo definida de acordo com a previsão de nosso modelo.
  
<img src=images/cff_005.png>

### Tabela de feriados e lojas
  
A terceira base que vamos precisar tratar seria a base de feriados, vamos cruzar esta tabela com a tabela das localidades das lojas para obter o resultado de que lojas são afetadas por qual feriado.

Assim temos os dados de lojas com seu id de loja(store_nbr) e onde se localiza.
As demais informações da tabela não utilizaremos.
  
 <img src=images/cff_010.png> 
 
 
 Aqui temos a tabela de feriados
  
 <img src=images/cff_011.png>
 
 Verificamos quais as ocorrências de cidades e estados para começar a pensar como abordar este problema de cruzamento.
  
 <img src=images/cff_012.png>
  
  Agora verificamos os tipos de feriados e verificamos 3 categorias, municipal, estadual e nacional.
  Verificamos também os tipos de feriados
  
 <img src=images/cff_013.png> 
  
  
Agora vamos construir uma função que nos devolve uma lista com as lojas afetadas por cada feriado
  
 <img src=images/cff_014.png>
  
Assim ficamos com este resultado
  
<img src=images/cff_015.png>  

Depois escrevemos outra função que vai dividir os feriados em diferentes classificações
 
  
<img src=images/cff_016.png>

Assim verificamos que rodou corretamente e a coluna classification foi criada e povoada corretamente com valores desejados
  
  
 <img src=images/cff_017.png>
  
  Criamos uma função para classificar os dias de mais venda véspera do natal e véspera do dia das mães, assim criamos a coluna major_holiday
  Por fim ficamos com este resultado.
  
  <img src=images/cff_018.png>
  
  ### Pré Processamento

  importamos todas estas tabelas para uso.
  Agora vamos deixar os dados prontos para usar na EDA e no nosso modelo preditivo 
  
  <img src=images/cff_019.png>
  
  Assim temos estes valores para a tabela de treino originalmente alteramos o tipo da data de cadeia de caracteres para Datetime
  
  <img src=images/cff_020.png>
  
  
Ao cruzar as tabelas de treino e preço de petróleo percebemos valores nulos, pelo fato que a loja funciona finais de semana mas as bolsas de commodities não, assim precisamos escrever uma função para copiar os valores de sexta-feira para os dias do final de semana.
  
  
  <img src=images/cff_022.png>
  
  
 Após aplicar a função criamos também a coluna que descreve o dia da semana equivalente do valor da data, assim podemos trabalhar melhor os apectos sazonais dos diferentes dias de semanas e verificar se existe este padrão para estes dados ou não. por fim ficamos com estas colunas e com todos os valores preenchidos.
  
  
  <img src=images/cff_023.png>
  
Agora vamos trazer as informações de classificação do feriado e feriado relevante para a tabela de dados principais
escrevemos este código para mapear os dados e puxar os valores que encontrar cruzando se a loja correspondente da linha está na relação de lojas afetadas para aquela data.
  
  <img src=images/cff_024.png>
  
 Criamos também um código para trazer da tabela de produtos alguns dados para a tabela principal: o grupo de produto, a classe de produto e  se é perecível
  
  
 <img src=images/cff_025.png>
  
  
  
 Por fim convertermos as linhas categóricas para variáveis dummies.
  
  
  
  <img src=images/cff_026.png>
  
  
  ### EDA
  
Nesta etapa vamos fazer algumas análises dos dados para ver se é pertinente manter as variáveis criadas e se é possível visualizar padrões.
  
A partir deste ponto do projeto usamos a funcionalidade de integração do Google Drive com o colab para carregar instantaneamente os arquivos, pois com a criação das variáveis dummies estávamos com um número grande de colunas e o tempo de carregamento para uso em mais de uma estação de trabalho começou a ser muito substancial.
  
  
  <img src=images/cff_027.png>
  
  
  
Referente a transações analisando a linha da variável alvo, verificamos que a loja possui uma correlação mais forte entre as variáveis, assim talvez um modelo por cada loja pode ser uma alternativa para prever este tipo de variável, vimos algum grau de correlação entre o feriado de natal e os dias de final de semana, também vermos na véspera de feriado uma correção moderada
  
  
  <img src=images/cff_028.png>
  
  
  
Agora quanto à unidades vendidas vimos uma correlação com: promoção, dia da semana e  colunas de classificação de produtos. Também se percebeu uma correlação moderada com os feriados.
  
  
  <img src=images/cff_29a.png>
  
  
  
Separamos os dados por categoria de produto, padaria e bebidas estas categorias foram escolhidas pois tiveram maior correlação com a nossa variável alvo, 'unidades vendidas'. 
  
Plotando unidades vendidas por data e destacando o sábado, vimos claramente uma região de concentração
 
  
  
  <img src=images/cff_030.png>
  
  
  
 Depois trocando a legenda para o dia com pontos mais altos vemos uma concentração também
  
  
  
  <img src=images/cff_031.png>
  
  
  Do Mesmo modo com o dia com pontos mais baixos.
  
  
  <img src=images/cff_032.png>

  
  Para os feriados vimos uns pontos de concentração para o setor de bebidas mas para o setor de padaria um padrão bem mais disperso 
  
  
  <img src=images/cff_033.png>
  
  
  Para Transações vimos os dias da semana claramente também afetando e criando camadas bem definidas.
  
  
  
  
  <img src=images/cff_034.png>
  
  
  
  Para feriados vimos nas transações que eles se encaixam com os pontos mais altos
  
  
  <img src=images/cff_035.png>
  
  
  Assim vimos que de um modo geral as variáveis criadas podem ser usadas por nosso modelo para compreender as diferenças dos dados, mas, que para alguns produtos seu  impacto ocorre de forma diferente mas as variáveis ainda são válidas para construção de nosso modelo.
  
 ### Construindo o modelo preditivo para transações
  
Importamos as bibliotecas e criamos uma função para separar os dados em treino e teste para séries temporais
  
  
  <img src=images/cff_036.png>
  
  
  
  Para definir os hiperparâmetros vamos seleciona-los com base em uma loja e depois escalamos o modelo para as demais lojas, 
  Selecionamos as variáveis e normalizamos a variável numérica
  
  
  <img src=images/cff_037.png>
  
  
Testamos uma abordagem de boost mas obtivemos um resultado bem similar mas o tempo de processamento foi muito maior assim, optaremos em seguir com o modelo de rede neural
  
  
  <img src=images/cff_038.png>
  
  
  
Assim definimos o nosso modelo de rede neural testamos outras variações de camadas, mas, esta configuração foi a que performou melhor.
  
  
  <img src=images/cff_039.png>
  
  
 O resultado do método de rede neural foi similar como explicado acima
  

 <img src=images/cff_040.png>
  
Agora escalamos o modelo para todas as lojas.
  
  
  <img src=images/cff_042.png>
  
  
 Definimos uma função para passar as informações de transações para o dataset de treino e usamos nosso modelo para prever os valores do dataset de teste.
  
  <img src=images/cff_041.png>
 
  Dropamos as variáveis Auxiliares e salvamos o dataset de treino e teste com as transações
  
  <img src=images/cff_043.png>
  
  ### Modelo Preditivo Final
  
  Criamos mais uma variável Dummie para as lojas
    
  
  <img src=images/cff_044.png>
  
  
 
  Criamos uma tendência linear identificando a passagem do tempo em dias
  
  
  <img src=images/cff_045.png>
  
  
  
  Selecionamos as variáveis
  
  
  <img src=images/cff_046.png>
  
  
  
  Normalizamos as variáveis numéricas
  
  
  <img src=images/cff_047.png>
  
  
  Definimos o nosso modelo
  
  
  
  <img src=images/cff_048.png>
  
  Após vários testes escalamos este modelo para todos os produtos da mesma forma como feito com o modelo de transações que foi escalado para lojas
  assim tivemos este relatório final do modelo
  
  <img src=images/cff_049.png>
  
  Esta tabela ilustra os melhores e piores desempenhos preditivos do modelo, foram criadas métricas para avaliar, como, média de unidades vendidas, o erro , o erro percentual em relação à média, a variação entre o menor e maior valor, o desvio padrão, o valor mínimo, o total de valores negativos, e coeficiente de variação.
  
  <img src=images/cff_050.png>
  
  
Como era esperado o modelo performou pior com dados mais heterogêneos, porém esta não é a única explicação, temos concorrentemente a variação, alto índice de devoluções também impactando de forma geral negativamente, porém de forma diferente em diferentes produtos, indo de impactar claramente a simplesmente não ter impacto nenhum no erro. olhando os agrupamentos por classe de produto e tipo de produto, não observamos pelo menos com os 20 produtos que temos um padrão claro.
Para os dados temos o valor de unidades vendidas que se dá por vendas - devoluções, mas como não temos esta separação, seria um dado interesante para agregar, pois tratando os dois valores em separado poderíamos observar melhor a influêcia ou não das devoluções nas vendas. outro ponto seria expandir o modelo para poder mensurar de forma melhor a influência das classes e tipos de produtos na assertividade do modelo.


## Recursos Usados

  - Criação de banco de dados e tabelas
  - Consulta em abiente SQL
  - Exportar resultado da consulta como CSV
  - Importação de Database
  - Limpeza de dados
  - Criação de um DataFrame
  - Análise de parâmetros estatísticos
  - Análise de gráficos
  - Criação de variáveis
  - Imputação de valores faltantes
  - Teste e validação de um modelo de Nearest Neighbors
  - Teste e validação de dois modelos preditivos de rede neural utilizando o Keras
  

## Links

  - Repositório: https://github.com/Alexandremsn/demand
  - Se for encontrado um bug, favor entrar em contato alexandremsneto1986@gmail.com


## Versão

1.0.0.0


## Autor

* **Alexandre Machado da Silva Neto**: @alexandremsn (https://github.com/alexandremsn)
