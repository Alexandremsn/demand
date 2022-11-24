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
  
Vamos trabalhar nos dados de treino e teste.
esta lista foi gerada baseada na consulta sql anteriomente realizada, que nos descreveu os 20 produtos mais vendidos.  

<img src=images/cff_006.png>

Após filtrar os dados ficamos com uma base de teste reduzida contendo apenas os 20 produtos mais vendidos/

  
<img src=images/cff_007.png>


importamos a base de treino e esta está


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


Realizamos novamente o pairplot e tivemos uma atenuação do formato price x carat
  
<img src=images/spair%20(1).png>
  
Vamos analisar mais a fundo esta variável plotamos ela individualmente.
Percebemos uma distância variável da região de concentração.

<img src=images/line_01.png>
  
Aplicamos a escala logarítmica para suavizar o ruído e obtivemos sucesso agora a distância está mais uniforme.  
Assim fazia sentido usar escala logarítmica na construção de nosso modelo.
  

  
<img src=images/line_02.png>  
 
  
Por fim a base ficou desta forma e decidimos adicionar a variável volume que seria a multiplicação dos 3 eixos
em um teste de correlação apresentou uma correlação maior que os eixos individuais. 
  
<img src=images/diamond_005.png>

Na de estimação de dados tivemos uma linha com ausência de alguns dados, assim para a coluna que faltava apenas o z usaremos a fórmula fornecida pelo codebook, e para a linha com ausência de dados nas 3 dimensões vamos usar uma coluna para doar os dados
assim pegamos uma informação de outra coluna com mesmo depth e mesma table.


<img src=images/diamond_006.png>
  
Assim aplicamos esta função para o cálculo do volume.
  
<img src=images/diamond_007.png>

E esta função para o cálculo das dimensões faltantes.

<img src=images/diamond_008.png>

Executamos as funções foi necessário adicionar 1 nas categorias, pois como usaríamos escala logarítmica o valor zero daria erro na função assim reajustamos a escala para partir de 1.


<img src=images/diamond_009.png>

Construindo o modelo, para as variáveis dimensionais tínhamos pouco ruído assim decidi não aplicar a elas a função logarítmica.

  
<img src=images/diamond_010.png>

Rodamos o teste da função e os números para o teste parecem muito promissores apesar de estarem em escala logarítmica ainda assim deram um valor baixo.

<img src=images/diamond_011.png>

Agora aplicamos nosso modelo ao dataset que queremos prever os preços.

<img src=images/diamond_012.png>

Salvamos em um arquivo .csv
  
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
