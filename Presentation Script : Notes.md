# Präsentation

### Was ist MongoDB?

- MongoDB verwaltet eine Sammlung von JSON Dokumenten
- Entwicklung begann 2007 und die Erste Veröffentlichung war 2009
- MongoDB ist die am weitesten verbreitete NoSQL Databse
- NoSQL steht nicht für NoSQL sondern für Not Only SQL
- Ziel ist eben mit MongoDB den relationalen Ansatz zu brechen
- Verschiedene Möglichkeiten mit MongoDB zu interagieren: Shell, Compass, Driver, weitere Drittanbieter Tools
- MongoDB geht in die Richtung von In Memory Datenbanken, weil vieles im Ram stattfindet --> sehr memory lasting
- MongoDB ist von Grund auf modern gebaut. Aufteilung auf mehrere Maschinen. Horizontale Skalierung mit der Erstellung von Clustern
- Kann auch in Kubernetes betrieben werden oder als Container 
- Daher sowohl Ausfallsicherheit als auch schnelle Skalierung möglich und daher auch gute Performance



### Aufbau von JSON Dokumenten

- Mit JSON lassen sich Boolsche Werte, Zahlen, Strings, Arrays und Objekte verwendet werden
- JSON besitzt ein festes Schema aus Key und Value Paaren
- Key selbst ist ein String 
- Der Wert kann einer der genannten Typen sein
- Der Key sollte eindeutig sein also nicht doppelt vorkommen, da es sonst beim verarbeiten zu Problemen kommen kann



### Aufbau von MongoDB

- Client interagiert mit mongos --> ist das Interface zwischen dem Client und dem Cluster.
- Mongos arbeitet wie ein router und verteilt die Anfragen --> kann wie ein Controller gesehen werden
- Config Server speichert Meta Daten und die Configuration des Clusters --> Daten über jedes einzelne Shard
- Außerdem speichert der Config Server Daten zur Authentifizierung 
- Jedes Cluster braucht mindestens 1 Config Server 
- MongoDB verteilt sowohl die Daten als auch Arbeitslast auf einzelne Shards 
- Wobei kleinere Datenbanken nur im primary shard laufen und wenn diese größer wird, so teilt MongoDB die Datenbank in Blöcke auf mehrere sekundäre shards auf
- Shard ist eine einzelne MongoDB Instanz
- Innerhalb der Shards läuft der mongod Service der die eigentliche Datenbank bereitstellt

### RDBMS vs. MongoDB 

- MongoDB arbeitet nicht mit Tabellen --> Zeilen und Spalten wie beim relationalen Ansatz
- Bei beiden Ansätzen befinden sich die Daten in einer Datenbank
- Beim relationalen Ansatz befinden sich innerhalb dieser Datenbanken Tabellen. Bei NoSQL befinden sich innerhalb der Datenbank eine oder mehrere Collections 
- Innerhalb der Tabelle befinden sich Zeilen (rows) und spalten (columns). Bei MongoDB sind es Dokumente und Felder
- Ein Datensatz in MySQL ist eine Zeile der Tabelle, bei MongoDB wäre es entsprechend ein Document also eine Menge an Key Value Paaren die in Form eines JSON Objekts vorliegt
- Syntax ist anders als bei MySQL aber von der Semantik gibt es durchaus parallelen

# MongoDB Server Installation

```
1. wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add 

2. echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list

3. sudo apt-get update

4. sudo apt-get install -y mongodb-org

6. sudo systemctl start mongod

7. sudo systemctl status mongod --> Datenbank läuft

Jedoch nur Localhost erreichbar! Deswegen an jedes Interface binden

8. vim /etc/mongod.conf

bindIpAll: true

9. sudo systemctl restart mongod

Shell ausprobieren und verbinden!

Admin Account anlegen!

10. mongo

11. use admin

12. db.createUser(
  {
    user: "admin",
    pwd:  passwordPrompt(),
    roles: [ { role: "root", db: "admin" }]
  }
)

13. sudo systemctl stop mongod

14. vim /etc/mongod.conf

security:
    authorization: enabled
    
15. sudo systemctl start mongod

16. sudo systemctl status mongod

17. mongo --> geht nicht keine dbs werden angezeigt

18. mongo -u admin -p admin

19. use eportfolio --> db anlegen 

20. db.createUser(
  {
    user: "dbUser",
    pwd:  passwordPrompt(),
    roles: [ { role: "dbOwner", db: "eportfolio" }]
  }
)

21. mongo -u dbUser -p dbuser --authenticationDatabase eportfolio

22. use eportfolio

```

#### Zurück zur Präsentation!

- Befehle durchgehen
- One Befehle funktionieren nach dem First Match Prinzip

```
Zuerst Basic Insert Zeigen später dann richtiger Import

1. db.flights.insertOne({
    "departureAirport": "QUL",
    "arrivalAirport": "TYN",
    "aircraft": "Airbus A300",
    "distance": 2090,
    "intercontinental": true,
    "pilot": "Stapylton",
    "timezone": "America/Argentina/Cordoba"
  })
 2. db.flights.find()
 
3. db.flights.insertMany([{
    "departureAirport": "MBU",
    "arrivalAirport": "NAW",
    "aircraft": "Airbus A320",
    "distance": 2487,
    "intercontinental": true,
    "pilot": "Smithend",
    "timezone": "Asia/Manila"
  },
  {
    "departureAirport": "MBU",
    "arrivalAirport": "GND",
    "aircraft": "Airbus A319",
    "distance": 1879,
    "intercontinental": true,
    "pilot": "Wincom",
    "timezone": "Asia/Shanghai"
  }])
  
  4. db.flights.find({ aircraft: "Airbus A320"})
  5. db.flights.find({ departureAirport: "MBU"})
  6. db.flights.find({ departureAirport: "MBU", arrivalAirport: "NAW"})
  7. db.flights.findOne({ departureAirport: "MBU"}) --> nur eins wird angezeigt first match
  
 Nun wegen des richtigen Imports die collection löschen!
  db.flights.drop()
  exit
  
Test Dateien Importieren Entweder über den Compass importieren oder über die Shell:

mongoimport flights.json -d eportfolio -c flights --jsonArray -u dbUser -p dbuser --authenticationDatabase eportfolio

mongoimport airports.json -d eportfolio -c airports --jsonArray -u dbUser -p dbuser --authenticationDatabase eportfolio

mongoimport passengers.json -d eportfolio -c passengers --jsonArray -u dbUser -p dbuser --authenticationDatabase eportfolio

1. db.passengers.updateOne({ _id: ObjectId("60abe65f5afdb1357e8c5cc0")}, {$set: { valid_ticket: true}})
2. db.passengers.find({ _id: ObjectId("60abe65f5afdb1357e8c5cc0")}).pretty() --> zeigen das es geändert wurde

Corona Tested Einfügen:

1. db.passengers.updateMany({}, { $set: {negative_covid_tested: false}})
2. db.passengers.find().pretty() --> jeder hat jetzt das Covid Feld
3. db.passengers.find({$and: [{valid_ticket:true}, {negative_covid_tested: true}]})

replace macht das gleiche wie update wobei das object mit dem update komplett überschrieben wird

db.passengers.deleteOne({ _id: ObjectId("60abe65f5afdb1357e8c5cc0")}) --> genau die Person gelöscht bei der das Ticket auf true gesetzt wurde

Theoretisch kann die ganze Collection mit deleteMany gelöscht werden
db.passengers.deleteMany({}) --> zeigen aber nicht ausführen
 
```

#### Projektion

Vielleicht möchte man bei einer find Abfrage nicht alle key value paare zurückbekommen.Damit man diese nicht unnötigerweise abfrägt und sie dann nicht weiter verwendet kann man dies mit einer Projektion filtern

```
db.airports.find({}, {code: 1}) --> Alle Kürzel der Flughäfen ausgeben
db.airports.find({}, {code: 1, _id: 0}) --> Id ausblenden
db.airports.find({ tz: "America/Denver"}, {code: 1, _id: 0})
mit limit(x) limitieren
```

Jetzt stellt sich die Frage wie kann ich die Informationen zu den Flughäfen mit den Flügen verbinden? Lösung: Aggregation 

```
db.flights.aggregate([{$lookup: {from: "airports", localField: "departureAirport", foreignField: "code", as: "departureAirport_details"}}]).pretty()

Pipelining möglich: 

db.flights.aggregate([{$lookup: {from: "airports", localField: "departureAirport", foreignField: "code", as: "departureAirport_details"}}, {$lookup: {from: "airports", localField: "arrivalAirport", foreignField: "code", as: "arrivalAirport_details"}}]).pretty()
```

### ENDE

#### Optional: Schema Validation

Dazu eine neue Collection anlegen mit Validation

```

db.createCollection('passengers_validation', {
  validator: {
    $jsonSchema: {
      bsonType: 'object',
      required: ['first_name', 'last_name', 'email', 'phone_number', 'passport_id', 'valid_ticket', 'negative_covid_tested'],
      properties: {
        first_name: {
          bsonType: 'string',
          description: 'must be a string!'
        },
        last_name: {
          bsonType: 'string',
          description: 'must be a string!'
        },
        email: {
          bsonType: 'string',
          description: 'must be a string!'
        },
        phone_number: {
          bsonType: 'string',
          description: 'must be a string!'
        },
        passport_id: {
          bsonType: 'string',
          description: 'must be a number!'
        },
        valid_ticket: {
          bsonType: 'bool',
          description: 'must be a true or false!'
        },
        negative_covid_tested: {
          bsonType: 'bool',
          description: 'must be a true or false!'
            }
        }
      }

  }
});

Dann passenger einfügen der aber nicht dem vorgegebenen Schema entspricht da er kein negative_covid_tested hat:

db.passengers_validation.insertOne({
    "departureAirport": "QUL",
    "arrivalAirport": "TYN",
    "aircraft": "Airbus A300",
    "distance": 2090,
    "intercontinental": true,
    "pilot": "Stapylton",
    "timezone": "America/Argentina/Cordoba"
  })
  
  --> funktioniert nicht
  db.passengers_validation.insertOne({
    "first_name": "Worthy",
    "last_name": "Kenningley",
    "email": "wkenningley0@canalblog.com",
    "phone_number": "+48 (271) 913-4834",
    "passport_id": "1267858168",
    "valid_ticket": true,
    "negative_covid_tested": true
  })
  
    db.passengers_validation.insertOne({
    "first_name": "Worthy",
    "last_name": "Kenningley",
    "email": "wkenningley0@canalblog.com",
    "phone_number": "+48 (271) 913-4834",
    "passport_id": "1267858168",
    "valid_ticket": true
  })
```

