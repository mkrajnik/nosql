# Martyna Krajnik, mkrajnik94@gmail.com
# Nosql - zaliczenie
Rozwiązania zadań z przedmiotu *Bazy NoSQL*:

1. [Zaliczenie](zaliczenie.md):
 - [ ] Neo4j
 - [x] EDA
1. [Egzamin](egzamin.md)
 - [ ] Aggregation Pipeline
 - [ ] MapReduce



## Zadanie 2a

### Parametry komputera:

| Producent | Dell |
|---|---|
| Model | Optiplex 9030 |
| Procesor | Intel Core i5-4590S @ 3.00 GHz |
| Dysk | 512 GB |
| Pamiec RAM | 8 GB | 
| System  | Windows 10 |
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


### Import 

|--| Mongodb | Postgresql |
|---|---|---|
|Czas | 33 minuty | 45 minut|
|Zużycie zasobów | Mongodb korzystalo z wszystkich procesorow jednoczesnie, dodatkowo nie zuzywalo przez caly czas trwania importu caly czas procesora i dysku na 100%. Korzystalo naprzemiennie z dysku / procesora. Pamiec poczatkowo rosla, az w koncu stanela na granicy 95-97 %. |  -- |
| Łatwosc importu | Używamy jednego polecenia | Trzeba plik przeparsować, a potem za pomoca skryptu wrzucic do bazy. |
| Korzystanie z danych | Kazdy komentarz ma swoje "atrybuty" z ktorych mozemy korzystać | Wszystkie dane sa wrzucone do jednej kolumny typu data json|




#### Import - Mongodb - szczególy

![zdjecie1](https://github.com/mkrajnik/nosql/blob/master/zdjecia/mongo1.png)

Używamy komendy mongoimport do importu



![zdjecie2](https://github.com/mkrajnik/nosql/blob/master/zdjecia/mongo2.png)

Jak widać, Mongo naprzemiennie korzysta z dysku/procesora. Nie wykorzystuje caly czas w 100% wszystkich zasobów.


![zdjecie3](https://github.com/mkrajnik/nosql/blob/master/zdjecia/mongo3.png)

Podczas trawnia importu - Mongo wykorzystuje cala pamiec dostepna na komputerze, co oznacza, ze podczas trwania importu najlepiej odejsc od komputera i zrobic sobie kawe... Bo korzystanie z niego podczas importu jest strasznie uciazliwe. Tak jak wczesniej bylo widac, dysk/procesor dziala naprzemiennie - przez caly import.

![zdjecie5](https://github.com/mkrajnik/nosql/blob/master/zdjecia/mongo5.png)

Jeszcze spojrzmy jak Mongo wykorzystuje procesory... Wielki + dla Mongo - wykorzystuje równomiernie procesory dostepne w komputzerze, nie obciaza tylko jednego.


![zdjecie6](https://github.com/mkrajnik/nosql/blob/master/zdjecia/mongo6.png)

Import zakonczony powodzeniem, zaimportowano wszystkie rekordy 


### Import Postgres

Do importu Postgres musimy przygotować plik, trzeba zamienić apostrofy na inny znak np. spacje. Wtedy dopiero poprawnie zaimportuje sie do bazy.
W PowerShellu używam polecenia:

``` get-content .\RC_2015-01 | %{$_ -replace "'"," "} > RC_2015-011 ```

Teraz w Postgresie tworzymy tabele:


``` sql
CREATE TABLE subreddits(
	id serial primary key,
	data json
);
```

Za pomoca skryptu w pythonie importujemy baze:

``` python 

import psycopg2

def insert():
  plik = "/mnt/vb/RC_2015-01.json"
  conn = psycopg2.connect(
  database="moja",
  user="postgres",
  host="/tmp/",
  password="123456"
)
    cursor = conn.cursor()
    i = 1
    with open(plik, "rb") as file:
        for a in file:
            cursor.execute("INSERT INTO subreddits (data) VALUES ('%s')" % (a))
            i=i+1
            if  i % 10000 == 0:
                conn.commit()
    conn.commit()
    cursor.close()
    conn.close()

insert()


```


Zużycie zasobów:

![zdjecie-post1](https://github.com/mkrajnik/nosql/blob/master/zdjecia/mongo-postgres2.png)

Postgres dziala podobnie jak Mongo - korzysta naprzemiennie z dysku/procesora, ale pamiec nie jest wykorzystywana w dokladnie 100%.

![zdjecie-post2](https://github.com/mkrajnik/nosql/blob/master/zdjecia/mongo-postgres1.png)

Postgres równomiernie rozklada prace na kazdy z procesorow


Przykladowy rekord:

![zdjecie-posrt2](https://github.com/mkrajnik/nosql/blob/master/zdjecia/mongo-postgres3.png)

## Zadanie 2b

### Zliczenie rekordów

####  MongoDB

```  
mydb.myColl.count();
```
![zdjecie7](https://github.com/mkrajnik/nosql/blob/master/zdjecia/mongo7.png)

#### Postgres

``` sql
select count(*) from subreddits ; 
```

## Zadanie 2c 

### Zapytania

->  Ile jest zarchiwizowanych plików

``` sql
db.myColl.find({ "archived":"true" }).count();
```

![zdjecie10](https://github.com/mkrajnik/nosql/blob/master/zdjecia/mongo10.png)

Jak widać... nie archiwizuja ;)

->  Przykladowy rekord

``` sql

db.myColl.findOne();

```

![zdjecie9](https://github.com/mkrajnik/nosql/blob/master/zdjecia/mongo9.png)

-> Suma punktów autora YoungModern 

``` sql
db.myColl.aggregate ([
  { $match: { "author": "YoungModern"  } },
  {  $group: { _id: "$author", total: { $sum: "$score"  } } }
])
```
![zdjecie11](https://github.com/mkrajnik/nosql/blob/master/zdjecia/mongo11.png)


### Zadanie 2d - Geojson

Przed pisaniem zapytań do bazy dopisany zostal indeks:

![geo1](https://github.com/mkrajnik/nosql/blob/master/zdjecia/geo1.png)


1. Miejscowosci oddalone o 10 km od Bolszewa: 
``` bash
> db.miasta.find({loc: {$near: {$geometry: {type: "Point", coordinates: [18.17585,54.618011]}, $maxDistance: 10000}}}).skip(1)
```


[![mapa1](https://github.com/mkrajnik/nosql/blob/master/zdjecia/geo2.png)](https://github.com/mkrajnik/nosql/blob/master/mapa/mapa1.geojson)

2. Miejscowosci ktore maja w nazwie "Nowa"

Dodajemy indeks na pole "name" zeby w nim przeszukiwac:

``` bash
db.miasta.createIndex( { name: "text" } )
```

**Zapytanie:**

``` bash
db.miasta.find( { $text: { $search: "Nowa" } } )
```



3. LineString z Wejherowa do Gdańska
``` bash
db.miasta.find({"name":"Gdańsk"})
db.miasta.find({"name":"Reda"})
db.miasta.find({"name":"Rumia"})
db.miasta.find({"name":"Gdynia"})
db.miasta.find({"name":"Sopot"})
```

[![mapa3](https://github.com/mkrajnik/nosql/blob/master/zdjecia/geo3.png)](https://github.com/mkrajnik/nosql/blob/master/mapa/mapa2.geojson)

