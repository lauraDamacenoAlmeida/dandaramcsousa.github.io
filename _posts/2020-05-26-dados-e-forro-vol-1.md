---
layout: post
title: "Análise de Dados e Forró"
subtitle: "Ao Vivo em Salvador, Vol. 1"
date: 2020-05-26 18:00:00 -0300
background: ''
---
Junho está batendo na porta, eu já sinto o cheiro de São João e como mulher nascida na cidade do [Maior São João do Mundo](https://www.youtube.com/watch?v=E9-BpjIvA6s) imaginem vocês, que diante desta pandemia, *agora estou sofrendo*. Depois de uma saudável discussão no PyLadies Paraíba sobre as melhores músicas e o melhor álbum da Calcinha Preta, senti o dever social de honrar o meu lado *fã da banda Calcinha Preta que declara seu amor e vira manchete dos jornais* para provar minha opinião. E assim originou-se minha saga (não a do vaqueiro) que começo a narrar neste post e vai seguir por mais alguns.

![e o vento levou - salvador - vol. 1](https://thumbs.gfycat.com/MajesticTastyElephant.webp)

O ponto onde eu gostaria de chegar era provar que o álbum volume 13 de Calcinha Preta é o melhor. Escolhi o Spotify, a plataforma que mais frequento e crio [playlists incríveis](https://dandaramcsousa.github.io/2020/04/20/guia-playlists.html) que você deveria seguir, para essa atividade e busquei conhecer um pouco da API (já até falei como conseguir as credenciais [aqui](https://dandaramcsousa.github.io/2020/05/20/tutorial-api-spotify.html). Então, montei um roteiro para minha análise de dados que idealmente deve ser replicável para qualquer artista que está no Spotify:
1. Coletar as informações de um artista, dado um nome;
2. Coletar os ids e nomes de todos os álbuns de um artista;
3. Coletar informações de todas as músicas de um álbum;
4. Coletar informações de todas as músicas de todos os álbuns de um artista;
5. Converter tudo para um csv.

A minha *balada prime* já tinha o passo-a-passo do sucesso. Tendo informações de todas as músicas de todos os álbuns da Calcinha Preta, eu conseguiria não só mostrar qual o melhor álbum, mas também outras coisas, como as melhores músicas, a linha do tempo de lançamentos... Mas peraí! Melhor? Como melhor? O que é melhor? Como defino melhor? Apesar de forró e análise de dados serem minhas *duas paixões, dois amores*, o método da coleta precisa ser bem transparente. Pensei, pensei e vi que para cada música, o Spotify disponibiliza a popularidade da faixa. Boa! Um cálculo feito pelo número de ouvintes daquela música e que na API é definida assim:

> Popularidade da faixa: O valor estará entre 0 e 100, sendo 100 o mais popular. A popularidade é calculada e baseia-se na maior parte no número total de reproduções que a faixa teve e quão recentes foram estas reproduções. De um modo geral, as músicas que estão sendo tocadas com uma maior frequência atualmente terão uma popularidade maior do que as músicas que foram muito tocadas no passado. As faixas duplicadas (por exemplo, a mesma faixa de um single e de um álbum) são classificadas independentemente. A popularidade do artistas e dos álbuns é derivada matematicamente da popularidade das faixas. O valor da popularidade pode ficar abaixo da popularidade real em alguns dias pois o valor não é atualizado em tempo real.

Para conseguir a popularidade de uma álbum, defini que o cálculo da mediana da popularidade de todas as músicas do álbum seria ideal. A média é muito afetada por valores extremos e a mediana, não. Você também pode ler [esse post da Alura](https://www.alura.com.br/artigos/media-ou-mediana-entendendo-cada-uma) sobre e começar a usar mais da mediana na vida.

Tudo bonito, tudo massa? Agora é hora de começar a codificar. E não seria outra linguagem, senão a melhor linguagem de programação segundo o IBGEu, e que tem uma biblioteca para o Spotify com o melhor trocadilho ([Spotipy](https://spotipy.readthedocs.io/en/2.12.0/) rs). Não só da Calcinha, mas é de Python *que eu gosto e carrego no coração*. Então, fica de olho que o volume 2 já já é lançado com muito Python para chamar de seu.

![a calcinha preta é nossa - belém do pará - vol.2](https://media.giphy.com/media/kI3NVhbtJKdJaPCLS7/giphy.gif)

