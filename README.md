# README del Programa: Ciutats i Camins

## Introducció
Aquest README proporciona una visió general de la funcionalitat i l'ús de la meva implementació del joc d'estratègia "Ciutats i Camins", desenvolupat com a part de la primera pràctica d'Algorísmia i Programació 2. Aquest joc està lleugerament basat en el popular joc de taula Catan™️.

## Taula de continguts
0. [La pràctica](#la-pràctica)
1. [Visió General](#visió-general)
2. [Instal·lació](#instal·lació)
3. [Ús](#ús)
4. [Característiques](#característiques)
5. [Jocs de proves](#jocs-de-proves)
6. [Decisions de disseny](#decisions-de-disseny)


## La pràctica
Aquest programa s'ha fet seguint les instruccions i plantilles proporcionades pel repositori
de GitHub de la [pràctica de Camins i ciutats](https://github.com/jordi-petit/ap2-camins-i-ciutats-2024?tab=readme-ov-file)


## Visió General
Aquesta aplicació proporciona la meva implementació per a jugar al joc d'estratègia "Ciutats i Camins", que es desenvolupa en un tauler rectangular dividit en cel·les que contenen recursos que s'extingeixen a mesura que s'extreuen. Els jugadors poden convertir aquests recursos en monedes que afegeixen al seu tresor. Gràcies a les monedes, poden construir ciutats i camins per competir per nous recursos i el control del territori.


## Instal·lació
### Requisits previs
Cal tenir instal·lats prèviament els paquets de python "pygame" i "yogi"
```bash
pip install pygame
pip install yogi
```
### Abans de començar
1. Guarda la carpeta al teu ordinador local
2. Navega fins el directori on es troben tots els arxius i assegura`t que hi son tots


## Ús
Per començar a jugar a camins i ciutats, segueix aquests passos:
1. Navega fins al directori on es troba l'aplicació.
2. Executa la següent comanda i ves escrivint l'entrada desitjada al terminal
```bash
python3 play.py
```

> Si vols que la partida es jugui segons la partida ja escrita en un joc de proves (ex: proves.inp)
```bash
python3 play.py < proves.inp
```


## Característiques

### Arquitectura de l'aplicació 
L'arquitectura que segueix l'aplicació és l'explicada en l'enunciat de la pràctica. El programa consta de diferents mòduls que s'utilitzen entre
ells seguint la següent estructura:
![images/images1.png](https://github.com/jordi-petit/ap2-camins-i-ciutats-2024/raw/main/images/packages.png)

Per usar el programa tan sols caldrà executar al play.py


### Requisits de l'entrada (Com jugar)
Com s'especifica a les instruccions de la pràctica, tots els fitxers d'entrada seguiran l'estructura donada als exemples. Això inclou l'ordre de les instruccions a l'hora d'iniciar una partida.
> Aquí es veu un exemple correcte de l'ordre i les instruccions d'un [estat inicial](https://github.com/jordi-petit/ap2-camins-i-ciutats-2024?tab=readme-ov-file)


### Errors per a Entrada Errònia

En cas que es defineixi una partida amb valors erronis, el programa retorna els següents errors:

- **Nombre de Torns Negatiu:**
  Si el nombre de torns és menor o igual a zero, es retorna:
  ```python
  raise ValueError('El nombre màxim de ciutats ha de ser com a mínim 1')
    ```
- **Nombre màxim de ciutats negatiu o nul:**
  Si el nombre de torns és menor o igual a zero, es retorna:
  ```python
  raise ValueError('És necessari un nombre de torns positiu per començar la partida...')
    ```
- **Nombre de jugadors negatiu o nul:**
  Si el nombre de jugadors és menor o igual a zero, es retorna:
  ```python
  raise ValueError('No es pot jugar amb un nombre de jugadors menor o igual a 0!')
    ```
- **Jugador que realitza l'acció quan no és el seu torn**
  Si és el torn d'un jugador, però realitza l'acció un altre, també estem obtenint una entrada errònia:
  ```python
    raise ValueError("No és el torn d'aquest jugador")
    ```
- **Jugador que realitza l'acció quan no és el seu torn**
  Si és el torn d'un jugador, però realitza l'acció un altre, també estem obtenint una entrada errònia:
  ```python
    raise ValueError("Acció no existent! Fi de la partida")
    ```



## Jocs de Proves
Dins del repositori, trobaràs una carpeta anomenada `jocs_de_proves` que conté diversos jocs de proves per verificar el comportament del programa en diferents escenaris. Aquests jocs de proves ajuden a provar característiques concretes del programa. A continuació una llista i què posen a prova cadascun.

### Partides Totalment Vàlides
- **Test-1.1.inp:** joc de proves original de l'exemple.
- **Test-1.2.inp:** Partida "petita".
- **Test-1.3.inp:** Tauler allargat, construir on s'ha destruït.

### Partides amb Moviments Invàlids
- **Test-2.1.inp:** 
  - Fer camí diagonal.
  - Construir ciutat sense suficients diners.
  - Fer camí amb més de mida 1.
  - Fer camí fora del tauler.
- **Test-2.2.inp:**
  - Ciutat fora del tauler.
  - Ciutat no adjacent a un camí propi.
  - Camí no adjacent a una ciutat.

### Errors d'Entrada
- **Test-3.1.inp:** Nombre de torns no vàlid.
- **Test-3.2.inp:** Nombre màxim de ciutats no vàlid.
- **Test-3.3.inp:** Nombre de jugadors no vàlid.

Aquests jocs de proves són útils per garantir el bon funcionament del programa en una varietat d'escenaris i per detectar possibles errors d'entrada i moviments invàlids.


## Decisions de disseny
En aquest apartat es fa una d escripció de com les decisions de disseny en cada mòdul preses durant el desenvolupament del programa
es veuen reflectides en el funcionament d'aquest.

Com a norma general s'ha decidit que les comprovacions es faran majoritàriament al mòdul game, i que els sub-mòduls i les seves accions públiques es limitaran a fer el que indiquen els seus noms.

### Places
A l'hora de definir els camins, aquests tenen les coordenades dels seus dos nodes.
És a dir, van des de (0,0) superior esquerre, fins a (n. files, n.columnes) inferior dret

- **Ciutat**
Una ciutat vé definida per les coordenades del node que té just a baix a la dreta

### Player
```python
def update_cash(self, value: int) -> None:
```
 - Aquesta funció suma el valor indicat, és a dir, és el mòdul game qui s'encarrega de cridar aquesta funció amb paràmetres negatius.

### Board
En aquest apartat hi ha la decisió de disseny més important.
A l'hora d'emmagatzemar una matriu amb els recursos de la ciutat, s'emmagatzema el mateix tauler però recobert per un marc de 0's.

![imatge2](images2.png.jpg)

Això permet que, quan s'han de recollir recursos (obligatori en cada torn), no sigui necessari comprovar si ens trobem en una ciutat d'un extrem del tauler per veure quines cel·les hem de visitar.

A més, les coordenades d'una ciutat en la matriu de recursos seran les mateixes que les del tauler.

Simplement sempre es visitaran les 4 cel·les adjacents, i en cas de trobar-se en un extrem es visitaran cel·les amb 0 recursos, que serà com si no hi hagués res.

> Per tal de fer això s'utilitza una funció `inserir_matriu` que insereix de forma centrada la matriu de recursos donada per l'entrada en una matriu de 0's 2 files i 2 columnes més gran 

Això es veu reflectit en diverses funcions del mòdul:
- **`get_size`** Com sabem, la redundància és innecessària, no cal guardar la mida d'una matriu quan ja tenim la matriu. Retorna la mida de la matriu restant 2 a les files i columnes (recordem que estem usant una matriu més gran que el tauler).
- **`get_resources`** Retorna els recursos de la casella següent a les coordenades demanades.

Això es deu a que el mòdul drawer està preparat per dibuixar un taulell que es guardi de forma estàndard.

Per tal de fer que el drawer accedeixi a les posicions correctes, cal fer aquesta petita modificació a la funció.

- `_cities`, `_paths`,`get_cities`i `get_paths` 
Tot i que la implementació més eficient seria guardar les dades en diccionaris (temps constant per fer comprovacions i eliminar), l'única forma que tenim d'accedir tant als camins com ciutats des d'altres mòduls és a través de les funcions públiques.
Aquestes, tenen definit a la capçalera que retornen una llista de tuples. Per tant el més eficient donades aquestes condicions és emmagatzemar-ho tal i com es retornarà


### Game
En aquest mòdul, la decisió principal de disseny ha sigut encapsular tant com sigui possible les funcions, per tal d'assegurar el funcionament del programa. A més, si algun dia es canvia alguna part concreta d'algun mòdul, és molt més fàcil adaptar el programa per millorar-ne l'eficiència editant funcions concretes.

- **Classe `_Rules`** Aquesta classe privada que serà un atribut privat de Game emmagatzema les normes bàsiques de la partida com els preus, diners inicials i ciutats màximes.

- **Recollida de recusos** Gràcies a les decisisons preses a Board, recollir recursos és només visitar 4 cel·les i recollir recursos si n'hi ha, indiferentment d'on es trobi la ciutat.

- **Crear camins i ciutats `_create_path` i `_create_city`** Quan es vol afegir un camí o ciutat al mapa, caldrà mirar dues coses:
1. Que sigui possible construir (existeixi al tauler, etc)
2. Que sigui legal construir (segons les normes del joc)

Per tant, aquesta estructura és la que ha seguit l'encapsulament de funcions

### Extra: `Drawer`
S'ha afegit un indicador a sota el nom de cada jugador que indica el nombre de monedes que té actualment

