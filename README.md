# Nosql - zaliczenie

## Zadanie 1

### Parametry komputera:

| Producent | Dell |
|---|---|
| Model | Optiplex 9030 |
| Procesor | Intel Core i5-4590S @ 3.00 GHz |
| Dysk | 512 GB |
| Pamiec RAM | 8 GB | 
| Typ systemu  | 64-bit |

### Plik z danymi:

Plik RC_2015-01.bz2 zajmuje 5,07 GB (bajtów: 5 452 413 560). Wszystkich komentarzy/JSON-ów jest 53 851 542.


### Rozpakowanie pliku

Użyty program *7zip* 

Czas: *13 minut*

Miejsce zajete po rozpakowaniu: *29,4 GB (bajtów: 31 648 374 104)*


### Import Mongodb

Czas : 33 minuty

Poczatek importu:
![zdjecie1](https://github.com/mkrajnik/nosql/blob/master/mongo1.png)

![zdjecie2](https://github.com/mkrajnik/nosql/blob/master/mongo2.png)

![zdjecie3](https://github.com/mkrajnik/nosql/blob/master/mongo3.png)

![zdjecie4](https://github.com/mkrajnik/nosql/blob/master/mongo4.png)


Jak widać mongoimport naprzemiennie korzystal z procesora/dysku twardego.


![zdjecie5](https://github.com/mkrajnik/nosql/blob/master/mongo5.png)

Ale praca byla podzielona rownomiernie na kazdy procesor.

![zdjecie6](https://github.com/mkrajnik/nosql/blob/master/mongo6.png)

Koniec importu, zaimportowano wszystkie dokumenty.
