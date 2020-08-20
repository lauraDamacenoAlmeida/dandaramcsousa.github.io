---
layout: post
title: "Simpsons ensinando K-means"
subtitle: "Vamos aprender e entender o algoritmo k-means com os personagens favoritos de todos os tempos?"
date: 2020-11-05 19:00:00 -0300
background: '/img/simpsons-k-means/1.png'
---
Os simpsons são a família mais famosa e mais conhecida de todos os tempos, com personagens super caricatos, engraçados e cheio de alegria e que tal entendermos como funciona um dos algoritmos mais famosos de machine learning com eles?

<img src="https://cosmonerd.com.br//uploads/2017/07/os-simpsons-fox-cosmonerd-post.jpg" width="85%">

Antes de mais nada precisamos ter em mente o que é machine learning ou aprendizado de máquina, é uma subárea da Inteligência Artificial, em que os algoritmos recebem a habilidade de aprender com os dados sem serem explicitamente programados. O aprendizado nada mais é do que encontrar padrões nos dados a partir de uma experiência, e com essa experiência o modelo vai conseguir prever, classificar ou agrupar novos dados de acordo com o objetivo dele.

Esse aprendizado é dividido em 3 grandes áreas: Supervisionado, não supervisionado, por reforço. Nesse post eu só vou explicar o aprendizado não supervisionado, pois é o foco do artigo ;). Mas caso você queira saber mais sobre os outros aprendizados, pode acessar o meu artigo pra [Sprint do Programaria](https://www.programaria.org/carreira-em-machine-learning-como-se-preparar-para-trabalhar-na-area/) e para o [AI Girls](https://dev.to/aigirlsbr/afinal-o-que-e-machine-learning-ih5).

O aprendizado não supervisionado a máquina precisa aprender por si mesma os padrões e tendências existentes nos dados, diferente do supervisionado não temos a variável alvo. Por exemplo, queremos explorar o dataset com alguns personagens dos simpsons e com isso rodamos um algoritmo não supervisionado para entendê-los melhor.
Os problemas que envolvem esse aprendizado é:

* Agrupamento ou clusterização: quando queremos agrupar os dados, de acordo com suas características ou descobrir grupos nos nossos dados, por exemplo agrupar os personagens pela idade. (spoiler: Iremos focar neste grupo no artigo)
* Associação: quando queremos descobrir regras que descrevem os nossos dados, por exemplo as pessoas que gostam do livro A, tendem a comprar o livro B.


## Mas Laura, porque usar cluterização? 

* Analise exploratória nos dados 

* Geração de resumo dos dados, quando a gente não conhece eles

* Detecção de outliers 

* Achar duplicações nos dados 

* Passo de pré processamento (para predição, mineiração de dados) 



## Diferentes tipos de algoritmos de clusterização: 

### Clusterização baseada em partição: 

Relativamente eficiente e é usado quando temos dados de tamanho grande ou médio 

Exemplo: K-Means, K-Median, Fuzzy c-means 

### Clusterização hierarquica: 

Produz grupos de árvores. É bem intuitivo e geralmente é bom de se utilizar quando temos datasets pequenos. 

Exemplo: Aglomerativo, Divisivo 


### Clusterização baseada na densidade: 

Produz clusters de forma arbitrária. Eles são bons quando lidam com aglomeração espacial, ou quando há ruido no conjunto de dados. 

Exemplo: DBSCAN 

**Mas infelizmente (ou felizmente), hoje aprenderemos apenas o K-means!**

K-means também conhecido como Isodata ou C- Means, é o algoritmo mais conhecido e utilizado para o agrupamento de dados e apoio a outros algoritmos que possuem alto custo computacional, é o algoritmo de agrupamento mais popular e utilizado devido à sua simplicidade, eficiência e facilidade de implementação.
Cada grupo é representado por um centroide, definido pela média dos dados pertencentes ao grupo, e o objetivo é particionar os dados em K grupos disjuntos, onde cada dado é ajustado ao grupo cujo o centroide é o mais próximo.

**Mas como podemos medir o quão semelhante dois personagens são?**

Através da distância entre os personagens, onde os personagens mais similares estarão mais próximos, ou seja, quanto menor a distância mais similares são, quanto maior a distância entre um personagem e outro, maior são suas diferenças. Em outras palavras, a distância entre os dados (personagens) é usada para moldar os clusters ou grupos. Então podemos dizer que o K-Means tenta minimizar a distancia intra-clusters e maximizam a distancia inter-clusters. 
Neste caso iremos usar a distância Euclidiana para ver a distância entre os dados, mas existem outros métodos para calcular a distância (como por exemplo: método de cossenos, correlação de pearson).



## Começando com K-Means: 

1. determino o  numero de clusters (K). O conceito chave é que o K-means escolhe aleatoriamente o ponto(centróides) de cada cluster. Determinar o numero de K, é bem mais complicado. 

Existem 2 abordagens para escolhermos a centróide: 

Podemos escolher randomicamente N posições da centroide, fora do conjunto de dados. 

Podemos  escolher randomicamente N posições da controide. 

2. Após a etapa de inicialização, que estava definindo o centroide de cada cluster, temos que atribuir cada cliente para o centro mais proximo. Para isso precisamos calcular a distancia de cada ponto de dados das centroides. 

3. O principal objetivo do K-means é minimizar a distância dos pontos de dados em relação as centroides e maximizar a distancia de outros centroides de cluster. 

Então nesta etapa temos que encontrar a centroide mais proxima de cada ponto de dados. Todos os pontos que tiverem mais proximos da centroide são atribuidos ao grupo. 

Em outras palavras, os pontos de dados que tiverem menores distancia da centroide pertencerao ao grupo. Podemos dizer que isso não resulta em bons clusters, pq as centroides foram dadas aleatoriamentes. 

4. Agora, a questão é: "Como podemos transformá-lo em melhores clusters, com menos erros? " 

Ok, nos movemos centróides. Na próxima etapa, cada centro de cluster 

ser atualizado para ser a média dos pontos de dados em seu cluster. De fato, cada centímetro se move de acordo com seus membros de cluster. Em outras palavras, o centróide de cada um dos 3 clusters se torna a nova média. 

5. Precisamo calcular a distancia dos pontos tudo denovo. E assim por diante. 

Contudo sendo uma heuristica não temos garantia que convergirá para um resultado otimo, e o resultado pode depender dos clusters iniciais 

Isso significa que este algoritmo é garantido para convergir a um resultado, mas o resultado pode ser um ótimo local (ou seja, não necessariamente o melhor possível resultado). Para resolver este problema, é comum executar todo o processo, várias vezes, com diferentes condições iniciais. 

Isso significa que, com centróides de partida randomizados, isso pode dar um resultado melhor. 

E como o algoritmo costuma ser muito rápido, não seria um problema executá-lo várias vezes. 

## E como saber se ele acertou? Como avaliar oque foi gerado? 

Uma das opções é compararmos os resultados gerados com os verdadeiros resultados, se tiver disponivel. Normalmente não temos esses resultados verdadeiros, entõa tem uma outra opção, é com base no objetivo do k-means, que é a distancia dos pontos dentro de um cluster. Além disso a media entre as distancias pode ser usadas como uma métrica de erro para o algoritmo. 


## O grande desafio de escolher um valor para o K: 

Essencialmente escolher o numero de clusters em um dataset é um problema muito frenquente. O valor correto de K é ambiguo pois depende muito da forma e da escala de distribuição de pontos em um dataset. Existe algumas abordagens para lidar com isso, mas uma das tecnicas mais usadas é executar o clustering em diferentes valores de K  e olhando para a metrica de precisão dele. Essa métrica pode ser “a distancia media entre os pontos de dados e seu centroide do cluster”, que indique quão densos são nossos clusters ou até que ponto minimizamos o erro de clustering.

Mas o problema é que com o aumento do número de clusters, a distância dos centróides para pontos de dados será sempre reduzir. Isso significa que aumentar K sempre diminuirá o "erro". 
 

Então, o valor da métrica como uma função de K é plotado e o "ponto de cotovelo" é determinado, onde a taxa de diminuição muda acentuadamente. É o K certo para clustering. Esse método é chamado de método do cotovelo. 

## Recapitular: 

k-Means é um cluster baseado em particionamento, que é: 

a) Relativamente eficiente em conjuntos de dados de médio e grande porte; 

b) Produz aglomerados semelhantes a esferas, porque os aglomerados são formados em torno dos centróides; 

c) Sua desvantagem é que devemos pré-especificar o número de clusters, e isso não é uma tarefa fácil. 


Bora pra prática?!!
Pessoal como uma boa pythonlover eu mostrarei o algoritmo através da biblioteca do python, chamada Sckt-learn.



