### Egzamin

#### Zadanie 3

***a) Wyszukać w sieci interesujące dane zawierające co najmniej 1_000_000 rekordów/jsonów.***

Dane sciagnelam z: http://archive.ics.uci.edu/ml/datasets/Individual+household+electric+power+consumption
Sa to dane dotyczace poboru pradu w domach. Archiwum zawiera 2075259 pomiarow zebranych pomiedzy 2006 rokiem a 2010.
 
 
***b) Dane zapisać w bazie MongoDB.***


Używamy komendy mongoimport:
``` sh
mongoimport --db moja --collection powerColl --type csv --headerline < household_power_consumption.txt 

```

Sprawdzamy czy import przeszedl poprawnie:
``` sh
db.powerColl.count()
2075259
```

Przykladowy rekord:

``` sh
db.powerColl.findOne(); 
```

![rek1](https://github.com/mkrajnik/nosql/blob/master/zdjecia/findOne.png)

***c) Przygotować w JavaScript co najmniej cztery agregacje korzystające z Aggregation Pipeline.***
  
#####  1)
*5 najwiekszych zuzyc pradu w 2 taryfie:*

``` sh

db.powerColl.aggregate([ 
  { $group: {_id: {"Time": "$Time"}, count: {$sum: "$Sub_metering_2"}}},
  { $sort : {count : -1}},
  { $limit: 5}
])
```

![r2](https://github.com/mkrajnik/nosql/blob/master/zdjecia/agregation1.png)

#####  2)
*5 Dat kiedy srednie zuzycie pradu na minute bylo wieksze niz 20 V*

``` sh

db.powerColl.aggregate([ 
 
{ $match: {Voltage: {$gte: 20}} }, 
  { $project : { _id: 0 ,Date : 1 , Voltage: 1 } },
  { $sort : {Voltage : -1}},
  { $limit: 5}
])
```
![ag2](https://github.com/mkrajnik/nosql/blob/master/zdjecia/ag2.png)

##### 3)
*5 godzin w których taryfa 3 jest najbardziej używana:*



``` sh
db.powerColl.aggregate([  
{$project : {
    hour: {$substr : ["$Time", 0, 2]},
    sub: {"Sub_metering_3": "$Sub_metering_3"}
  }} ,
{ $group: {_id: {"hour": "$hour", subd : "$sub.Sub_metering_3"}}},
 { $group: {_id: "$_id.hour", count: {$sum: "$_id.subd"}}},
  { $sort : {count : -1}},
  { $limit: 5}
])

```

![agg3](https://github.com/mkrajnik/nosql/blob/master/zdjecia/ag3.png)


##### 4)
*Ile wynosilo najwieksze zuzycie pradu (lacznie) w dniu 4 luty 2010*

``` sh


db.powerColl.aggregate([ 
  { $match: { "Date" : "4/2/2010" } },
  { $project : { _id: 0 ,Date : 1 , wynik: { $add: ["$Sub_metering_3","$Sub_metering_2", "$Sub_metering_1"] } } },
  { $sort : {wynik : -1}},
  { $limit: 5}
])
``` 
![ag4](https://github.com/mkrajnik/nosql/blob/master/zdjecia/agregation2.png)


***4. Zaprogramować i wykonać wszystkie agregacje z pkt. 3 w_innym niż JavaScript języku programowania. Skorzystać z jednego z driverów wymienionych na stronie MongoDB Drivers.***

### Python

##### 1)
*5 najwiekszych zuzyc pradu w 2 taryfie:*


``` python
import pymongo
import json
from bson.son import SON
from pymongo import MongoClient
client = MongoClient()

db = client['moja']
collection = db['powerColl']

pipeline = [
  { "$group": {"_id": {"Time": "$Time"}, "count": {"$sum": "$Sub_metering_2"}}},
  { "$sort" : {"count" : -1}},
  { "$limit": 5}
]
 

zapytanie = db.powerColl.aggregate(pipeline)
for doc in zapytanie:
   print(doc)
```

![p1](https://github.com/mkrajnik/nosql/blob/master/zdjecia/python11.png)








##### 2)
*5 Dat kiedy srednie zuzycie pradu na minute bylo wieksze niz 20 V*

``` python
import pymongo
import json
from bson.son import SON
from pymongo import MongoClient
client = MongoClient()

db = client['moja']
collection = db['powerColl']

pipeline = [
  { "$match": { "Voltage": {"$gte": 20}} }, 
  { "$project" : { "_id": 0 , "Date" : 1 , "Voltage": 1 } },
  { "$sort" : {="Voltage" : -1}},
  { "$limit": 5}
]
 

zapytanie = db.powerColl.aggregate(pipeline)
for doc in zapytanie:
   print(doc)
```    

 

![p2](https://github.com/mkrajnik/nosql/blob/master/zdjecia/python1.png)







##### 3)
*5 godzin w których taryfa 3 jest najbardziej używana:*

``` python
import pymongo
import json
from bson.son import SON
from pymongo import MongoClient
client = MongoClient()

db = client['moja']
collection = db['powerColl']

pipeline = [ 
{"$project" : {
   "hour": {"$substr" : ["$Time", 0, 2]},
   "sub": {"Sub_metering_3": "$Sub_metering_3"}
  }} ,
 { "$group": {"_id": {"hour": "$hour", "subd" : "$sub.Sub_metering_3"}}},
 { "$group": {"_id": "$_id.hour", "count": {"$sum": "$_id.subd"}}},
 { "$sort" : {"count" : -1}},
 { "$limit": 5}
]
 

zapytanie = db.powerColl.aggregate(pipeline)
for doc in zapytanie:
   print(doc)

```
![p3](https://github.com/mkrajnik/nosql/blob/master/zdjecia/python4.png)


 
 
