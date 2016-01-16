### Egzamin

#### Zadanie 3

*1. Wyszukać w sieci interesujące dane zawierające co najmniej 1_000_000 rekordów/jsonów.*

Dane sciagnelam z: http://archive.ics.uci.edu/ml/datasets/Individual+household+electric+power+consumption
Sa to dane dotyczace poboru pradu w domach. Archiwum zawiera 2075259 pomiarow zebranych pomiedzy 2006 rokiem a 2010.
 
2. Dane zapisać w bazie MongoDB.

Używamy komendy mongoimport:
``` sh
mongoimport --db moja --collection powerColl

```

3. Przygotować w JavaScript co najmniej cztery agregacje korzystające z Aggregation Pipeline.

4. Zaprogramować i wykonać wszystkie agregacje z pkt. 3 w_innym niż JavaScript języku programowania. Skorzystać z jednego z driverów wymienionych na stronie MongoDB Drivers.
