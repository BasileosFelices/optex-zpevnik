# Makra pro vytvoření zpěvníčku s akordy #

Tato práce vznikla v rámci předmětu BI-TEX na FIT ČVUT. Jejím obsahem jsou makra umožňující vytvoření jednoduchého zpěvníčku z akordy pro libovolné strunné nástroje.

## Obsah repozitáře ##

* zpevnik-makra.tex - soubor s makry
* zpevnik.tex - příklad použití maker
* pisnicky/*.tex - adresář obsahující jednotlivé písně
* drawchord/*.tex - adresář obsahující soubory potřebné pro vykreslování hmatníku.
    * drawchord/typesetchords.tex - Definice output rutiny potřebná k sázení na okraj
    * drawchord/{NASTROJ}.tex - Makra na kresbu hmatníku uvedeného nástroje.
    * drawchord/{NASTROJ}DICT.tex - Slovník hmatů pro uvedený nástroj.
* zpevnik.pdf - příklad vygenerovaného zpěvníku

## Použití maker ##

### Sazba zpěvníku ###

Nejprve je potřeba zavést makra pomocí příkazu:
```
#!tex
input zpevnik-makra
```

Pro vygenerování úvodní strany lze použít maker \booktitle a \bookauthor:

```
#!tex
\booktitle Titulek zpěvníku zakončený prázdnou řádkou

\bookauthor Jméno autora zakončené prázdnou řádkou

```

Vlastní obsah zpěvníku musí být uvozen makrem \songs. Jednotlivé písničky mohou být vloženy pomocí příkazu \input.

```
#!tex
\songs
\input pisnicky/all-my-loving
\input pisnicky/dont_worry
\input pisnicky/gronska_pisnicka
\input pisnicky/fajn-dzob
```

Pro vložení obsahu je možné použít makro \readtoc.

Soubor je potřeba ukončit pomocí \bye.


### Sazba písniček ###
Každá píseň musí začít voláním makra \beginsong, které následuje volání makra \title a \author.

```
#!tex
\begin song

\title{All my loving}
\author{The Beatles}

```

Předehru je možné vložit pomocí \intro <Jména akordů zakončená prázdným řádkem>:

```
#!tex
\intro C Dmi F C

```


Každá sloka by měla být uvozena makry \beginverse a \endverse.

Akordy lze vkládat pomocí makra \chord{<název akordu>}, které vložíte před znak, nad kterým se má akord objevit. Pokud má akord obsahovat křížek, je nutné ho zapsat tímto způsobem \\#.

Pokud se v písničce nějaká část sloky opakuje, je možné vložit repetici. K tomu stačí opakující se část vložit mezi znaky |: a :|. Za :| je možné vložit číslo vyjadřující počet opakování, pak by mělo následovat odřádkování.

Pro vložení refrénu byla vytvořena makra \beginrefrain a \endrefrain, které fungují podobně jako \beginverse a \endverse.

Pro vložení značky Ref. je možné využít \refrain


```
#!tex

\beginsong

\title{Fajn džob}

\beginverse
Moje \chord{Gmi}briga má \chord{F}jméno \chord{Gmi}Ariel,
řek' kapitán, \chord{F}když mě \chord{Gmi}zval.
|:\chord{B}Prej abych s ním jako \chord{F}lodník jel
a ten \chord{Gmi}džob co mi \chord{F}nabíd', \chord{Gmi}bral.:|
\endverse

\beginverse
Von velký prachy mi sliboval,
čert ví, jestli náký má.
|:Já nevěřil a tak žvanil dál,
že zejtra už vyplouvá.:|
\endverse

\beginrefrain
\chord{G}Celej den a \chord{C}celou \chord{G}noc
\chord{Emi}stožár se pode mnou \chord{D}kej\chord{G}vá,
chceš to znát? Tak \chord{C}klidně \chord{D}pojď,
\chord{Emi}ať víš jaký to \chord{D}bej\chord{G}vá.

Žaludek stoupá \chord{C}vejš a \chord{G}vejš,
\chord{Emi}na lodi všechno \chord{D}skří\chord{G}pá,
do očí tak jako \chord{C}mořská \chord{G}sůl
\chord{Emi}vostrej vítr tě \chord{D}ští\chord{G}pá.\chord{Gmi}
\endrefrain

\beginverse
Kdyby do vočí vám někdo tvrdit chtěl,
že ta práce pro vás je fajn,
|:dejte si říct, to by bral jenom ten
co je blázen a nemá šajn.:|
\endverse

\refrain
```

### Vykreslování akordů ###
Vykresování akordů se aktivuje deklarací toho, jaký nástroj chceme používat. To uděláme v hlavním souboru zpěvníku (zpevnik.tex) pomocí příkazu

```
#!tex

\include drawchord/{NASTROJ}
```
kde *NASTROJ* může (zatím) nabývat hodnot 'ukulele' nebo 'guitar'.

Vypisovat všechny akordy v písni obrázkami hmatníku je z hlediska užití zpěvníku špatně. Lehké akordy se vyskytují ve všech písničkách a tak bychom je kreslili na každou stránku i přesto, že je muzikant už zná. Sám muzikant nejlépe ví které akordy zná a které ne. Proto necháme rozhodnutí o tom, zda akord vykreslit či ne, na něm.

Když se rozhodneme, že akord je těžký a chceme ho vypsat tak jednoduše připíšeme makro **\chord** na **\drawchord**. Podle toho jaký nástroj jsme zvolili se vyhledá hmat v přílišeném slovníku hmatů a na pravý okraj stránky se hmatník vykreslí.

Pokud akord nebude ve slovníku nalezen tak se vykreslí prázdný hmatník a před jménem akordu se napíše otazník.


## Vygenerování pdf ##
Pro vygenerování pdf zpěvníku použijte

```
#!tex

pdfcsplain <nazev>
```