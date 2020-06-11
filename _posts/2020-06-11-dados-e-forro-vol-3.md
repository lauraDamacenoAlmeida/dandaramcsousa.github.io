---
layout: post
title: "Análise de Dados e Forró - 3"
subtitle: "Ao Vivo no Recife, Vol. 3"
date: 2020-06-11 17:19:00 -0300
background: ''
---


*Voltei, pra dizer o quanto eu te amo!* Ops, essa é a análise de Calcinha Preta. :x

*Hoje alguém me disse que você sem querer perguntou* pelo volume 3 de Análise de Dados e Forró. Por isso, cá estou com esta que é a última parte de uma carreira de sucesso. 

![a calcinha preta é nossa - belém do pará - vol.2](https://media.giphy.com/media/XbyBk5V6Ng3Jrx74BC/giphy.gif)

Depois de organizar o que gostaríamos de fazer e mexer na API do Spotify para ter um [.csv](https://drive.google.com/file/d/1ZN6iTZH3tq1MQcY1usL0s6cBE4EkBPgX/view?usp=sharing) bonito, nos falta apenas as visualizações dos dados. Porém, entretanto, todavia, ainda não disse quais perguntas vamos responder. Então vamos lá:
1. Qual é a música mais popular de Calcinha Preta? 
2. Qual é o álbum mais popular?
3. E o single?
4. Todo ano Calcinha Preta lança música?

Com essas quatro perguntas, nós conseguimos, por exemplo saber se o volume 13 é realmente o álbum mais popular, quantas músicas dele estão presentes no top hits e ainda explorar outros pontos do nosso conjunto de dados. 

Essas são as únicas perguntas que podemos fazer? Claro que não. Eu poderia descobrir qual  é o artista que mais faz feat com  Calcinha Preta ou mesmo quantas músicas explicits a banda  tem no repertório… No final, a verdade é que *é muito linda, é boazuda, Calcinha Preta nos leva a loucura*.

Começamos carregando nosso conjunto de dados e importando o Altair, uma biblioteca  Python para visualização de dados muito boa e intuitiva. Recomendo dar uma olhada na documentação e começar a usar nas suas análises.

```
import altair as alt
import pandas as pd
 
data = pd.read_csv('calcinhapreta.csv')
```

Em seguida vamos organizar o top 20 das músicas da Calcinha Preta. Assim, respondemos nossa primeira pergunta e ainda exploramos se o nossas músicas favoritas estão entre as mais populares do Spotify.

```
data_top20 = data.sort_values('popularity', ascending = False).head(20)
alt.Chart(data_top20).mark_circle(color = '#222437', opacity = 1).encode(
    y = alt.Y('name', axis = alt.Axis(title='Nome'), sort = None),
    x = alt.X('popularity', axis = alt.Axis(title = "Popularidade")),
    tooltip = ['name', 'popularity', 'album', 'album_type']
)
```
O Altair, como vocês podem ver acima, é bem intuitivo. A estrutura dele é:
**Altair.Chart(dado).marca(atributos para a marca).encode(atributos do gráfico)**. Reforço que vale a pena olhar a documentação do Altair e exemplos de uso para descobrir as possibilidades de visualizações que existem.

E nosso top20 é:

<img src="/img/forroedados/0-top20.png" width="95%">

Os velhos fãs da Calcinha Preta irão questionar: Tchau pro Seu Amor é o primeiro lugar? Calma, calcinhers, lembrem da definição de popularidade que foi dada lá no [volume 01](https://dandaramcsousa.github.io/2020/05/26/dados-e-forro-vol-1.html). Uma música nova tem muito mais chance de ser mais popular. Agora presta atenção em como Cobertor é sucesso! Presente no álbum As 20+ e está em segundo lugar, perdendo só para um hit atual. Outras músicas que são clássicas confirmam a consolidação da Calcinha Preta como uma banda de forró icônica. Mágica, por exemplo, aparece duas vezes no top 20 e vocês podem pensar que é uma *mágica* ou um erro da programadora. Bom, mas não é. Mágica é tão sucesso que as versões do álbum Vol. 12 e do As 20+ estão presentes nesse nosso top 20.

Provavelmente você olhou e procurou pelo seu top 5 e como ele se encaixa no que os nossos dados provam. E mais provavelmente ainda você reclamou dos dados. *O que fazer para aceitar* que sua opinião não é igual a da maioria? Bom, vale lembrar que esses são dados unicamente do Spotify, ou seja,  um recorte dos ouvintes da Calcinha Preta. Talvez ter os dados de outras plataformas como last.fm e deezer gerassem um resultado mais preciso. Vou deixar a indireta aqui para alguém fazer… rs

Continuamos agora em busca dos álbuns mais populares. O ‘album_type’ nos informa a categoria daquele álbum, podendo ser um álbum, um single, um EP. Como queremos apenas os álbuns mais populares, vamos restringir ao tipo ‘album’. E dessa vez vamos colocar mais um conceito do Altair: a sobreposição de gráficos. Para isso, basta atribuir cada gráfico a uma variável e depois somá-las. Sério, como não amar?

```
data_groupby = pd.DataFrame(data, columns = ['album','album_type','popularity','duration_ms'])
 
data_groupby_album_chart =  data_groupby[data_groupby['album_type'] ==  'album']
 
bars = alt.Chart(data_groupby_album_chart).mark_bar(color = '#222437').encode(
    x=alt.X('median(popularity):Q', axis = alt.Axis(title='Mediana da Popularidade')),
    y=alt.Y('album:O', sort='-x', axis = alt.Axis(title='Álbum'))
)
text = alt.Chart(data_groupby_album_chart).mark_text(dx=-15, dy=3, color='white').encode(
    x=alt.X('median(popularity):Q', axis = alt.Axis(title='Mediana da Popularidade')),
    y=alt.Y('album:O', sort='-x', axis = alt.Axis(title='Álbum')),
    text=alt.Text('median(popularity):Q')
)
bars + text
```

E aí, meus queridos fãs da banda Calcinha Preta, é o momento de glória desta que vos escreve. Este é o momento onde os humilhados são exaltados, o momento que eu olho e digo pra essa visualização que *me mate de beijo e me chame de bem*. Ou seja, é o momento onde Calcinha Preta Volume 13 é coroado como o álbum mais popular e só isso importa. 

<img src="/img/forroedados/1-albumpopular.png" width="95%">

Mudando o ‘album_type’ para ‘single’ a resposta é:

<img src="/img/forroedados/2-singlepopular.png" width="95%">

E a linha do tempo de lançamentos? Se quisermos  as informações sobre ano de lançamento, nome do que foi lançado e tipo? Então, para isto manipularemos um gráfico de pontos incluindo a variável cor com uma escala personalizada.

<img src="/img/forroedados/3-linhadotempo.png" width="95%">

Olha só, uma banda que está em constante lançamento de conteúdos desde 1996 não teria como não ser a maior banda de forró. É perceptível que nos últimos anos ela vem lançando muito mais singles. Talvez por causa da alta rotatividade que a banda passou ou por ser mais difícil organizar álbuns. São análises que vão além de um csv. 

E chegamos ao fim! Essa trilogia foi uma delícia de se fazer mas sei que ainda há muito mais que pode ser feito. Vocês sentiram falta de algo? Como por exemplo, enxergar os tooltips que estão presentes em algumas visualizações? Então, vem pro Colab onde está todo o conteúdo resumido [aqui](https://colab.research.google.com/drive/1ai1x7V8xxL6geOvBdnYcWjo3p6a11Yke?usp=sharing).

![louca por ti - belém do pará - vol.2](https://media.giphy.com/media/ZDttRtORPfh9FVc0cl/giphy.gif)

Agradeço a paciência de vocês em acompanhar semanalmente meus posts, de identificarem as músicas que eu saí colocando por todo o texto e por divulgarem para uma galera que chegou até mim através dessa trilogia. Agradeço também às PyLadies que me deram todo apoio, ao Gera que sempre leu tudo antes de ser oficialmente postado para poder aprovar e à Vanessa por ser a mente pensante que entrou comigo neste rolê do Spotify, de sempre se empolgar com qualquer texto que escrevi, que produziu a maior parte dos GIFs que estão aqui e no Colab, que fez correções ortográficas e que comigo compõe a futura produtora Forró e Bit. E como já diria Diau, *vocês são demais*.