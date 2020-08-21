---
layout: post
title: "Aprendendo k-means com os simpsons [Não está pronto ainda!]"
subtitle: "Vamos aprender e entender o algoritmo k-means com os personagens favoritos de todos os tempos?"
date: 2020-09-19 19:00:00 -0300
background: '/img/simpsons-k-means/1.jpg'
---


Imagine-se na seguinte situação, o desenho dos simpsons tem milhares(ou seria centenas?) de personagens e você precisa vender donuts para esses personagens, parece simples né?! Mas e se eu te disser que cada um deles tem gostos diferentes, cada um deles gostam de coisas diferentes, e pode até ser que tenhama lguns que não gostam de donuts. Aí fica complicado né?! A gente passaria muitos dias (ou meses!) tentando vendê-los, e isso não é muito produtivo. 

<img src="https://i.pinimg.com/originals/f4/80/75/f480751f597dcd9d812e169baa81f7eb.jpg" width="90%">

Mas e se eu te disser que existe uma solução para esse problema, algo que possa facilitar as nossas vendas de donuts! Podemos usar um algoritmo para segmentar, ou seja, criar grupos diferentes de personagens baseados em suas caracteristicas, então teríamos um grupo de personagens que não gostam de donuts, grupos de personagens são muito fãns de Dounuts, personagens crianças (que podem gostar de Dounuts de chocolate), etc.. (por fim, podemos ter vários clusters ou grupos possíveis). 

Então que tal entendermos como funciona um dos algoritmos que pode nos ajudar a resolver esse problema e muitos outros problemas?

<img src="https://cosmonerd.com.br//uploads/2017/07/os-simpsons-fox-cosmonerd-post.jpg" width="90%">

Antes de mais nada precisamos ter em mente o que é machine learning ou aprendizado de máquina, que é uma subárea da Inteligência Artificial, em que os algoritmos recebem a habilidade de aprender com os dados sem serem explicitamente programados. O aprendizado nada mais é do que encontrar **padrões nos dados a partir de uma experiência**, e com essa experiência o modelo vai conseguir prever, classificar ou agrupar novos dados de acordo com o objetivo dele.

Esse aprendizado é dividido em 3 grandes áreas: Supervisionado, não supervisionado, por reforço. Nesse post eu só vou explicar o **aprendizado não supervisionado**, pois é o foco do artigo ;). Mas caso você queira saber mais sobre os outros aprendizados, pode acessar o meu artigo pra [Sprint do Programaria](https://www.programaria.org/carreira-em-machine-learning-como-se-preparar-para-trabalhar-na-area/) e para o [AI Girls](https://dev.to/aigirlsbr/afinal-o-que-e-machine-learning-ih5).

No aprendizado não supervisionado a máquina precisa aprender por si mesma os padrões e tendências existentes nos dados, diferente do supervisionado não temos a variável alvo. Por exemplo, queremos explorar o dataset com alguns personagens dos simpsons e com isso rodamos um algoritmo não supervisionado para entendê-los melhor. E o algoritmo nos retornou a saída abaixo com os clusters que ele formou, o que vocês vêem de semelhante entre os personagens dentro do clusters?

<img src="/img/simpsons-k-means/artigo-1.png" width="85%">

Se você olhar bem, o modelo separou os personagens pela idade (UAAAAL!!), ou seja, personagens com idades baixas formaram um grupo de pessoas mais novos, e já personagens com idades altas formam um grupo de pessoas mais velhas, portanto clusters nada mais é que grupos Bem bacana né?! 

Normalmente o aprendizado não supervisioando é utilizado nos seguintes problemas:

* Agrupamento ou clusterização: quando queremos agrupar os dados, de acordo com suas características ou descobrir grupos nos nossos dados, por exemplo agrupar os personagens pela idade. (spoiler: Iremos focar neste grupo no artigo)

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

### Clusterização hierárquica: 

Cria uma hierarquia decompondo uma série de objetos usando algum critério.

Exemplo: Aglomerativo, Divisivo 


**Mas infelizmente (ou felizmente), hoje aprenderemos apenas o K-means!**

K-means também conhecido como Isodata ou C- Means, é o algoritmo mais conhecido e utilizado para o agrupamento de dados e apoio a outros algoritmos que possuem alto custo computacional, é o algoritmo de agrupamento mais popular e utilizado devido à sua simplicidade, eficiência e facilidade de implementação. E ele recebeu esse nome pois ele particiona os dados e K grupos diferentes (OAAAAL), já já eu explico o significado do "means".

Para fazer os grupos ele precisa encontrar personagens semelhantes, **mas como podemos medir o quão semelhante dois personagens são? Como trazer isso para a matemática?**

Através de cálculos de distância entre os personagens, onde os personagens mais similares estarão mais próximos, ou seja, quanto menor a distância mais similares são, quanto maior a distância entre um personagem e outro, maior são suas diferenças. Em outras palavras, a distância entre os dados (personagens) é usada para moldar os clusters ou grupos. 

Então podemos dizer que o K-Means tenta minimizar a distancia intra-clusters (ou seja, diminuir a distância entre os personagens dentro de um grupo) e maximizam a distância inter-clusters (ou seja, precisamos aumentar a distância entre grupos diferentes). 

<img src="/img/simpsons-k-means/artigo-2.png" width="85%">

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

2. Depois, é definido aleatoriamente, um centroide/centro para cada cluster (isso o algoritmo realiza sozinho).

3. Como principal objetivo do K-means é minimizar a distância dos pontos de dados em relação as centroides e maximizar a distancia de outros centroides de cluster. 

Então nesta etapa temos que encontrar a centróide/centro mais próximo de cada ponto de dados, todos os pontos que estiverem mais proximos da centroide são atribuidos ao grupo. Podemos dizer que isso não resulta em bons clusters, pq as centroides foram dadas aleatórios, a princípio. 

4. Agora, a questão é: "Como podemos transformá-lo em melhores clusters, com menos erros?" 

Ok, nós movemos os centróides. Na próxima etapa, cada centro de cluster  ser atualizado para ser a média dos pontos de dados em seu cluster, é daí que vem o "means" do K-means ("means" para quem não sabe significa "média" em português).

5. Precisamo calcular a distância dos pontos tudo denovo. E as etapas 3 e 4 são repetidas até o momento em que os centróides não mudam, aí significa que obtermos a posição ideal dos centróides. 

Bem simples né?!


Contudo sendo uma heurística não temos garantia que convergirá para um resultado otimo, e o resultado pode depender dos clusters iniciais. Isso significa que este algoritmo é garantido para convergir a um resultado, mas o resultado pode ser um ótimo local (ou seja, não necessariamente o melhor possível resultado). Para resolver este problema, é comum executar todo o processo, várias vezes, com diferentes condições iniciais. 

Isso significa que, com centróides de partida randomizados, isso pode dar um resultado melhor. 

E como o algoritmo costuma ser muito rápido, não seria um problema executá-lo várias vezes. 

## E como saber se ele acertou? Como avaliar oque foi gerado? 

Uma das opções é compararmos os resultados gerados com os verdadeiros resultados, se tiver disponivel. Normalmente não temos esses resultados verdadeiros, entõa tem uma outra opção, é com base no objetivo do k-means, que é a distancia dos pontos dentro de um cluster. Além disso a media entre as distancias pode ser usadas como uma métrica de erro para o algoritmo. 


## O grande desafio de escolher um valor para o K: 

Essencialmente escolher o número de clusters em um dataset é um problema muito frenquente. O valor correto de K é ambiguo pois depende muito da forma e da escala de distribuição de pontos em um dataset. Existe algumas abordagens para lidar com isso, mas uma das tecnicas mais usadas é executar o clustering em diferentes valores de K  e olhando para a metrica de precisão dele. Essa métrica pode ser “a distância média entre os pontos de dados e seu centróide do cluster”, que indique quão densos são nossos clusters ou até que ponto minimizamos o erro de clustering.

Mas o problema é que com o aumento do número de clusters, a distância dos centróides para pontos de dados será sempre reduzir. Isso significa que aumentar K sempre diminuirá o "erro". 
Então, o valor da métrica como uma função de K é plotado e o "ponto de cotovelo" é determinado, onde a taxa de diminuição muda acentuadamente. É o K certo para clustering. Esse método é chamado de método do cotovelo e tem um artigo sensacional do pizza de dados sobre isso, para acessar clique [aqui](https://medium.com/pizzadedados/kmeans-e-metodo-do-cotovelo-94ded9fdf3a9). 

## Recapitular: 

k-Means é um cluster baseado em particionamento, que é: 

a) Relativamente eficiente em conjuntos de dados de médio e grande porte; 

b) Produz aglomerados semelhantes a esferas, porque os aglomerados são formados em torno dos centróides; 

c) Sua desvantagem é que devemos pré-especificar o número de clusters, e isso não é uma tarefa fácil. 


Bora pra prática?!!
Pessoal como uma boa pythonlover eu mostrarei o algoritmo através da biblioteca do python, chamada Sckt-learn.



