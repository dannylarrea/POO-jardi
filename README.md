## El Jardí

L'exercici consisteix en realitzar una simulació d'un jardí amb diverses
espècies de plantes:

```
       O                                
      O|                                
      || O                              
   O  || |                              
  O|  || |                              
  ||O || |                              
  ||| || |                       *                    O                             
  ||| || |                       :*                   |                             
  ||| || |                       ::  *                |                   *         
  |||.||.|   ..O ..            ..::  : *              |                   :         
________________________________________    ________________________________________
El jardí després d'una estona               El mateix jardí poc després de començar

Les plantes de l'esquerra són Altibus, les de la dreta són Declinus i els punts són llavors.
```

Per fer el programa crearem les següents classes amb els mètodes i atributs
especificats. Cal afegir els modificadors adequats (*public*, *protected*,
*private*, *abstract*, *static*...) així com les variables o mètodes
addicionals que es considerin convenients.

### Classe Planta

#### Atributs

* `esViva`: un booleà que indicarà si la planta està viva o no.

* `altura`: un enter que guardarà l'alçada de la planta.

#### Mètodes

* `char getChar(int nivell)`: serveix per construir el dibuix de la planta.
Donat un enter el mètode ens retornarà el caràcter de la planta en aquest
nivell d'alçada (per exemple, retornarà un caràcter per la tija, un caràcter
diferent per la part superior i un espai en blanc si el nivell està per
sobre de l'alçada de la planta).

   Cada tipus de planta implementarà el seu propi mètode *getChar*.

* `Planta creix()`: aquest mètode es crida cada vegada que la planta té
l'oportunitat de créixer. Cada tipus de planta implementarà el seu model
de creixement, però aquí es donarà una implementació bàsica que podran
reciclar les diverses espècies de plantes.

   El comportament que es definirà aquí és el següent: mentre la planta
no arribi a tenir una alçada de 10, la seva alçada augmentarà en 1. Si
la planta arriba a tenir una alçada de 10, la planta mor (cosa que
s'indica posant la seva variable *esViva* a *false*).

   A més, aquest mètode permet retornar una *Planta* que servirà per dur
a terme la seva reproducció. En la implementació per defecte, però,
es retornarà sempre *null*.

* `int escampaLlavor()`: quan una planta deixa anar una llavor, es cridarà
aquest mètode per saber a on la tira. Aquest mètode retornarà un enter que
indicarà en quina posició respecte a la planta es vol deixar la llavor.
Per exemple, retornar 1 significa que la llavor es deixarà a la següent
posició a la dreta, mentre que retornar -2 significarà dues posicions
més cap a l'esquerra.

   En aquesta implementació genèrica, es retornarà un nombre aleatori d'entre
 els següents: -2, -1, 1 i 2.

* `int getAltura()`: retorna l'alçada de la planta.

* `boolean esViva()`: retorna *true* si la planta està viva i *false*
en cas contrari.

### Classe Llavor

La classe *Llavor* representa un tipus especial de planta que produeix
una planta d'algun altre tipus al cap d'un temps.

#### Atributs

* `Planta planta`: guarda la planta que produirà la llavor quan germini
(al cap d'un temps).

* `int temps`: guarda la quantitat de temps que ha passat des que la
llavor s'ha creat.

#### Mètodes

* Constructor: l'únic constructor d'aquesta classe rep una planta i la
guarda a l'atribut *planta*. En cas que la planta passada, però, sigui
de tipus *Llavor*, llançarà una *IllegalArgumentException*, ja que no
pot ser que una llavor produeixi una altra llavor al germinar.

* `Planta creix()`: les llavors no creixen com la resta de les plantes.
Aquest mètode augmentarà el temps en 1, i si arriba a 5, retornarà l'atribut
 *planta*. En cas contrari, retornarà *null*.

* `char getChar(int nivell)`: aquest mètode retornarà sempre un espai en
blanc, excepte a l'alçada 0, en que retornarà el caràcter "." (punt).

### Classe Altibus

L'*Altibus* és un tipus de planta que es caracteritza per assolir grans
alçades.

#### Mètodes

* `Llavor creix()`: l'*Altibus* creix com qualsevol *Planta*, però a més,
si la seva alçada és superior a 7, deixa anar una llavor, que quan germini
donarà lloc a una nova *Altibus*.

* `char getChar(int nivell)`: el tronc de l'*Altibus* és el caràcter "|"
(pipe) i la part més superior és el caràcter "O" (o majúscula).

### Classe Declinus

La *Declinus* és un tipus de planta que es caracteritza per fer-se petita
un cop ha assolit la seva alçada màxima.

#### Mètodes

* `Llavor creix()`: la *Declinus* té un ritme de creixement inferior a
l'habitual: només augmenta la seva alçada una de cada dues vegades que es
crida aquest mètode.

 A més, quan arriba a la seva alçada màxima, que és 4, comença a decréixer
 al mateix ritme, fins que arriba a tenir alçada de 0 i mor.

 A més, quan arriba a la seva alçada màxima, deixa anar una llavor. En total,
 deixa anar dues llavors en el seu cicle vital, una quan assoleix l'alçada
 màxima i l'altra just abans de començar a decréixer.

* `char getChar(int nivell)`: el tronc de la *Declinus* és el caràcter
":" (dos punts) i la part superior és el caràcter "\*" (asterisc).

### Classe Jardi

Aquesta classe representa una línia de sòl fèrtil.

#### Atributs

* `Planta jardi[]`: es la renglera de plantes. En cada posició hi pot
haver una planta, però també pot estar buida.

#### Mètodes

* Constructor: l'únic constructor d'aquesta classe rep la mida que tindrà
el jardí. En cas que la mida sigui negativa o 0, es suposarà que el jardí
té mida 10. El jardí comença sense plantes.

* `void temps()`: aquest mètode s'anirà cridant a mesura que passi el temps.
Per a cada planta del jardí, es realitzaran les següents accions:

 1. Es cridarà el mètode *creix* de la planta.

 2. Si el mètode *creix* ha retornat una llavor, s'utilitzarà el mètode
 *escampaLlavor* de la planta i la seva posició per saber on ha anat a parar
 la llavor. Aleshores, si l'espai del jardí on ha anat a parar la llavor
 està buit, s'afegirà la llavor en aquesta posició.

 3. Si el mètode *creix* ha retornat una planta, es posarà la planta
 retornada en el lloc de la planta original.

 4. Finalment, si la planta no està viva, es buidarà l'espai del jardí.

* `String toString()`: aquest mètode retornarà un *String* multilínia
(recorda que es pot posar el símbol \n en un String per indicar diverses
línies) que representarà un dibuix del jardí.

   Per fer-la, s'hauran d'anar recorrent les diferents posicions del jardí
 i les diferents alçades i esbrinar quins caràcters hi van amb el mètode
 *getChar* de les plantes. Podem suposar que l'alçada màxima d'un jardí
 sempre serà 10.

   L'última línia de la cadena estarà formada per caràcters "\_" (barra baixa)
 que representen el terra.

* `boolean plantaLlavor(Planta novaPlanta, int pos)`: aquest mètode posarà
la planta rebuda a la posició del jardí indicada, sempre que la posició
estigui lliure.

### Classe Principal

Aquesta classe iniciarà el programa seguint les següent directrius:

Es plantaran almenys una planta *Altibus* i una planta *Declinus* a un jardí.
Aleshores, es farà passar el temps mitjançant un buttó o enllaç, es mostrarà el jardí i s'esperarà que
l'usuari clique un altra buttó o enllaç per finalitzar.

Si es clica el finalitzar s'acabarà el programa. En cas contrari, es farà
passar més temps i es tornarà a mostrar el jardí, etc.

### Optatiu

Enriqueix el teu jardí amb un nou tipus de planta.
