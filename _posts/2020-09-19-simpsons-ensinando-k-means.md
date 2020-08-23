---
layout: post
title: "Aprendendo k-means com os simpsons [Não está pronto ainda!]"
subtitle: "Vamos aprender como funciona um dos algoritmos mais famosos de machine learning com os personagens mais famosos de todos os tempos?"
date: 2020-09-19 19:00:00 -0300
background: '/img/simpsons-k-means/1.jpg'
---


Imagine-se na seguinte situação, o desenho dos simpsons tem milhares(ou seria centenas?) de personagens e você precisa vender donuts para esses personagens, parece simples né?! Mas e se eu te disser que cada um deles tem gostos diferentes, cada um deles gostam de coisas diferentes, e pode até ser que tenhama lguns que não gostam de donuts. Aí fica complicado né?! A gente passaria muitos dias (ou meses!) tentando vendê-los, e isso não é muito produtivo. 

<img src="https://miro.medium.com/max/1400/1*4wW4eTei-_4jjFJ99pGgIw.png" width="90%">

Mas e se eu te disser que existe uma solução para esse problema, algo que possa facilitar as nossas vendas de donuts! Podemos usar um algoritmo para segmentar, ou seja, criar grupos diferentes de personagens baseados em suas características, então teríamos um grupo de personagens que não gostam de donuts, grupos de personagens são muito fãns de Dounuts, personagens crianças (que podem gostar de Dounuts de chocolate), etc.. (por fim, podemos ter vários clusters ou grupos possíveis). 

Então que tal entendermos como funciona um dos algoritmos que pode nos ajudar a resolver esse problema e muitos outros problemas?

<img src="https://cosmonerd.com.br//uploads/2017/07/os-simpsons-fox-cosmonerd-post.jpg" width="90%">

Antes de mais nada precisamos ter em mente o que é machine learning ou aprendizado de máquina, que é uma subárea da Inteligência Artificial, em que os algoritmos recebem a habilidade de aprender com os dados sem serem explicitamente programados. O aprendizado nada mais é do que encontrar **padrões nos dados a partir de uma experiência**, e com essa experiência o modelo vai conseguir prever, classificar ou agrupar novos dados de acordo com o objetivo dele.

Esse aprendizado é dividido em 3 grandes áreas: Supervisionado, não supervisionado, por reforço. Nesse post eu só vou explicar o **aprendizado não supervisionado**, pois é o foco do artigo ;). Mas caso você queira saber mais sobre os outros aprendizados, pode acessar o meu artigo pra [Sprint do Programaria](https://www.programaria.org/carreira-em-machine-learning-como-se-preparar-para-trabalhar-na-area/) e para o [AI Girls](https://dev.to/aigirlsbr/afinal-o-que-e-machine-learning-ih5).

No aprendizado não supervisionado a máquina precisa aprender por si mesma os padrões e tendências existentes nos dados, diferente do supervisionado não temos a variável alvo. Por exemplo, queremos explorar o dataset com alguns personagens dos simpsons e com isso rodamos um algoritmo não supervisionado para entendê-los melhor. E o algoritmo nos retornou a saída abaixo com os clusters que ele formou, o que vocês vêem de semelhante entre os personagens dentro do clusters?

<img src="/img/simpsons-k-means/artigo-1.png" width="85%">

Se você olhar bem, o modelo separou os personagens pela idade (UAAAAL!!), ou seja, personagens com idades baixas formaram um grupo de pessoas mais novos, e já personagens com idades altas formam um grupo de pessoas mais velhas, portanto clusters nada mais é que grupos Bem bacana né?! 

Normalmente o aprendizado não supervisioando é utilizado nos seguintes problemas:

* Agrupamento ou clusterização: quando queremos agrupar os dados, de acordo com suas características ou descobrir grupos nos nossos dados, por exemplo agrupar os personagens pela idade (spoiler: Iremos focar neste grupo no artigo). Então cluster nada mais é que um grupo de objetos (personagens, pontos, clientes, etc..) similares.

* Associação: quando queremos descobrir regras que descrevem os nossos dados, por exemplo as pessoas que gostam do livro A, tendem a comprar o livro B.

Como o nosso objetivo é encontrar grupos dentro do desenho dos Simpsons, iremos utilizar algoritmos de clusterização.

## Mas quais as vantagens de usar cluterização? 

* Analise exploratória nos dados 

* Geração de resumo dos dados, quando a gente não conhece eles

* Detecção de outliers 

* Achar duplicações nos dados 


## E quais os diferentes tipos de algoritmos de clusterização?

<img src="https://media.giphy.com/media/xT5LMP8sIeIbxbW1Da/giphy.gif" width="85%">

### Clusterização baseada em partição: 

Constroem várias partições e depois é avaliado através de algum critério, por exemplo: "Faz sentido os clusters formados?".

Exemplo: K-Means, K-Median, Fuzzy c-means 

<img src="/img/simpsons-k-means/artigo-4.png" width="85%">


### Clusterização hierárquica: 

Cria uma hierarquia decompondo uma série de objetos usando algum critério.

Exemplo: Aglomerativo, Divisivo 

<img src="/img/simpsons-k-means/artigo-3.png" width="85%">



**Mas infelizmente (ou felizmente), hoje aprenderemos apenas o K-means!**

K-means também conhecido como Isodata ou C- Means, é o algoritmo mais conhecido e utilizado para o agrupamento de dados e apoio a outros algoritmos que possuem alto custo computacional, é o algoritmo de agrupamento mais popular e utilizado devido à sua simplicidade, eficiência e facilidade de implementação. E ele recebeu esse nome pois ele particiona os dados e K grupos diferentes (OAAAAL),então se você passar pra ele o valor de K=3, o algoritmo irá formar 3 grupos (já já eu explico o significado do "means").

Para fazer os grupos ele precisa encontrar personagens semelhantes, **mas como podemos medir o quão semelhante dois personagens são? Como trazer isso para a matemática?**

Através de cálculos de distância entre os personagens, onde os personagens mais similares estarão mais próximos, ou seja, quanto menor a distância mais similares são, quanto maior a distância entre um personagem e outro, maior são suas diferenças. Em outras palavras, a distância entre os dados (personagens) é usada para moldar os clusters ou grupos. 

Então podemos dizer que o K-Means tenta minimizar a distância intra-clusters (diminuir a distância entre os personagens dentro de um grupo) e maximizam a distância inter-clusters (precisamos aumentar a distância entre grupos diferentes). 

<img src="/img/simpsons-k-means/artigo-2.png" width="85%">

Nessa imagem vocês podem ver que a distância entre os clusters é exibido pela seta preta, e a distância intra-clusters é representado pelas setas dentro de cada um dos clusters. 

Há alguns cálculos muito populares para calcular a distância entre os dados, entre eles temos:

**Para dados binários:**

* Distância de Hamming

**Para dados categóricos:**

* Distância de Matching

**Para dados contínuos:**

* Distância euclidiana
* Correlação de Pearson
* Medida de cosseno (muito utilizado em textos)

## Como o K-Means funciona?

Vamos explorar como ele funciona por baixo dos panos?

<img src="https://media.giphy.com/media/RwLDkna2fN3fG/giphy.gif" width="85%">


1. Primeiro, precisamos definir um ‘K’, ou seja, um número de clusters (ou agrupamentos) que o algoritmo precisa fazer. 

2. Depois, é definido aleatoriamente, um centróide, ou seja, um ponto de referência para cada cluster (isso o algoritmo realiza sozinho).

3. Agora é encontrar a centróide mais próximo de cada ponto de dados, todos os pontos que estiverem mais próximos da centróide são atribuidos ao grupo. Podemos dizer que isso não resulta em bons clusters, pois os centróides foram dadas aleatórios, a princípio. 

4. Agora, a questão é: "Como podemos deixar os nossos clusters mais perfeitos?". Nós podemos mover os centróides. Na próxima etapa, cada centro de cluster  ser atualizado para ser a média dos pontos de dados em seu cluster, é daí que vem o "means" do K-means ("means" para quem não sabe significa "média" em português).

5. Precisamo calcular a distância dos pontos tudo denovo, portanto as etapas 3 e 4 são repetidas até o momento em que os centróides não mudam, aí significa que obtemos a posição ideal dos centróides. 

Bem simples né?!


## Mas atenção!!

<img src="https://media.giphy.com/media/liW10vuLjuUA8/giphy.gif" width="85%">

O k-means (assim como muitos outros algoritmos de machine learning) é uma **heurística**, e com isso não temos garantia que convergirá para um **resultado ótimo**, e o resultado pode depender dos clusters iniciais. Isso significa que este algoritmo é garantido para convergir a um resultado, mas o resultado pode ser um ótimo local (ou seja, não necessariamente o melhor possível resultado). Para resolver este problema, é comum executar todo o processo, várias vezes, com diferentes condições iniciais. 


## E como saber se ele acertou? Como avaliar oque foi gerado? 

Uma das opções é compararmos os resultados gerados com os verdadeiros resultados, se tiver disponível. Normalmente não temos esses resultados verdadeiros, então tem uma outra opção, é com base no objetivo do k-means, ou seja, vamos considerar a distância dos pontos dentro de um cluster, por exemplo: pode ser usado a análise por Silhouette, que mede o quão bem um ponto se encaixa em um cluster (se você se interessou sobre essa técnica tem um artigo bem bacana que ensina como que aplica ela, clicando [aqui](https://dev.to/giselyalves13/aprendizado-nao-supervisionado-com-k-means-106f)), além disso a média entre as distâncias pode ser usada como uma métrica de erro para o algoritmo. 


## O grande desafio de escolher um valor para o K: 

<img src="https://media.giphy.com/media/3orif2teAM5HwdVMB2/giphy.gif" width="85%">


Essencialmente escolher o número de clusters em um dataset é um problema muito frenquente, pois o valor correto de K é ambiguo porque depende muito da forma e da escala de distribuição de pontos em um dataset. Existe algumas abordagens para lidar com isso, mas uma das técnicas mais usadas é executar o clustering em diferentes valores de K  e olhando para a métrica de precisão dele, essa métrica pode ser “a distância média entre os pontos de dados e seu centróide do cluster”, que indique quão densos são nossos clusters ou até que ponto minimizamos o erro de clustering.

Outra forma é usarmos o "Método do cotovelo", que nos ajuda a definir a melhor quantidade de clusters que podem ser encontrados, mesmo sem saber a reposta antecipadamente, tem um artigo sensacional do pizza de dados sobre isso, para acessar clique [aqui](https://medium.com/pizzadedados/kmeans-e-metodo-do-cotovelo-94ded9fdf3a9). 



Bora pra prática?!!
Pessoal, como uma boa pythonlover eu mostrarei a implementação do algoritmo através da biblioteca do python chamada [Scikit-learn](https://scikit-learn.org/stable/) e irei utilizar também um CSV que eu criei com algumas informações fictícias de alguns personagens do simpsons, para acessa-lo clique [aqui](). E logo abaixo temos a tabela com os personagens e as informações que vamos utilizar.

<div align="center"><img src="/img/simpsons-k-means/artigo-5.png" width="50%" aling='center'></div>

Antes de mais nada, é necessário ler os dados, portanto utilizaremos a biblioteca Pandas para isso.
```
import pandas as pd
df = pd.read_csv('data/simpsons.csv',sep=";")
df.head()
```
Então nesse código, lemos o arquivo CSV cujo nome é "simpsons" e atribuimos os valores que estão neles em uma variável chamada "df", que virou um objeto tabular do tipo DataFrame que é exclusivo do Pandas. E com isso estamos pedindo para exibir as 5 primeiras linhas da nossa tabela com a função .head(), que por default exibe 5 linhas (se você quiser visualizar mais linhas, você passar por parâmetro a quantidade que você quer).

Se você rodar esse código, terá a seguinte saída:

<img src="/img/simpsons-k-means/artigo-7.png" width="60%">

Se você quiser entender mais sobre os dados, você pode utilizar a função . describe() do pandas, que tem um resumo estatístico (mostra a média, valor máximo e minimo, desvio padrão e os quartis dos dados). Vou falar bem por cima o que esse resumo nos passou. 

<img src="/img/simpsons-k-means/artigo-8.png" width="60%">

Como vocês podem ver, no index de "count" ele mostra a quantidade de amostra que temos em cada uma das colunas, no nosso caso ele está mostrando que temos 9 personagens, portanto temos 9 informações sobre o cabelo, o peso e a idade. Já o "mean" indica o valor médio de cada uma das colunas (importante ressaltar que essa medida de resumo é muito sensível a outliers, ou seja valores atípicos.)

```
from sklearn.cluster import KMeans
kmeans=KMeans(n_clusters=3)
kmeans.fit(df.drop('Personagem',axis=1))

```
No código acima importamos o algoritmo, e instânciamos e atribuimos a uma variável chamada "kmeans". Ah! E definimos a quantidade de cluster como 3, e treinamos o modelo com o método .fit(), precisamos passar os dados dentro dos parênteses, no meu caso eu deletei a coluna com o nome dos personagens ao passar por parâmetro.

E esses foram os clusters gerados pelo algorítmo:

<img src="/img/simpsons-k-means/artigo-6.png" width="60%">

Podemos notar que temos um cluster com os personagens mais novos, e tem um grupo de personagens mais velhos, entratanto podemos ver que o Homer ficou sozinho lá na ponta, pode ser pois ele é o único personagem que tem pouco cabelo. Mas será que a quantidade de clusters, estão corretos? Vamos utilizar o método do cotovelo para validar =).









