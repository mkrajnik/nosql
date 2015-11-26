# Nosql - zaliczenie

## Zadanie 2a

### Parametry komputera:

| Producent | Dell |
|---|---|
| Model | Optiplex 9030 |
| Procesor | Intel Core i5-4590S @ 3.00 GHz |
| Dysk | 512 GB |
| Pamiec RAM | 8 GB | 
| Typ systemu  | 64-bit |
| Mongo | 3.0.7 |
| Postgres | 9.4.5 |
| Python | 3.5.0 |

### Plik z danymi:

Plik RC_2015-01.bz2 zajmuje 5,07 GB (bajtów: 5 452 413 560). Wszystkich komentarzy/JSON-ów jest 53 851 542.


### Rozpakowanie pliku

Użyty program *7zip* 

Czas: *13 minut*

Miejsce zajete po rozpakowaniu: *29,4 GB (bajtów: 31 648 374 104)*


### Import Mongodb

Czas : 33 minuty

#### Screeny z importu:

![zdjecie1](https://github.com/mkrajnik/nosql/blob/master/mongo1.png)

![zdjecie2](https://github.com/mkrajnik/nosql/blob/master/mongo2.png)

![zdjecie3](https://github.com/mkrajnik/nosql/blob/master/mongo3.png)

![zdjecie4](https://github.com/mkrajnik/nosql/blob/master/mongo4.png)

![zdjecie5](https://github.com/mkrajnik/nosql/blob/master/mongo5.png)

![zdjecie6](https://github.com/mkrajnik/nosql/blob/master/mongo6.png)


#### Podsumowanie
Mongodb korzystalo z wszystkich procesorow jednoczesnie, dodatkowo nie zuzywalo przez caly czas trwania importu caly czas procesora i dysku na 100%. Korzystalo naprzemiennie z dysku / procesora. Pamiec poczatkowo rosla, az w koncu stanela na granicy 95-97 %.


### Import Postgres

Do importu Postgres musimy przygotować plik, trzeba zamienić apostrofy na inny znak np. spacje. Wtedy dopiero poprawnie zaimportuje sie do bazy.
W PowerShellu używam polecenia:

get-content somefile.txt | %{$_ -replace "expression","replace"}

