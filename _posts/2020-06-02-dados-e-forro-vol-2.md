---
layout: post
title: "Análise de Dados e Forró - 2"
subtitle: "Ao Vivo em Belém do Pará, Vol. 2"
date: 2020-06-02 12:10:00 -0300
background: ''
---

*Como foi tão bom a gente se encontrar* no volume 1 e que bom que você está aqui novamente. Hoje, vamos estruturar nossa conversa anterior em código. Que pode ser feito no Google Colab ou em qualquer editor que você prefira. Porém qual outro editor teria corgis e gatinhos passeando enquanto você programa?

<img src="/img/forroedados/image01.png" width="95%">

Certo, mas como mexer nessa API, Dandara? *Ai! Me dá uma solução!* Primeiro, instalamos o [Spotipy](https://spotipy.readthedocs.io/en/2.12.0/), a biblioteca para uso da [API do Spotify](https://developer.spotify.com/documentation/web-api/reference/) com Python. Depois, definimos o que queremos de cada função para em seguida implementá-las.

``` 
!pip install spotipy
from spotipy.oauth2 import SpotifyClientCredentials
import spotipy
import sys
import csv
 
sp = spotipy.Spotify(client_credentials_manager = SpotifyClientCredentials(client_id="##", client_secret="#"))
``` 

*No começo é tudo maravilha*. Para que nosso código seja aplicável a qualquer artista vamos definir a função para obter as informações de um artista apenas pelo nome, e em seguida coletar seu id (por boas práticas, através de um get).

**get_artist(name): dado um nome, retorna um artista**

```
def get_artist(name):
    results = sp.search(name)
    items = results['tracks']['items']
    if len(items) > 0:
        return items[0]['artists'][0]
    else:
        return None
```

**get_artist_id(artists): dado um artista, retorna o id dele**

```
def get_artist_id(artist):
    return artist['id']
```

O Spotipy possui uma função que dado o id do artista, nos devolve as informações de todos os álbuns e singles que o artista tem no Spotify. 

Aqui vem um pulo do gato rapidinho: o limite default do Spotipy é de coletar 20 álbuns e Calcinha Preta possui bem mais que isso, mais precisamente 46 álbuns, assim atribuímos esse valor como limit na função. Poderia ter colocado um limite maior? Sim, claro, mas aí eu deveria fazer um condicional para coletar apenas os álbuns do artista que passamos o id. Ao colocar um valor maior que a quantidade correta de álbuns do artista no spotify, a função vai começar a retornar álbuns de artistas relacionados e não do artista que você quer. 

**get_artist_albums_id_names(id): dado o id do artista, retorna os ids e nomes de todos os álbuns daquele artista disponíveis no Brasil**

```
def get_artist_albums_id_names(id):
  albums = sp.artist_albums(id, country = 'BR', limit=46)
  albums_id_name = {}
  for i in range(len(albums['items'])):
    id = albums['items'][i]['id']
    name = albums['items'][i]['name']
    albums_id_name[id] = name
 
  return albums_id_name
```

Boa! Temos os ids e os nomes de todos os álbuns. Agora,  já podemos dizer que *a Calcinha Preta é nossa, é nossa, é nossa*? Calma, jovem… Vamos agora conseguir todas as informações das músicas dos álbuns. Primeiro uma função para coletar todas as músicas de um álbum só.

A função sp.album_tracks(album_id) retorna as faixas do álbum, porém não tem todas as informações que estamos querendo colher. Assim, usaremos essa função para ter o id de cada faixa e passaremos esse valor na função sp.track(id_track).


**get_album_songs(album_id, album_name): dado o id e nome do álbum, retorna um dicionário com as informações das músicas do álbum (álbum, tipo do álbum, número da música no álbum, id, nome, popularidade, se a faixa é explícita, duração em milissegundos, data de lançamento e artistas envolvidos)**

```
def get_album_songs(album_id, album_name):
  spotify_album = {}
 
  tracks = sp.album_tracks(album_id)
  
  for n in range(len(tracks['items'])):
    id_track = tracks['items'][n]['id']
    track = sp.track(id_track)
    spotify_album[id_track] = {}
    
    spotify_album[id_track]['album'] = album_name
    spotify_album[id_track]['album_type'] = track['album']['album_type']
    spotify_album[id_track]['track_number'] = track['track_number']
    spotify_album[id_track]['id_track'] = track['id']
    spotify_album[id_track]['name'] = track['name']
    spotify_album[id_track]['popularity'] = track['popularity']
    spotify_album[id_track]['explicit'] = track['explicit']
    spotify_album[id_track]['duration_ms'] = track['duration_ms']
    spotify_album[id_track]['release_date'] = track['album']['release_date']
 
    artists_track = track['artists']
    spotify_album[id_track]['artists'] = []
    for artist in artists_track:
      spotify_album[id_track]['artists'].append(artist['name'])
  return spotify_album

```
O próximo passo então é fazer uma função que chame get_album_songs(album_id, album_name) para todos os álbuns. Criamos aqui um condicional para resolver casos de álbuns duplicados.

**get_all_albums_songs(albums_ids_names): dado um dicionário com ids e nomes de álbuns, retorna todas as músicas de todos os albuns de um artista provenientes da função get_album_songs(album_id, album_name)**

```
def get_all_albums_songs(albums_ids_names):
  spotify_albums = []
  albums_names = []
  for id, name in albums_ids_names.items():
    if name not in albums_names:
      albums_names.append(name)
      album_songs = get_album_songs(id,name) 
    for item in album_songs.items():
      spotify_albums.append(item[1]) 
  return spotify_albums
```

Foi *bom, bom, bom, bom, bom, bom, bailar* até aqui. Então vamos colocar tudo isso em um csv?

**convert_to_csv(filepath, name): converte para um csv os dados**

```
def convert_to_csv(filepath, name):
  keys = filepath[0].keys()
  print(keys)
  csv_name = ''+ name + '.csv'
  with open(csv_name, 'w') as output_file:
    dict_writer = csv.DictWriter(output_file, keys)
    dict_writer.writeheader()
    dict_writer.writerows(filepath)
  return
```

E, por fim, para rodar vamos fazer um script rapidinho:

```
name = "Calcinha Preta"
artist = get_artist(name)    
if artist:
  artist_id = get_artist_id(artist)
  albums_id_names = get_artist_albums_id_names(artist_id)
  all_albums = get_all_albums_songs(albums_id_names)
  convert_to_csv(all_albums, 'calcinhapreta')   
else:
  logger.error("Can't find artist: %s", artist)
```

O resultado em csv você pode encontrar [aqui](https://drive.google.com/file/d/1ZN6iTZH3tq1MQcY1usL0s6cBE4EkBPgX/view?usp=sharing).

Eeeei! *Não, não me deixe agora porque* ainda não acabou! O volume 3 vem aí com todo o poder que só o Altair pode nos proporcionar para sanar a grande dúvida que impulsiona a humanidade: qual o álbum mais popular da Calcinha Preta?

![hoje a noite - belém do pará - vol.2](https://media.giphy.com/media/MEw48X9bUCPgFT0OLE/giphy.gif)