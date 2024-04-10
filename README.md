# Pràctica GCED-AP2 2024 · Ciutats i Camins

El projecte desenvolupat es basa en la implementació d'una aplicació pel joc d'estratègia anomenat "Ciutats i Camins".

Aquest joc es desenvolupa en un tauler rectangular dividit en cel·les que contenen recursos que s'extingeixen a mesura que s'extreuen. Els jugadors poden convertir aquests recursos en monedes que afegeixen al seu tresor. Gràcies a les monedes, poden construir ciutats i camins per competir per nous recursos i el control del territori.

# Implementació del joc de Ciutats i Camins

He mantingut originals els mòduls `drawer`, `places` i `play`. He hagut d'implementar els mòduls `player`, `board` i `game`, en aquest ordre. He decidit fer aquest ordre, ja que el mòdul `game` utilitza el `board` i aquest al `player`, llavors he pensat anar segons la complexitat i els requisits. 

## Mòdul `player`

El mòdul `player` conté la classe `Player` que emmagatzema la informació relativa a un jugador.

Aquest és el mòdul més simple. Les funcions simplement fan un `return` de paràmetres de la classe `Player` i la seva única acció incrementa el valor del seu paràmetre `_cash`.

## Mòdul `board`

El mòdul `board` conté la classe `Board` que emmagatzema la informació relativa al tauler de joc.

Considero que he de donar explicacions sobre la matriu `_nodes`. Aquesta matriu representa els nodes del tauler. En accedir al node corresponent de la matriu, trobem una tupla. 

El primer valor és una llista que s'inicialitza a:
```python
[-1, -1, -1, -1]
```
Aquesta llista mostra les IDs del "propietaris" del node. Amb propietaris em refereixo a persones que tenen una ciutat (com a màxim pot haver-hi una) o l'extrem d'un camí al node. Pot haver-hi diversos propietaris perquè si una ciutat és a un node, hi poden convergir allà fins a quatre camins de diferents jugadors. Això és deu a una de les normes respecte a la construcció de camins, la qual diu que es pot construir un camí sempre que: "Cap dels extrems de l'aresta té un camí d'un altre jugador (llevat que aquell node sigui una ciutat, perquè les ciutats promouen intercanvis)."

El segon valor s'inicialitza a:
```python
False
```
Serveix per mostra si hi ha una ciutat a aquell node o no. Aquell valor booleà es mantindrà sempre i quan no hi hagi una ciutat en aquell node. Aquest valor és útil per satisfer per exemple la norma anteriorment mencionada, que deia que només dos camins o més de diferents jugadors poden convergir en un node si hi ha una ciutat.

He tractat que les funcions i accions d'aquest mòdul siguin fàcils d'entendre mitjançant comentaris i els docstrings. Tot i això, hi ha una funció que potser és més difícil d'entendre. Parlo de la següent funció:
```python
def _coords_information(self, player: Player, coord: Coord) -> int:
        """
        Returns 1 if the coordinate belongs to the player.
        Returns 0 if belongs to no one.
        Returns -10 if there is another's player city.
        Returns -20 if there is another player's path in that coordinate.
        """
        
        id = player.get_id()
        coord_owners = self._nodes[coord[0]][coord[1]][0]
        is_city = self._nodes[coord[0]][coord[1]][1]
        
        if id in coord_owners: #4 iterations as max, constant cost
            return 1
        
        elif coord_owners == [-1,-1,-1,-1]:
            return 0
        
        elif is_city == True:
            #due to the previous conditions we know that the coordinate belongs to another player
            #consequently we don't have to put another condition to verify it
            return -10

        else:
            return -20
```
Aquesta funció és útil al mòdul `game` per verificar la legalitat de dues accions que els jugadors poden fer: construir una ciutat i construir un camí. He escollit que retornin aquests números per tal que quan s'hagi de construir un camí, només es deixi fer en cas que hi hagi una combinació de valors correctes. Aquesta combinació és la suma dels valors retornats per aquesta funció en avaluar els dos nodes del camí. D'aquesta manera, les condicions per a poder construir un camí es poden expressar en els resultats d'aquestes sumes. Aquestes combinacions són explicades al mòdul `game` a la funció encarregada de construir camins.

Quant a la construcció de la ciutat, no cal combinació, cap suma, simplement que la funció en retorni 1.

## Mòdul `game`

El mòdul `game` defineix la classe `Game` que és la que uneix el tauler de joc amb els jugadors i s'encarrega de llegir l'entrada per anar simulant el joc torn a torn. Aquest mòdul és el més complex de tots i necessitarà més aclariments. A continuació aclariré aquelles funcions o accions que ho necessitin.

### `__init__`

S'encarrega de la lectura de tot el que és necessari per iniciar el joc. Al principi vaig pensar en només llegir determinada informació si anava acompanyada d'una paraula clau. Poso un exemple: només llegiria el nombre de torns si anés precedit de la paraula clau `_number_turns`, com es mostra a l'exemple. Vaig descartar aquesta idea pel fet que ens donen la informació següent: "Pots assumir que tots els fitxers d'entrada segueixen l'estructura donada als exemples". I després ens deien: "Ara bé, no pots assumir que les dades que hi figurin siguin correctes". Llavors, com només he de verificar que les dades numèriques siguin correctes, les paraules que les precedeixen no són importants. Només cal fixar-se en l'ordre en què es van llegint les dades.

### `is_game_over`

Aquesta funció retorna True si el joc s'ha acabat, False altrament. Això ho verifica comparant el torn actual amb el nombre de torns.

### `next_turn`

Aquesta acció llegeix i executa el torn del jugador si el joc no s'ha acabat. Es divideix en dues parts:
- La primera recull els recursos de les cel·les adjacents a cadascuna de les ciutats d'un jugador fent una crida a `_get_resources`.
- La segona és l'encarregada de llegir l'acció que es vol executar i, en cas de ser legal i no tenir errors numèrics, executar-la. 

Al final incrementa en 1 el comptador dels torns.

En cas que el joc s'hagi acabat, farà un `print` del guanyador.

### `_get_resources`

Recull el recurs d'una cel·la si és possible i augmenta el `cash` del jugador en 1. Com que l'acció `next_turn` crida a aquesta funció quatre cops (una per cada cel·la que pot tenir adjacent una ciutat), s'ha de tenir en compte els casos en què la ciutat estigui als extrems del tauler. Això s'entén millor amb un exemple:
- Imaginem que tinc una ciutat al node (0,0). Segons com he definit el next_turn, també recolliria els recursos de la cel·la (-1,-1), que és a l'altre cantó del tauler (segons com python interpreta els índexs negatius). Per evitar això tenim la següent línia de codi:
```python
exists = (coord[0] >= 0 and coord[1] >= 0 and coord[0] <= size[0] and coord[1] <= size[1])
``` 
Aquesta variable `exists`, que més tard es trobarà en un `if` juntament amb una altre, evita que es recullin recursos de cel·les no adjacents a la ciutat.

És cert que podria considerar altres accions per evitar que cridi a aquestes coordenades no desitjades, però per tal de fer més comprensible el codi i per concordança amb altres parts més importants de tots els mòduls, he considerat que era el més adient.

### `_check_in_board`, `_check_turn`, `_check_your_city` i `_check_negative`

Són accions que mostren errors en cas l'usuari hagi introduït un valor erroni.